---
title: 表單資料模型的內容感知設定覆寫支援
description: 設定表單資料模型，根據環境與不同端點通訊。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
duration: 88
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 7%

---

# 內容感知雲端設定

當您在本機環境中建立雲端設定，並在成功測試時，您會想要在上游環境中使用相同的雲端設定，但不需要變更端點、秘密金鑰/密碼和/或使用者名稱。 為了達成此使用案例，Cloud Service上的AEM Forms引進了定義上下文感知雲端設定的功能。
例如，透過為使用不同的連線字串和金鑰，Azure儲存體帳戶雲端設定可在開發、暫存和生產環境中重複使用。

建立內容感知雲端設定需要下列步驟

## 建立環境變數

可以透過 Cloud Manager 設定和管理標準環境變數。它們提供給執行階段環境並且可以在 OSGi 設定中使用。[環境變數可以是特定環境的值或環境秘密，具體取決於變更的內容。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



以下熒幕擷取畫面顯示定義的azure_key和azure_connection_string環境變數
![environment_variable](assets/environment-variables.png)

然後，可以在要用於適當環境的設定檔案中指定這些環境變數。例如，如果您希望所有作者執行個體都使用這些環境變數，您將在config.author資料夾中定義設定檔案，如下所示

## 建立組態檔

在IntelliJ中開啟您的專案。 導覽至config.author並建立名為的檔案

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

將下列文字複製到您在上一步建立的檔案中。 此檔案中的程式碼會以環境變數覆寫accountName和accountKey屬性的值 **azure_connection_string** 和 **azure_key**.

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
>此設定將套用至您的雲端服務執行個體中的所有作者環境。 若要將設定套用至發佈環境，您必須將相同的設定檔案置於intelliJ專案的config.publish資料夾中
>[!NOTE]
> 請確定正在覆寫的屬性是雲端設定的有效屬性。 導覽至雲端設定，以尋找您要覆寫的屬性，如下所示。

![cloud-config-property](assets/cloud-config-properties.png)

對於具有基本驗證的REST型雲端設定，您通常會想要為serviceEndPoint、userName和密碼屬性建立環境變數。

## 後續步驟

[將您的AEM專案推送到Cloud Manager](./push-project-to-cloud-manager-git.md)
