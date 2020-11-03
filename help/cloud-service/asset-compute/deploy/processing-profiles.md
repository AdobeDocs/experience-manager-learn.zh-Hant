---
title: 將資產計算工作者與AEM處理設定檔整合
description: AEM as a Cloud Service透過AEM Assets Processing Profiles與部署至Adobe I/O Runtime的資產計算工作者整合。 「處理設定檔」是在「作者」服務中設定，以使用自訂工作者處理特定資產，並將工作者產生的檔案儲存為資產轉譯。
feature: asset-compute, processing-profiles
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 1%

---


# 與AEM處理設定檔整合

若要讓資產運算工作者在AEM中以Cloud Service的形式產生自訂轉譯，必須透過「處理設定檔」在AEM中以Cloud Service Author服務的形式註冊。 受該處理設定檔約束的所有資產都會在上傳或重新處理時呼叫工作者，並透過資產的轉譯產生並提供自訂轉譯。

## 定義處理設定檔

首先建立新的處理配置檔案，該配置檔案將使用可配置參數調用工作器。

![處理設定檔](./assets/processing-profiles/new-processing-profile.png)

1. 以 __AEM管理員身分登入AEM，以Cloud Service Author服務的身分登入__。 因為這是教學課程，我們建議您使用開發環境或沙盒中的環境。
1. 導覽至「 __工具>資產>處理設定檔」__
1. 點選「 __建立__ 」按鈕
1. 命名處理設定檔， `WKND Asset Renditions`
1. 點選「自 __訂__ 」標籤，點選「新 __增」__
1. 定義新服務
   + __轉譯名稱：__ `Circle`
      + 用於在AEM Assets中識別此轉譯的檔案名稱轉譯
   + __擴充功能：__ `png`
      + 將生成的轉譯的副檔名。 設為 `png` 這樣，工作者的web services就支援的輸出格式，會在剪掉的圓圈後面產生透明背景。
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 這是通過獲取的員工的URL `aio app get-url`。 根據AEM做為雲端服務環境，確保URL點位於正確的工作區。
      + 請確定工作器URL指向正確的工作區。 AEM作為雲端服務舞台應使用「舞台」工作區URL，而AEM作為雲端服務生產應使用「生產」工作區URL。
   + __服務參數__
      + 點選「 __新增參數」__
         + 關鍵: `size`
         + 值: `1000`
      + 點選「 __新增參數」__
         + 關鍵: `contrast`
         + 值: `0.25`
      + 點選「 __新增參數」__
         + 關鍵: `brightness`
         + 值: `0.10`
      + 這些鍵／值配對會傳遞至資產計算工作器，並可透過 `rendition.instructions` JavaScript物件使用。
   + __Mime 類型__
      + __包括：__`image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + 這些MIME類型是工作者的npm模組中唯一的MIME類型。 此清單會限制自訂工作者將處理哪些資產。
      + __排除：__ `Leave blank`
         + 切勿使用此服務配置使用這些MIME類型處理資產。 在這種情況下，我們只使用允許清單。
1. 點選 __右上__ 「儲存」

## 套用並叫用處理設定檔

1. 選擇新建立的處理配置檔案， `WKND Asset Renditions`
1. 點選 __頂端動作列中的「套用描述檔至資料夾__ 」
1. 選取要將處理設定檔套用至的檔案夾，例如點選「套 `WKND` 用」 __。__
1. 導覽至未透過 __AEM >資產>檔案套用處理設定檔的檔案夾__ ，然後點選 `WKND`至。
1. 在套用「處理設定檔」的檔案夾下方的任何檔案夾中，上傳某些新的影像資產([sample-1.jpg](../assets/samples/sample-1.jpg)、 [sample-2.jpg](../assets/samples/sample-2.jpg)和 [sample-3.jpg](../assets/samples/sample-3.jpg))，並等待已上傳的資產被處理。
1. 點選資產以開啟其詳細資訊
   + 預設轉譯可能比自訂轉譯在AEM中產生和顯示得更快。
1. 從左側邊 __欄開啟__ 「轉譯」檢視
1. 點選命名的資產並 `Circle.png` 檢閱產生的轉譯

   ![產生的轉譯](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！ 您已完成教學課 [程](../overview.md) ，瞭解如何將AEM延伸為雲端服務資產計算微服務！ 您現在應該能夠設定、開發、測試、除錯和部署自訂的「資產計算」工作者，以便AEM做為「雲端服務作者」服務使用。

### 在Github上檢視完整的專案原始碼

Github上提供最終資產計算專案，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是專案的最終狀態，已填入工作者和測試案例，但不包含任何憑證，例如。 `.env`、 `.config.json` 或 `.aio`。_

## 疑難排解

+ [AEM中資產中遺失自訂轉譯](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的資產處理失敗](../troubleshooting.md#asset-processing-fails)
