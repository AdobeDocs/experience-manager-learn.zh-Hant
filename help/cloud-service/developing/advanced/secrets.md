---
title: 在AEM as a Cloud Service中管理秘密
description: 瞭解在AEM as a Cloud Service中管理機密的最佳實務，使用AEM提供的工具和技術來保護您的敏感資訊，確保您的應用程式安全保密。
version: Experience Manager as a Cloud Service
topic: Development, Security
feature: OSGI, Cloud Manager
role: Developer
jira: KT-15880
level: Intermediate, Experienced
exl-id: 856b7da4-9ee4-44db-b245-4fdd220e8a4e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# 在AEM as a Cloud Service中管理秘密

管理秘密（例如API金鑰和密碼）對於維護應用程式安全性至關重要。 Adobe Experience Manager (AEM) as a Cloud Service提供強大的工具，可安全地處理秘密。

在本教學課程中，您將學習AEM中管理秘密的最佳實務。 我們將介紹AEM所提供的工具和技術，以保護您的敏感資訊，確保您的應用程式安全無虞且保密。

本教學課程假設您具備AEM Java開發、OSGi服務、Sling模型和Adobe Cloud Manager的工作知識。

## 密碼管理員OSGi服務

在AEM as a Cloud Service中，透過OSGi服務管理秘密提供可擴充且安全的方法。 OSGi服務可設定為處理敏感資訊，例如API金鑰和密碼、透過OSGi設定來定義，以及透過Cloud Manager設定。

### OSGi服務實施

我們將逐步開發自訂OSGi服務，[會公開OSGi設定的秘密](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values)。

實作透過`@Activate`方法從OSGi設定讀取秘密，並透過`getSecret(String secretName)`方法公開這些秘密。 或者，您可以為每個密碼建立離散方法，例如`getApiKey()`，但此方法需要更多維護，因為新增或移除密碼時。

```java
package com.example.core.util.impl;

import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.apache.sling.api.resource.ValueMap;
import org.apache.sling.api.resource.ValueMapDecorator;
import java.util.Map;

@Component(
    service = { SecretsManager.class }
)
public class SecretsManagerImpl implements SecretsManager {
    private static final Logger log = LoggerFactory.getLogger(SecretsManagerImpl.class);
 
    private ValueMap secrets;

    @Override
    public String getSecret(String secretName) {
        return secrets.get(secretName, String.class);
    }

    @Activate
    @Modified
    protected void activate(Map<String, Object> properties) {
        secrets = new ValueMapDecorator(properties);
    }
}
```

身為OSGi服務，最好透過Java介面註冊及使用。 底下是簡單的介面，可讓消費者透過OSGi屬性名稱取得秘密。

```java
package com.example.core.util;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface SecretsManager {
    String getSecret(String secretName);
}
```

## 將密碼對應至OSGi設定

若要公開OSGi服務中的密碼值，請使用[OSGi密碼組態值](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values)將它們對應到OSGi組態。 將OSGi屬性名稱定義為金鑰，以從`SecretsManager.getSecret()`方法擷取秘密值。

在您的AEM Maven專案的OSGi設定檔案`/apps/example/osgiconfig/config/com.example.core.util.impl.SecretsManagerImpl.cfg.json`中定義秘密。 每個屬性都代表AEM中公開的秘密，其值是透過Cloud Manager設定。 金鑰是OSGi屬性名稱，用來從`SecretsManager`服務擷取秘密值。

```json
{
    "api.key": "$[secret:api_key]",
    "service.password": "$[secret:service_password]"
}
```

除了使用共用密碼管理員OSGi服務之外，您可以直接在使用密碼的特定服務的OSGi設定中包含密碼。 如果只有單一OSGi服務需要秘密，且未在多個服務之間共用秘密，則此方法會很實用。 在此案例中，秘密值定義於特定服務的OSGi設定檔案中，並透過`@Activate`方法存取服務的Java程式碼。

## 使用秘密

可以透過各種方式從OSGi服務使用秘密，例如從Sling模型或其他OSGi服務。 以下是如何從兩者中取用秘密的範例。

### 從Sling模型

Sling模型通常會為AEM網站元件提供商業邏輯。 `SecretsManager` OSGi服務可以透過`@OsgiService`註解使用，並在Sling模型內用於擷取秘密值。

```java
import com.example.core.util.SecretsManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.OsgiService;

@Model(
    adaptables = {SlingHttpServletRequest.class, Resource.class},
    adapters = {ExampleDatabaseModel.class}
)
public class ExampleDatabaseModelImpl implements ExampleDatabaseModel {

    @OsgiService
    SecretsManager secretsManager;

    @Override 
    public String doWork() {
        final String secret = secretsManager.getSecret("api.key");
        // Do work with secret
    }
}
```

### 來自OSGi服務

OSGi服務通常會在AEM中公開可重複使用的商業邏輯，供Sling模型、AEM服務（例如工作流程）或其他自訂OSGi服務使用。 `SecretsManager` OSGi服務可以透過`@Reference`註解使用，並在OSGi服務內用來擷取秘密值。

```java
import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component
public class ExampleSecretConsumerImpl implements ExampleSecretConsumer {

    @Reference
    SecretsManager secretsManager;

    public void doWork() {
        final String secret = secretsManager.getSecret("service.password");
        // Do work with the secret
    }
}
```

## 在Cloud Manager中設定秘密

OSGi服務和設定就緒後，最後一個步驟就是在Cloud Manager中設定秘密值。

密碼的值可以透過[Cloud Manager API](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#tag/Variables)設定，或是更常見的透過[Cloud Manager UI](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#overview)設定。 若要透過Cloud Manager UI套用機密變數：

![Cloud Manager密碼設定](./assets/secrets/cloudmanager-configuration.png)

1. 登入[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)。
1. 選取您要設定密碼的AEM程式和環境。
1. 在「環境詳細資料」檢視中，選取「**組態**」標籤。
1. 選取「**新增**」。
1. 在「環境設定」對話方塊中：
   - 輸入OSGi設定中參照的密碼變數名稱（例如`api_key`）。
   - 輸入密碼值。
   - 選取要套用密碼的AEM服務。
   - 選取&#x200B;**密碼**&#x200B;作為型別。
1. 選取&#x200B;**新增**&#x200B;以保留密碼。
1. 視需要新增儘可能多的秘密。 完成時，選取&#x200B;**儲存**，將變更立即套用至AEM環境。

針對秘密使用Cloud Manager設定的好處是，可以為不同的環境或服務套用不同的值，並且無需重新部署AEM應用程式即可旋轉秘密。
