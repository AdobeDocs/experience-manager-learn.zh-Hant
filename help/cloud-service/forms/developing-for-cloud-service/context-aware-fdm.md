---
title: 上下文感知配置覆蓋對表單資料模型的支援
description: 設定表單資料模型以根據環境與不同的端點通信。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 10423
source-git-commit: 2ac0f6b3964590e5443700f730a3fc02cb3f63bc
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# 上下文感知雲配置

在本地環境中建立雲配置並成功測試時，您希望在上游環境中使用相同的雲配置，但不必更改端點、密鑰/密碼或用戶名。 為了實現此使用案例，AEM Forms在Cloud Service上引入了定義上下文感知雲配置的能力。
例如，Azure儲存帳戶雲配置可以在開發、階段和生產環境中使用不同的連接字串和密鑰來重複使用。

建立上下文感知雲配置需要執行以下步驟

## 建立環境變數

標準環境變數可以通過雲管理器進行配置和管理。 它們被提供給運行時間環境，並可用於OSGi配置。 環境變數可以是特定於環境的值，也可以是基於正在更改的內容的環境機密。

[環境變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)

以下螢幕抓圖顯示了定義的azure_key和azure_connection_string環境變數
![環境變數](assets/environment-variables.png)

然後，可以在配置檔案中指定這些環境變數以在適當環境中使用。例如，如果希望所有作者實例使用這些環境變數，您將在config.author資料夾中定義配置檔案，如下所指定

## 建立配置檔案

在IntelliJ中開啟項目。 導航到config.author並建立名為

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

將以下文本複製到上一步中建立的檔案中。 此檔案中的代碼正在用環境變數覆蓋accountName和accountKey屬性的值 **azure_connection_string** 和 **azure密鑰**。

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>此配置將應用於雲服務實例中的所有作者環境。 要將配置應用於發佈環境，您必須將同一配置檔案放在intelliJ項目的config.publish資料夾中
>[!NOTE]
> 請確保正在被覆蓋的屬性是雲配置的有效屬性。 導航到雲配置以查找要覆蓋的屬性，如下所示。

![雲配置屬性](assets/cloud-config-properties.png)

對於基於REST的雲配置，通常需要為serviceEndPoint、userName和password屬性建立環境變數。