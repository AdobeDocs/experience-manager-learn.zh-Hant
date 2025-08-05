---
title: 預覽通用編輯器擴充功能
description: 了解如何在開發過程中預覽本機執行的通用編輯器擴充功能。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner, Intermediate, Experienced
doc-type: Tutorial
jira: KT-18658
source-git-commit: f0ad5d66549970337118220156d7a6b0fd30fd57
workflow-type: ht
source-wordcount: '306'
ht-degree: 100%

---


# 預覽本機的通用編輯器擴充功能

>[!TIP]
> 了解如何[建立通用編輯器擴充功能](https://developer.adobe.com/uix/docs/services/aem-universal-editor/)。

若要在開發過程中預覽通用編輯器擴充功能，您需要：

1. 在本機執行擴充功能。
2. 接受自我簽署憑證。
3. 在通用編輯器中開啟頁面。
4. 更新位置 URL 以載入本機擴充功能。

## 在本機執行擴充功能

這裡假設您已建立[通用編輯器擴充功能](https://developer.adobe.com/uix/docs/services/aem-universal-editor/)，並想要在本機進行測試和開發時預覽此擴充功能。

使用以下命令啟動通用編輯器擴充功能：

```bash
$ aio app run
```

您將會看到的輸出如下所示：

```
To view your local application:
  -> https://localhost:9080
To view your deployed application in the Experience Cloud shell:
  -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
```

預設情況下，這會在 `https://localhost:9080` 執行您的擴充功能。


## 接受自我簽署憑證

通用編輯器需要 HTTPS 來載入擴充功能。由於本機開發使用自我簽署憑證，因此您的瀏覽器必須明確地信任此憑證。

開啟新的瀏覽器索引標籤，並導覽到 `aio app run` 命令輸出的本機擴充功能 URL：

```
https://localhost:9080
```

您的瀏覽器將顯示憑證警告。接受憑證以繼續。

![接受自我簽署憑證](./assets/local-extension-preview/accept-certificate.png)

接受憑證後，您便會看到本機擴充功能的預留位置頁面：

![擴充功能可供存取](./assets/local-extension-preview/extension-accessible.png)


## 在通用編輯器中開啟頁面

透過[通用編輯器主控台](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/)或編輯使用通用編輯器的 AEM Sites 中的頁面來開啟通用編輯器：

![在通用編輯器中開啟頁面](./assets/local-extension-preview/open-page-in-ue.png)


## 載入擴充功能

在通用編輯器中，找到介面正中央上方的「**位置**」欄位。展開欄位並更新&#x200B;**在「位置」欄位中的 URL**，**而非瀏覽器位址列的 URL**。

附加以下查詢參數：

* `devMode=true` – 啟用通用編輯器的開發模式。
* `ext=https://localhost:9080` – 載入本機執行的擴充功能。

範例：

```
https://author-pXXX-eXXX.adobeaemcloud.com/content/aem-ue-wknd/index.html?devMode=true&ext=https://localhost:9080
```

![更新通用編輯器位置 URL](./assets/local-extension-preview/update-location-url.png)


## 預覽擴充功能

執行瀏覽器&#x200B;**強制重新載入**&#x200B;以確保使用更新後的 URL。

通用編輯器現在將載入您的本機擴充功能，但僅限您的瀏覽器工作階段。

您在本機所做的任何程式碼變更都會立即反映出來。

![本機擴充功能已載入](./assets/local-extension-preview/extension-loaded.png)

