---
title: 使用AEM Assets和InDesign Server設定資產範本
description: 「資產範本」可讓行銷人員建立、管理和提供數位和印刷的數位資產。 當與InDesign伺服器整合時，使用資產範本建立行銷手冊、名片、傳單、廣告和貼文卡會更輕鬆。 本節將說明使用AEM設定InDesign伺服器。
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: developer, architect, administrator
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# 使用AEM Assets和InDesign Server設定資產範本{#set-up-asset-templates-with-aem-assets-and-indesign-server}

「資產範本」可讓行銷人員建立、管理和提供數位和印刷的數位資產。 當與InDesign伺服器整合時，使用資產範本建立行銷手冊、名片、傳單、廣告和貼文卡會更輕鬆。 本節將說明使用AEM設定InDesign伺服器。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM **必須**&#x200B;在上傳INDD範本時連線至執行中的InDesign伺服器。 INDD檔案的初始處理部分需要InDesign伺服器。

## 下載InDesign Server試用版{#download-indesign-server-trial}

下載[InDesign Server試用版下載網站](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## 啟動InDesign Server {#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
