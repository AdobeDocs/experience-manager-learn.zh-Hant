---
title: 使用Java™ API的SQL連接
description: 瞭解如何使用Java™ SQL API和AEM出口埠從as a Cloud Service連接到SQL資料庫。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9356
thumbnail: KT-9356.jpeg
exl-id: ec9d37cb-70b6-4414-a92b-3b84b3f458ab
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# 使用Java™ API的SQL連接

必須將到SQL資料庫（和其他非HTTP/HTTPS服務）的連接代理AEM出。

此規則的例外是 [專用出口ip地址](../dedicated-egress-ip-address.md) 正在使用，且服務在Adobe或Azure上。

## 高級網路支援

以下高級網路選項支援以下代碼示例。

確保 [適當](../advanced-networking.md#advanced-networking) 本教程之後，高級網路配置已設定完畢。

| 無高級網路 | [靈活的埠出口](../flexible-port-egress.md) | [專用出口IP地址](../dedicated-egress-ip-address.md) | [虛擬專用網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

由於機密不能儲存在代碼中，因此最好通過以下方式提供SQL連接的用戶名和密碼 [秘密OSGi配置變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values)，使用AIO CLI或Cloud Manager API設定。

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

以下 `aio CLI` 命令可用於根據每個環境設定OSGi機密：

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 代碼示例

此Java™代碼示例是OSGi服務，該服務通過以下雲管理器連接到外部SQL Server Web伺服器 `portForwards` 規則 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 的下界。

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/MySqlExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.sql.*;

@Component
@Designate(ocd = MySqlExternalServiceImpl.Config.class)
public class MySqlExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(MySqlExternalServiceImpl.class);

    // Get the proxy host using the AEM_PROXY_HOST Java environment variable provided by AEM as a Cloud Service
    private static final String PROXY_HOST = System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel");

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = 30001;

    private Config config;

    @Override
    public boolean isAccessible() {
        log.debug("MySQL connection URL: [ {} ]", getConnectionUrl(config));

        // Establish a connection with the external MySQL service
        // This MySQL connection URL is created in getConnection(..) which will use the AEM_PROXY_HOST is it exists, and the proxied port.
        try (Connection connection = DriverManager.getConnection(getConnectionUrl(config), config.username(), config.password())) {
            // Validate the connection
            connection.isValid(1000);

            // Close the connection, since this is just a simple connectivity check
            connection.close();

            // Return true if AEM could reach the external SQL service
            return true;
        } catch (SQLException e) {
            log.error("Unable to establish an connection with MySQL service using connection URL  [ {} ]", getConnectionUrl(config), e);
        }

        return false;
    }

    /**
     * Create a connection string to the MySQL using the AEM-provided AEM_PROXY_HOST and portForwards.portOrg port
     * defined in the Cloud Manager API mapping.
     *
     * @param config OSGi configuration object
     * @return the MySQL connection URI
     */
    private String getConnectionUrl(Config config) {
        return String.format("jdbc:mysql://%s:%d/wknd-examples", PROXY_HOST, PORT_FORWARDS_PORT_ORIG);
    }

    @Activate
    protected void activate(Config config) throws ClassNotFoundException, SQLException {
        this.config = config;

        // Load the required MySQL Driver class required for Java to make the connection
        // The OSGi bundle that contains this driver is deployed via the project's all project
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
    }

    @ObjectClassDefinition
    @interface Config {
        @AttributeDefinition(type = AttributeType.STRING)
        String username();

        @AttributeDefinition(type = AttributeType.STRING)
        String password();
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
