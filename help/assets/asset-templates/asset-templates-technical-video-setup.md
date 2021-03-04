---
title: 使用AEM Assets和InDesign Server設定資產模板
description: 「資產範本」可讓行銷人員建立、管理和提供數位和印刷的數位資產。 當與InDesign伺服器整合時，使用資產範本建立行銷手冊、名片、傳單、廣告和貼文卡會更輕鬆。 本節將介紹帶的InDesignAEM伺服器的配置。
version: 6.3, 6.4, 6.5
topic: 內容管理
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---


# 使用AEM Assets和InDesign Server設定資產模板{#set-up-asset-templates-with-aem-assets-and-indesign-server}

「資產範本」可讓行銷人員建立、管理和提供數位和印刷的數位資產。 當與InDesign伺服器整合時，使用資產範本建立行銷手冊、名片、傳單、廣告和貼文卡會更輕鬆。 本節將介紹帶的InDesignAEM伺服器的配置。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>AEM上載INDD模板時，**必須**&#x200B;連接到正在運行的InDesign伺服器。 INDD檔案的初始處理部分需要InDesign伺服器。

## 下載InDesign Server試用版{#download-indesign-server-trial}

下載[InDesign Server試用版下載網站](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## 啟動InDesign Server{#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
