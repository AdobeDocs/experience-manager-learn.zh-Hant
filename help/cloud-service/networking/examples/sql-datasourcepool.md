---
title: 使用JDBC DataSourcePool的SQL連線
description: 瞭解如何使用AEM的JDBC DataSourcePool和輸出連線埠，從AEM as a Cloud Service連線到SQL資料庫。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 117
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---

# 使用JDBC DataSourcePool的SQL連線

與SQL資料庫（以及其他非HTTP/HTTPS服務）的連線必須從AEM使用代理連線，包括使用AEM的DataSourcePool OSGi服務來管理連線的連線。

## 進階網路支援

下列進階網路選項支援下列程式碼範例。

在執行本教學課程之前，請確定已設定[適當的](../advanced-networking.md#advanced-networking)進階網路設定。

| 沒有進階網路 | [彈性連線埠輸出](../flexible-port-egress.md) | [專用輸出IP位址](../dedicated-egress-ip-address.md) | [虛擬私人網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi設定

OSGi設定的連線字串使用：

+ 透過`AEM_PROXY_HOST`OSGi設定環境變數[&#x200B; &#x200B;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values)將`$[env:AEM_PROXY_HOST;default=proxy.tunnel]`值作為連線的主機
+ `30001`是Cloud Manager連線埠轉寄對應`portOrig`→`30001`的`mysql.example.com:3306`值

由於密碼不得儲存在程式碼中，因此最好透過OSGi設定變數、使用AIO CLI或Cloud Manager API來設定SQL連線的使用者名稱和密碼。

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

以下`aio CLI`命令可用於根據每個環境設定OSGi秘密：

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 程式碼範例

此Java™程式碼範例屬於OSGi服務，透過AEM的DataSourcePool OSGi服務連線至外部MySQL資料庫。
DataSourcePool OSGi Factory設定接著會指定透過`30001`enableEnvironmentAdvancedNetworkingConfiguration`portForwards`作業中的[規則對應到外部主機和連線埠](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration)的連線埠(`mysql.example.com:3306`)。

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

## MySQL驅動程式相依性

AEM as a Cloud Service通常需要您提供Java™資料庫驅動程式來支援連線。 提供驅動程式的最佳作法通常是透過`all`套件將包含這些驅動程式的OSGi套件成品內嵌至AEM專案。

### Reactor pom.xml

將資料庫驅動程式相依性包含在Reactor `pom.xml`中，然後在`all`子專案中參照它們。

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

將資料庫驅動程式相依性成品內嵌到`all`套件中，以部署這些成品，並可在AEM as a Cloud Service上使用。 這些成品&#x200B;__必須__&#x200B;是匯出資料庫驅動程式Java™類別的OSGi組合。

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
