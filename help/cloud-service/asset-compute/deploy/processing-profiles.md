---
title: 將Asset compute背景工作與AEM處理設定檔整合
description: AEMas a Cloud Service可透過AEM Assets處理設定檔與部署至Adobe I/O Runtime的Asset compute背景工作整合。 處理設定檔設定於「作者」服務中，以使用自訂背景工作處理特定資產，以及將背景工作產生的檔案儲存為資產轉譯。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# 與AEM處理設定檔整合

若要Asset compute背景工作在AEMas a Cloud Service中產生自訂轉譯，必須透過處理設定檔在AEMas a Cloud Service作者服務中註冊。 受該處理設定檔約束的所有資產將在上傳或重新處理時叫用背景工作，並產生自訂轉譯，並可透過資產的轉譯提供使用。

## 定義處理設定檔

首先，建立新的處理設定檔，此設定檔將使用可設定的引數叫用背景工作。

![處理設定檔](./assets/processing-profiles/new-processing-profile.png)

1. 以身分登入AEMas a Cloud Service作者服務 __AEM管理員__. 由於這是教學課程，建議您使用開發環境或沙箱中的環境。
1. 瀏覽至 __工具>資產>處理設定檔__
1. 點選 __建立__ 按鈕
1. 為處理設定檔命名， `WKND Asset Renditions`
1. 點選 __自訂__ 標籤，然後點選 __新增__
1. 定義新服務
   + __轉譯名稱：__ `Circle`
      + 用來在AEM Assets中識別此轉譯的轉譯檔案名稱
   + __副檔名：__ `png`
      + 產生的轉譯副檔名。 將設為 `png` 因為這是worker的Web服務支援的輸出格式，所以會在圓剪下的後面產生透明背景。
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 這是工作者的URL，取得自 `aio app get-url`. 根據AEMas a Cloud Service環境，確保URL指向正確的工作區。
      + 確定背景工作URL指向正確的工作區。 AEMas a Cloud Service階段應使用階段工作區URL，而AEMas a Cloud Service生產應使用生產工作區URL。
   + __服務引數__
      + 點選 __新增引數__
         + 索引鍵： `size`
         + 值： `1000`
      + 點選 __新增引數__
         + 索引鍵： `contrast`
         + 值： `0.25`
      + 點選 __新增引數__
         + 索引鍵： `brightness`
         + 值： `0.10`
      + 這些會傳入Asset compute背景工作階段的索引鍵/值組，並可透過以下方式使用： `rendition.instructions` javascript物件。
   + __Mime型別__
      + __包括：__ `image/jpeg`， `image/png`， `image/gif`， `image/bmp`， `image/tiff`
         + 這些MIME型別是工作者的npm模組中的唯一型別。 此清單會限制由自訂背景工作處理的專案。
      + __排除：__ `Leave blank`
         + 請勿使用此服務設定來處理具有這些MIME型別的資產。 在此情況下，我們僅使用允許清單。
1. 點選 __儲存__ 在右上方

## 套用及叫用處理設定檔

1. 選取新建立的處理設定檔， `WKND Asset Renditions`
1. 點選 __套用設定檔至資料夾__ 在頂端動作列中
1. 選取要套用處理設定檔的資料夾，例如 `WKND` 然後點選 __套用__
1. 透過瀏覽至未套用處理設定檔的資料夾 __AEM >資產>檔案__ 並點選 `WKND`.
1. 上傳一些新影像資產([sample-1.jpg](../assets/samples/sample-1.jpg)， [sample-2.jpg](../assets/samples/sample-2.jpg)、和 [sample-3.jpg](../assets/samples/sample-3.jpg))，並等候處理上傳的資產。
1. 點選資產以開啟其詳細資料
   + 在AEM中，預設轉譯的產生和出現速度可能會比自訂轉譯更快。
1. 開啟 __轉譯__ 從左側邊欄檢視
1. 點選名為的資產 `Circle.png` 並檢閱產生的轉譯

   ![產生的轉譯](./assets/processing-profiles/rendition.png)

## 已完成！

恭喜！您已完成 [教學課程](../overview.md) 如何延伸AEMas a Cloud ServiceAsset compute微服務！ 您現在應該能夠設定、開發、測試、除錯和部署自訂Asset compute背景工作，以供AEMas a Cloud Service作者服務使用。

### 在Github上檢閱完整的專案原始程式碼

最終Asset compute專案可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github包含是專案的最終狀態，已完整填入工作程式和測試案例，但不包含任何認證，例如。 `.env`， `.config.json` 或 `.aio`._

## 疑難排解

+ [AEM資產中缺少自訂轉譯](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的資產處理失敗](../troubleshooting.md#asset-processing-fails)
