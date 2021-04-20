---
title: 將Asset compute員工與處理設AEM定檔整合
description: 作AEM為Cloud Service，可透過AEM Assets處理設定檔與部署至Adobe I/O Runtime的Asset compute員工整合。 「處理設定檔」是在「作者」服務中設定，以使用自訂工作者處理特定資產，並將工作者產生的檔案儲存為資產轉譯。
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 2%

---


# 與處理設定AEM檔整合

asset compute工作者若要以Cloud Service形式產生自訂轉譯，AEM必須透過「處理設定檔」以AEMCloud Service作者服務的形式註冊。 受該處理設定檔約束的所有資產都會在上傳或重新處理時呼叫工作者，並透過資產的轉譯產生並提供自訂轉譯。

## 定義處理設定檔

首先建立新的處理配置檔案，該配置檔案將使用可配置參數調用工作器。

![處理設定檔](./assets/processing-profiles/new-processing-profile.png)

1. 以&lt;AEMa0/>管理員&#x200B;__的身分登AEM錄為Cloud Service作者服務。__&#x200B;因為這是教學課程，我們建議您使用開發環境或沙盒中的環境。
1. 導覽至「__工具>資產>處理設定檔__」
1. 點選&#x200B;__Create__&#x200B;按鈕
1. 命名處理設定檔`WKND Asset Renditions`
1. 點選「__Custom__」標籤，點選「__新增__」
1. 定義新服務
   + __轉譯名稱：__ `Circle`
      + 將用於在AEM Assets標識此轉譯的檔案名轉譯
   + __擴充功能：__ `png`
      + 將生成的轉譯的副檔名。 設定為`png`，因為這是工作者Web服務支援的支援輸出格式，並導致圓形裁剪後出現透明背景。
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 這是通過`aio app get-url`獲取的工作器的URL。 根據作為Cloud Service環境，確保URL點位於AEM正確的工作區。
      + 請確定工作器URL指向正確的工作區。 作AEM為Cloud Service舞台應使用舞台工作區URL,AEM作為Cloud Service生產應使用生產工作區URL。
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
      + 這些鍵／值對會傳遞至Asset compute工作器，並可透過`rendition.instructions` JavaScript物件使用。
   + __Mime 類型__
      + __包括：__ `image/jpeg`、 `image/png`、 `image/gif`、 `image/bmp`、  `image/tiff`
         + 這些MIME類型是工作者的npm模組中唯一的MIME類型。 此清單會限制自訂工作者將處理哪些資產。
      + __排除：__ `Leave blank`
         + 切勿使用此服務配置使用這些MIME類型處理資產。 在這種情況下，我們只使用允許清單。
1. 點選右上角的&#x200B;__Save__

## 套用並叫用處理設定檔

1. 選擇新建立的處理配置檔案`WKND Asset Renditions`
1. 點選頂端動作列中的「將描述檔套用至資料夾」(__Apply Profile to Folder(s)__)
1. 選擇要將處理配置檔案應用到的資料夾，例如`WKND` ，然後點選&#x200B;__應用__
1. 導覽至未透過&#x200B;__AEM >資產>檔案__&#x200B;套用處理設定檔的檔案夾，然後點選`WKND`。
1. 在套用「處理設定檔」的資料夾下方的任何資料夾中，上傳一些新的影像資產（[sample-1.jpg](../assets/samples/sample-1.jpg)、[sample-2.jpg](../assets/samples/sample-2.jpg)和[sample-3.jpg](../assets/samples/sample-3.jpg)），並等待已上傳的資產被處理。
1. 點選資產以開啟其詳細資訊
   + 預設轉譯的產生和顯示速度可能會比自訂轉AEM譯更快。
1. 從左側邊欄開啟&#x200B;__轉譯__&#x200B;檢視
1. 點選名為`Circle.png`的資產並檢視產生的轉譯

   ![產生的轉譯](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！ 您已完成[教學課程](../overview.md)，瞭解如何延伸AEM為Cloud ServiceAsset compute微服務！ 您現在應該能夠設定、開發、測試、除錯和部署自訂Asset compute工作者，以供您的Cloud Service作AEM者服務使用。

### 在Github上檢視完整的專案原始碼

Github提供最終Asset compute項目，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是專案的最終狀態，已填入工作者和測試案例，但不包含任何憑證，例如。`.env`、 `.config.json` 或 `.aio`。_

## 疑難排解

+ [自訂轉譯遺失於](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [資產處理失敗AEM於](../troubleshooting.md#asset-processing-fails)
