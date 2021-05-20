---
title: 使用AEM Assets和InDesign Server設定資產範本
description: 資產範本可讓行銷人員建立、管理及提供數位及列印用的數位資產。 與InDesign伺服器整合時，使用資產範本可更輕鬆地建立行銷手冊、名片、傳單、廣告和貼文卡。 使用AEM設定InDesign伺服器的相關說明請參閱本節。
version: 6.3, 6.4, 6.5
topic: 內容管理
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# 使用AEM Assets和InDesign Server設定資產範本{#set-up-asset-templates-with-aem-assets-and-indesign-server}

資產範本可讓行銷人員建立、管理及提供數位及列印用的數位資產。 與InDesign伺服器整合時，使用資產範本可更輕鬆地建立行銷手冊、名片、傳單、廣告和貼文卡。 使用AEM設定InDesign伺服器的相關說明請參閱本節。

>[!VIDEO](https://video.tv.adobe.com/v/17069/?quality=9&learn=on)

>[!NOTE]
>
>上傳INDD範本時，AEM **必須**&#x200B;連線至執行中的InDesign伺服器。 INDD檔案的初始處理部分需要InDesign伺服器。

## 下載InDesign Server試用版{#download-indesign-server-trial}

下載[InDesign Server試用下載網站](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)

## 啟動InDesign Server{#starting-indesign-server}

```shell
# macOS command

$ /Applications/Adobe\ InDesign\ CC\ Server\ 2017/InDesignServer -port 8080
```
