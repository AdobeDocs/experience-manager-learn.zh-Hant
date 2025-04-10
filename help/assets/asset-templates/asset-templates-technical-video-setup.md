---
title: 使用AEM Assets和InDesign Server設定資產範本
description: 資產範本可讓行銷人員建立、管理和提供數位資產和列印資產。 與InDesign伺服器整合時，使用資產範本可更輕鬆建立行銷手冊、名片、傳單、廣告和明信片。 本節將說明使用AEM設定InDesign伺服器的相關資訊。
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 5b764d86-8ced-46ed-838e-4bd2e75fd64c
duration: 428
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# 使用AEM Assets和InDesign Server設定資產範本{#set-up-asset-templates-with-aem-assets-and-indesign-server}

資產範本可讓行銷人員建立、管理和提供數位資產和列印資產。 與InDesign伺服器整合時，使用資產範本可更輕鬆建立行銷手冊、名片、傳單、廣告和明信片。 本節將說明使用AEM設定InDesign伺服器的相關資訊。

>[!VIDEO](https://video.tv.adobe.com/v/17069?quality=12&learn=on)

>[!NOTE]
>
>上傳INDD範本時，AEM **必須**&#x200B;連線至執行中的InDesign伺服器。 INDD檔案的部分初始處理需要InDesign伺服器。

## 下載InDesign Server試用版 {#download-indesign-server-trial}

下載[InDesign Server試用下載網站](https://www.adobeprerelease.com/)

## 啟動InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
