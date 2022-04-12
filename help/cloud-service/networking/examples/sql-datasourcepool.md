---
title: 使用JDBC DataSourcePool的SQL連接
description: 瞭解如何使用JDBC DataSourcePool和AEM出口端從AEMas a Cloud Service連接到SQL資料庫。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# 使用JDBC DataSourcePool的SQL連接

必須將到SQL資料庫（和其他非HTTP/HTTPS服務）的連接代理AEM出，包括使用AEMDataSourcePool OSGi服務來管理連接的連接。

## 高級網路支援

以下高級網路選項支援以下代碼示例。

確保 [適當](../advanced-networking.md#advanced-networking) 本教程之後，高級網路配置已設定完畢。

| 無高級網路 | [靈活的埠出口](../flexible-port-egress.md) | [專用出口IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

OSGi配置的連接字串使用：

+ `AEM_PROXY_HOST` 值 [OSGi配置環境變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 作為連接的主機
+ `30001` 即 `portOrig` Cloud Manager埠正向映射的值 `30001` → `mysql.example.com:3306`

由於機密不能儲存在代碼中，因此最好通過OSGi配置變數（使用AIO CLI或Cloud Manager API設定）提供SQL連接的用戶名和密碼。

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

以下 `aio CLI` 命令可用於根據每個環境設定OSGi機密：

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 代碼示例

此Java™代碼示例是OSGi服務，該服務通過AEMDataSourcePool OSGi服務連接到外部MySQL資料庫。
DataSourcePool OSGi工廠配置依次指定埠(`30001`)的 `portForwards` 規則 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作到外部主機和埠， `mysql.example.com:3306`。

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
    }
}
```

## MySQL驅動程式依賴項

AEMas a Cloud Service通常要求您提供Java™資料庫驅動程式以支援連接。 通常，通過將包含這些驅動程式的OSGi捆綁對象嵌入到項目中，最AEM好地提供驅動程式 `all` 檔案。

### 反應器pom.xml

在反應器中包括資料庫驅動程式依賴項 `pom.xml` 然後把它們引入 `all` 子項目。

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## 所有pom.xml

將資料庫驅動程式依賴項對象嵌入 `all` 包已部署並可在AEMas a Cloud Service上使用。 這些藏物 __必須__ 是導出資料庫驅動程式Java™類的OSGi捆綁包。

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
