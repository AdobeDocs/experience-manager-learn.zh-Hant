---
title: 將Asset compute背景工作與AEM處理設定檔整合
description: AEM as aCloud Service透過AEM Assets處理設定檔與部署至Adobe I/O Runtime的Asset compute背景工作整合。 處理設定檔是在製作服務中設定，以使用自訂背景工作處理特定資產，並將背景工作產生的檔案儲存為資產轉譯。
feature: asset compute微服務
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: 整合，開發
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 1%

---


# 與AEM處理設定檔整合

若要讓Asset compute背景工作以Cloud Service形式在AEM中產生自訂轉譯，必須透過處理設定檔在AEM中註冊為Cloud Service製作服務。 受該處理設定檔約束的所有資產都會在上傳或重新處理時叫用背景工作，並產生自訂轉譯，並透過資產的轉譯提供使用。

## 定義處理設定檔

首先建立新的處理設定檔，此設定檔將使用可設定的參數叫用背景工作。

![處理設定檔](./assets/processing-profiles/new-processing-profile.png)

1. 以&#x200B;__AEM管理員__&#x200B;登入AEM作為Cloud Service製作服務。 由於此教學課程建議您使用開發環境或沙箱中的環境。
1. 導覽至&#x200B;__工具>資產>處理設定檔__
1. 點選&#x200B;__建立__&#x200B;按鈕
1. 為處理設定檔命名， `WKND Asset Renditions`
1. 點選&#x200B;__Custom__&#x200B;標籤，然後點選&#x200B;__Add New__
1. 定義新服務
   + __轉譯名稱：__ `Circle`
      + 在AEM Assets中用於識別此轉譯的檔案名稱轉譯
   + __擴充功能：__ `png`
      + 將產生的轉譯的擴充功能。 設為`png`，因為這是工作人員Web服務支援的支援輸出格式，並且會在切出的圓後產生透明背景。
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 這是通過`aio app get-url`獲取的工作器的URL。 根據AEM作為Cloud Service環境，確保URL指向正確的工作區。
      + 請確定背景工作URL指向正確的工作區。 AEM as aCloud Service階段應使用階段工作區URL，而AEM as aCloud Service生產應使用生產工作區URL。
   + __服務參數__
      + 點選&#x200B;__新增參數__
         + 關鍵: `size`
         + 值: `1000`
      + 點選&#x200B;__新增參數__
         + 關鍵: `contrast`
         + 值: `0.25`
      + 點選&#x200B;__新增參數__
         + 關鍵: `brightness`
         + 值: `0.10`
      + 這些傳遞至Asset compute背景工作且可透過`rendition.instructions` JavaScript物件使用的索引鍵/值組。
   + __Mime 類型__
      + __包括：__ `image/jpeg`、  `image/png`、  `image/gif`、  `image/bmp`、  `image/tiff`
         + 這些MIME類型是工作器npm模組的唯一類型。 此清單會限制自訂背景工作將處理哪些資產。
      + __不包括：__ `Leave blank`
         + 請勿使用此服務設定以這些MIME類型處理資產。 在此情況下，我們只使用允許清單。
1. 點選右上角的&#x200B;__儲存__

## 套用和叫用處理設定檔

1. 選取新建立的處理設定檔`WKND Asset Renditions`
1. 點選頂端動作列中的「將描述檔套用至資料夾」____
1. 選取要將處理設定檔套用至的資料夾，例如`WKND`，然後點選&#x200B;__套用__
1. 導覽至未透過&#x200B;__AEM >資產>檔案__&#x200B;套用處理設定檔的資料夾，然後點選`WKND`。
1. 在套用「處理設定檔」的資料夾下方的任何資料夾中，上傳一些新影像資產（[sample-1.jpg](../assets/samples/sample-1.jpg)、[sample-2.jpg](../assets/samples/sample-2.jpg)和[sample-3.jpg](../assets/samples/sample-3.jpg)），並等待上傳的資產處理。
1. 點選資產以開啟其詳細資訊
   + 預設轉譯在AEM中的產生和顯示速度可能會比自訂轉譯更快。
1. 從左側邊欄開啟&#x200B;__轉譯__&#x200B;檢視
1. 點選名為`Circle.png`的資產，並檢閱產生的轉譯

   ![產生的轉譯](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！ 您已完成[教學課程](../overview.md)，了解如何將AEM延伸為Cloud ServiceAsset compute微服務！ 您現在應該能夠設定、開發、測試、除錯和部署自訂Asset compute背景工作，以供AEM作為Cloud Service製作服務使用。

### 在Github上檢閱完整專案原始碼

最終Asset compute專案可在Github取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是專案的最終狀態，已完全填入背景工作和測試案例，但不含任何憑證，例如。`.env`、 `.config.json` 或 `.aio`。_

## 疑難排解

+ [AEM中遺失資產的自訂轉譯](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的資產處理失敗](../troubleshooting.md#asset-processing-fails)
