---
title: 新增使用者指定中繼資料標籤的教學課程
description: 瞭解如何從Azure儲存體帳戶儲存及擷取最適化表單資料。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14501
duration: 39
exl-id: 6a3af59d-f916-451f-887c-0f4580cbcb3e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 2%

---

# 建立OSGi設定

已建立名為「Azure入口網站設定」的自訂OSGi設定，以指定Azure儲存URI和SAS權杖URI。 這兩個值可用來建構REST API以與Azure儲存體通訊

![azure-portal-configuration](assets/azure-portal-configuration.png)

以下列出設定服務的完整程式碼

AzurePortalConfiguration

```java
package com.azure.portal.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Azure Portal Configuration", description = "Settings for connecting to Azure Portal")

public @interface AzurePortalConfiguration {

    @AttributeDefinition(name = "SAS Token", description = "Enter the SAS Token")
    String getSASToken() default "";

    @AttributeDefinition(name = "Enter the Storage URI", description = "Enter the Storage URI")
    String getStorageURI() default"";

}
```

AzurePortalConfigurationService

```java
package com.azure.portal.configuration.service;

public interface AzurePortalConfigurationService {
    String getStorageURI();
    String getSASToken();

}
```

AzurePortalStorageConfigImpl

```java
package com.azure.portal.configuration.service.impl;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import com.azure.portal.configuration.service.AzurePortalConfigurationService;
import com.azure.portal.configuration.AzurePortalConfiguration;
@Component(
        service = AzurePortalConfigurationService.class,
        immediate = true,
        property = {
                Constants.SERVICE_ID+ "=Azure Portal Config Service",
                Constants.SERVICE_DESCRIPTION +"=This service reads values from Azure portal config"
        }
)
@Designate(ocd=AzurePortalConfiguration.class)
public class AzurePortalStorageConfigImpl implements AzurePortalConfigurationService {
    private AzurePortalConfiguration azurePortalConfiguration;
    @Activate
    protected void activate(AzurePortalConfiguration configuration) {
        System.out.println("##### in activate #####");
        this.azurePortalConfiguration = configuration;
    }

    @Override
    public String getStorageURI() {
        // TODO Auto-generated method stub
        return azurePortalConfiguration.getStorageURI();
    }

    @Override
    public String getSASToken() {
        // TODO Auto-generated method stub
        return azurePortalConfiguration.getSASToken();
    }

}
```

## 後續步驟

[建立blob索引標籤](./create-blob-index-tags.md)
