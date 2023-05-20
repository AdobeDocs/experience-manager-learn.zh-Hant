---
title: 將Asset compute工作人員與處AEM理配置檔案整合
description: AEMas a Cloud Service通過AEM Assets處理配置檔案與部署到Adobe I/O Runtime的Asset compute工人整合。 「處理配置檔案」在「作者」服務中配置，以使用自定義工作程式處理特定資產，並將工作程式生成的檔案儲存為資產格式副本。
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

# 與處理配AEM置檔案整合

要使Asset compute工作人員在as a Cloud Service中生成自定義格AEM式副本，必須通過「處理配置檔案」AEM在as a Cloud Service作者服務中註冊這些格式副本。 受該「處理配置檔案」約束的所有資產都將在上載或重新處理時調用該工作人員，並通過資產的格式副本生成並提供自定義格式副本。

## 定義處理配置檔案

首先建立一個新的「處理配置檔案」，該配置檔案將使用可配置參數調用工作人員。

![處理配置檔案](./assets/processing-profiles/new-processing-profile.png)

1. 登錄AEM到as a Cloud Service作者服務 __管AEM理員__。 因為本教程建議使用開發環境或沙盒中的環境。
1. 導航到 __「工具」>「資產」>「處理配置檔案」__
1. 點擊 __建立__ 按鈕
1. 命名處理配置檔案， `WKND Asset Renditions`
1. 點擊 __自定義__ 頁籤，然後點擊 __添加新__
1. 定義新服務
   + __格式副本名稱：__ `Circle`
      + 用於標識此格式副本的格式副本的檔案名(在AEM Assets)
   + __擴展：__ `png`
      + 生成的格式副本的擴展。 設定為 `png` 因為這是工作人員的Web服務支援的支援輸出格式，並導致圓切出後出現透明背景。
   + __終結點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 這是通過以下方式獲取的工作人員的URL `aio app get-url`。 根據as a Cloud Service環境確保URL點在正確的工AEM作區。
      + 確保工作URL指向正確的工作區。 AEMas a Cloud Service階段應使用階段工作區URL,AEMas a Cloud Service生產應使用生產工作區URL。
   + __服務參數__
      + 點擊 __添加參數__
         + 金鑰: `size`
         + 值: `1000`
      + 點擊 __添加參數__
         + 金鑰: `contrast`
         + 值: `0.25`
      + 點擊 __添加參數__
         + 金鑰: `brightness`
         + 值: `0.10`
      + 這些傳遞到Asset compute工作進程且通過 `rendition.instructions` JavaScript對象。
   + __Mime 類型__
      + __包括：__ `image/jpeg`。 `image/png`。 `image/gif`。 `image/bmp`。 `image/tiff`
         + 這些MIME類型是工作人員的npm模組中的唯一類型。 此清單限制由自定義工作人員處理。
      + __不包括：__ `Leave blank`
         + 切勿使用此服務配置使用這些MIME類型處理資產。 在這種情況下，我們只使用允許清單。
1. 點擊 __保存__ 右上角

## 應用並調用處理配置檔案

1. 選擇新建立的「處理配置檔案」， `WKND Asset Renditions`
1. 點擊 __將配置檔案應用到資料夾__ 的子菜單。
1. 選擇要將處理配置檔案應用到的資料夾，如 `WKND` 點擊 __應用__
1. 導航到未通過應用處理配置檔案的資料夾 __AEM>資產>檔案__ 並點擊 `WKND`。
1. 上載一些新映像資產([示例1.jpg](../assets/samples/sample-1.jpg)。 [示例2.jpg](../assets/samples/sample-2.jpg), [示例3.jpg](../assets/samples/sample-3.jpg))，並等待上載的資產被處理。
1. 點擊資產以開啟其詳細資訊
   + 預設格式副本的生成和顯示速度可能比自AEM定義格式副本快。
1. 開啟 __格式副本__ 從左側欄查看
1. 點擊名為 `Circle.png` 並查看生成的格式副本

   ![生成的格式副本](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！您已完成 [教程](../overview.md) 如何擴展AEMas a Cloud ServiceAsset compute微服務！ 現在，您應該能夠設定、開發、test、調試和部署自定義Asset compute工作程式，供as a Cloud Service作者服AEM務使用。

### 查看Github上的完整項目原始碼

Github的最終Asset compute項目可在以下網址獲得：

+ [輔助線 — wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contains是項目的最終狀態，它完全填充了工作程式和test案例，但不包含任何憑據，如。 `.env`, `.config.json` 或 `.aio`._

## 疑難排解

+ [中的資產中缺少自定義格式副AEM本](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [資產處理失敗AEM](../troubleshooting.md#asset-processing-fails)
