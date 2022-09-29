---
title: 將Asset compute背景工作與AEM處理設定檔整合
description: AEM as a Cloud Service透過AEM Assets處理設定檔與部署至Adobe I/O Runtime的Asset compute背景工作整合。 處理設定檔是在製作服務中設定，以使用自訂背景工作處理特定資產，並將背景工作產生的檔案儲存為資產轉譯。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# 與AEM處理設定檔整合

若要讓Asset compute背景工作在AEMas a Cloud Service中產生自訂轉譯，必須透過處理設定檔在AEMas a Cloud Service作者服務中註冊。 受該處理設定檔約束的所有資產都會在上傳或重新處理時叫用背景工作，並產生自訂轉譯，並透過資產的轉譯提供使用。

## 定義處理設定檔

首先建立新的處理設定檔，此設定檔將使用可設定的參數叫用背景工作。

![處理設定檔](./assets/processing-profiles/new-processing-profile.png)

1. 登入AEMas a Cloud Service作者服務，作為 __AEM管理員__. 由於此教學課程建議您使用開發環境或沙箱中的環境。
1. 導覽至 __工具>資產>處理設定檔__
1. 點選 __建立__ 按鈕
1. 為處理設定檔命名， `WKND Asset Renditions`
1. 點選 __自訂__ 標籤，然後點選 __新增__
1. 定義新服務
   + __轉譯名稱：__ `Circle`
      + 在AEM Assets中識別此轉譯的轉譯檔案名稱
   + __擴充功能：__ `png`
      + 產生的轉譯的擴充功能。 設為 `png` 因為這是工作人員的web服務支援的支援輸出格式，因此在切斷的圓圈後面會產生透明背景。
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 這是工作人員的URL，取自 `aio app get-url`. 根據AEMas a Cloud Service環境，確定URL點位在正確的工作區。
      + 請確定背景工作URL指向正確的工作區。 AEMas a Cloud ServiceStage應使用預備工作區URL，而AEMas a Cloud Service生產應使用生產工作區URL。
   + __服務參數__
      + 點選 __新增參數__
         + 關鍵: `size`
         + 值: `1000`
      + 點選 __新增參數__
         + 關鍵: `contrast`
         + 值: `0.25`
      + 點選 __新增參數__
         + 關鍵: `brightness`
         + 值: `0.10`
      + 這些傳遞至Asset compute背景工作且可透過 `rendition.instructions` JavaScript物件。
   + __Mime 類型__
      + __包括：__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + 這些MIME類型是工作器npm模組的唯一類型。 此清單會限制自訂背景工作處理的項目。
      + __不包括：__ `Leave blank`
         + 請勿使用此服務設定以這些MIME類型處理資產。 在此情況下，我們只使用允許清單。
1. 點選 __儲存__ 在右上角

## 套用和叫用處理設定檔

1. 選取新建立的處理設定檔， `WKND Asset Renditions`
1. 點選 __將配置檔案應用到資料夾__ 在頂端動作列中
1. 選取要將處理設定檔套用至的資料夾，例如 `WKND` 點選 __套用__
1. 導覽至「處理設定檔」未套用的資料夾，透過 __AEM >資產>檔案__ 並點進 `WKND`.
1. 上傳一些新影像資產([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg)，和 [sample-3.jpg](../assets/samples/sample-3.jpg))，並等待上傳的資產處理完成。
1. 點選資產以開啟其詳細資訊
   + 預設轉譯在AEM中的產生和顯示速度可能會比自訂轉譯更快。
1. 開啟 __轉譯__ 從左側邊欄檢視
1. 點選已命名的資產 `Circle.png` 並查看生成的格式副本

   ![產生的轉譯](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！ 您已完成 [教學課程](../overview.md) 如何擴充AEMas a Cloud ServiceAsset compute微服務！ 您現在應該能夠設定、開發、測試、除錯和部署自訂Asset compute背景工作，以供AEMas a Cloud Service製作服務使用。

### 在Github上檢閱完整專案原始碼

最終Asset compute專案可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是專案的最終狀態，已完全填入背景工作和測試案例，但不含任何憑證，例如。 `.env`, `.config.json` 或 `.aio`._

## 疑難排解

+ [AEM中遺失資產的自訂轉譯](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的資產處理失敗](../troubleshooting.md#asset-processing-fails)
