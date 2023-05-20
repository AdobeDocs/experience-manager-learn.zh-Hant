---
title: Dynamic Media與AEM Assets概述
description: 本視頻系列概括介紹了如何使用Adobe Experience ManagerDynamic Media作為內容服務來管理和訪問媒體內容。 Dynamic Media讓您管理和發佈動態數字型驗 — 這是Experience Manager Assets獨有的功能。 我們的框架和元件套件使營銷人員能夠定制和提供跨所有設備的互動式多媒體體驗。
feature: Smart Crop, Video Profiles, Image Profiles, Viewer Presets, 360 VR Video, Image Sets, Spin Sets
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 59462cb4-d379-4e58-b786-ff8dbae6191c
source-git-commit: c2e105123302ae37dc7cfca9533110a655e83858
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 0%

---

# 將Dynamic Media與AEM Assets {#understanding-aem-dynamic-media}

此多部分視頻系列概括介紹了如何使用Adobe Experience ManagerDynamic Media作為內容服務來管理和訪問媒體內容。 Dynamic Media讓您管理和發佈動態數字型驗 — 這是Experience Manager Assets獨有的功能。 我們的框架和元件套件使營銷人員能夠定制和提供跨所有設備的互動式多媒體體驗。

## Dynamic Media概述

>[!VIDEO](https://video.tv.adobe.com/v/27144?quality=12&learn=on)

>[!NOTE]
>
>此處演示的功能可通過Dynamic MediaDMS7運行模式獲得，我們當前支援的運行模式，而不一定是DMHybrid運行模式，DMS7已經替換了該模式。

此視頻描述如何使用Adobe Experience ManagerDynamic Media作為內容服務來管理和訪問媒體內容。 Dynamic Media採用單一「主資產」方法，在該方法中，您可以上載可請求滿足無限組所需消耗性變體或衍生格式副本的影像資產或視頻資產。 包括：

* 已說明單一主資產到URL產品交付項
* 影像處理選項
* 內容查看器選項
* 將URL複製到影像和響應查看器
* 智慧裁剪URL變體

## 用於AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/27145?quality=12&learn=on)

>[!NOTE]
>
>此處演示的功能可通過Dynamic MediaDMS7運行模式獲得，我們當前支援的運行模式，而不一定是DMHybrid運行模式，DMS7已經替換了該模式。 此視頻參考了第1部分視頻(Dynamic Media概述)中描述的概念。

此視頻描述了媒體內容在Adobe Experience ManagerDynamic Media的管理方式，並可以在AEM Sites使用，並帶有一個元件，以便簡單且自動裁剪，以根據響應頁面寬度進行優化。 輕鬆建立互動式影像標題並生成要在任何內容管理系統中使用的複製URL。

* AEM SitesDynamic Media元件靈活性
* 使用影像預設本地下載
* 建立和發佈互動式橫幅

## 構建混合媒體集合

>[!VIDEO](https://video.tv.adobe.com/v/27146?quality=12&learn=on)

>[!NOTE]
>
>此處演示的功能可通過Dynamic MediaDMS7運行模式獲得，我們當前支援的運行模式，而不一定是DMHybrid運行模式，DMS7已經替換了該模式。 此視頻參考了第1部分視頻(Dynamic Media概述)中描述的概念。

此視頻描述了媒體資產的混合媒體查看器集合的簡單建立過程，包括旋轉集、視頻和產品影像集合。 將內容添加到混合媒體集，並建立自定義查看器以在最終的複製URL或AEM Sites元件中進行選擇。

* 從相應的產品照片建立旋轉集
* 為Dynamic Media視頻上傳和編碼主視頻
* 從旋轉集、視頻和照片建立混合媒體集
* 編輯和使用自定義混合媒體查看器

## 影像預設集

>[!VIDEO](https://video.tv.adobe.com/v/27320?quality=12&learn=on)

此視頻描述如何建立影像預設以及什麼是影像預設，即URL縮短器，用於在URL請求時對影像進行操作的一系列影像伺服器參數。 瞭解擴展和編輯影像預設的有用技術。

* 影像預設簡訊器隱藏顯式影像伺服器命令的集合
* 僅使用一個像素尺寸 — 寬度或高度 — 來符合新調整大小的影像而不填充
* 始終使用銳化
* 「URL修飾符」欄位，添加附加命令以調整影像預設的大小

## 高級URL修飾符

>[!VIDEO](https://video.tv.adobe.com/v/27319?quality=12&learn=on)

此視頻描述了通過調整影像大小以利用源檔案本身的功能 — 背景透明度，內置剪貼路徑以及作物和文本作為變數 — 以及Dynamic Media的URL修飾符。

* 在「Dynamic Media修改量」欄位中使用URL修改量
* 在具有透明度的影像上更改背景顏色
* 剪切到影像路徑
* 裁剪到影像路徑
* 從Photoshop檔案建立文本模板

## JPEG檔案大小管理

>[!VIDEO](https://video.tv.adobe.com/v/27404?quality=12&learn=on)


>[!NOTE]
>
>影像質量是以逆壓縮的百分比來測量的，其中100%質量被壓縮得最少，從而產生高質量影像但檔案大小相對較大。 Jpeg壓縮是一種有損壓縮方案，其中壓縮設定決定影像質量和檔案大小。

使用2個命令調整jpeg壓縮設定，將jpeg影像質量與生成的檔案大小（以千位元組為單位）平衡，以提高頁面載入速度。 QLT通過調整jpeg壓縮質量設定來定義影像質量。 「JPEG大小」(Size)命令允許您指定使用壓縮需要達到的檔案大小。

## 隱藏字幕

>[!VIDEO](https://video.tv.adobe.com/v/28074?quality=12&learn=on)

通過附加「複製URL」以指向附加的「隱藏字幕」檔案文檔web.VTT欄位檔案，包含任何視頻的CC資訊，可輕鬆將隱藏字幕添加到Dynamic Media視頻。

## 影像銳化

此視頻介紹銳化影像對保持影像保真度至關重要的原因，以及如何使用高級設定來製作完美的影像。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
