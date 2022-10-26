---
title: Dynamic Media與AEM Assets概述
description: 本影片系列概述如何使用Adobe Experience Manager Dynamic Media作為內容服務服務來管理和存取媒體內容。 Dynamic Media可讓您管理和發佈動態數位體驗 — Experience Manager Assets獨有的功能。 我們的架構和元件套裝可讓行銷人員跨所有裝置自訂和提供互動式多媒體體驗。
feature: Smart Crop, Video Profiles, Image Profiles, Viewer Presets, 360 VR Video, Image Sets, Spin Sets
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 59462cb4-d379-4e58-b786-ff8dbae6191c
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '871'
ht-degree: 0%

---

# 將Dynamic Media與AEM Assets搭配使用 {#understanding-aem-dynamic-media}

此多部分影片系列概述如何使用Adobe Experience Manager Dynamic Media作為內容服務服務來管理和存取媒體內容。 Dynamic Media可讓您管理和發佈動態數位體驗 — Experience Manager Assets獨有的功能。 我們的架構和元件套裝可讓行銷人員跨所有裝置自訂和提供互動式多媒體體驗。

## Dynamic Media概述

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>此處展示的功能可搭配Dynamic Media DMS7執行模式使用，目前支援的執行模式不一定是DMHybrid執行模式（DMS7已取代）。

此影片說明如何使用Adobe Experience Manager Dynamic Media作為內容服務服務來管理和存取媒體內容。 Dynamic Media採用單一主資產方法運作，您可上傳影像資產或視訊資產，以完成無限組需要的可耗用變數或衍生轉譯。 包括：

* 單一主資產到URL產品交付項說明
* 影像處理選項
* 內容檢視器選項
* 複製URL至影像和回應式檢視器
* URL的智慧型裁切變體

## Dynamic Media與AEM Sites及任何其他CMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>此處展示的功能可搭配Dynamic Media DMS7執行模式使用，目前支援的執行模式不一定是DMHybrid執行模式（DMS7已取代）。 此影片會參考第1部分影片(Dynamic Media概觀)中說明的概念。

此影片說明如何在Adobe Experience Manager Dynamic Media中管理媒體內容，以及如何透過元件輕鬆在AEM Sites中使用，以讓您輕鬆自動裁切，並根據回應式頁面寬度進行最佳化。 輕鬆建立互動式影像橫幅，並產生複製URL以用於任何內容管理系統。

* AEM Sites Dynamic Media元件彈性
* 使用影像預設集在本機下載
* 建立和發佈互動式橫幅

## Dynamic Media以建立混合媒體收集和自訂檢視器

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>此處展示的功能可搭配Dynamic Media DMS7執行模式使用，目前支援的執行模式不一定是DMHybrid執行模式（DMS7已取代）。 此影片會參考第1部分影片(Dynamic Media概觀)中說明的概念。

此影片說明混合媒體檢視器媒體資產集合的簡單建立程式，包括回轉集、影片和產品影像集合。 將內容新增至混合媒體集，並建立自訂檢視器，以在最終的複製URL或AEM Sites元件中進行選擇。

* 從適當的產品像片建立回轉集
* 上傳及編碼Dynamic Media影片的主要影片
* 從回轉集、視訊和像片建立混合媒體集
* 編輯和使用自訂混合媒體檢視器

## Dynamic Media影像預設集

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

此影片說明如何建立影像預設集，以及什麼是影像預設集，這是一系列影像伺服器引數的URL縮短程式，只要URL要求，就會在影像上運作。 了解擴充和編輯影像預設集的重要技巧。

* 影像預設集縮短程式隱藏顯式影像伺服器命令的集合
* 僅使用一個像素尺寸（寬度或高度），以符合新調整大小的影像，無需填充
* 一律使用銳利化
* 「URL修飾詞」欄位，以新增額外命令來調整影像預設集的大小

## Dynamic Media進階URL修飾元

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

本影片說明如何運用來源檔案本身的功能調整影像大小以外的用途：背景透明度、內建剪裁路徑，以及以變數形式裁切和文字（使用Dynamic Media的URL修飾元）。

* 在「Dynamic Media修飾詞」欄位中使用URL修飾詞
* 更改影像上具有透明度的背景顏色
* 剪裁影像路徑
* 裁切至影像路徑
* 從Photoshop檔案建立文字範本

## Dynamic Media控制JPEG檔案大小（以千位元組為單位）

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>影像品質是以反向壓縮的百分比來測量，其中100%品質最低會壓縮，產生高品質的影像，但檔案大小相對較大。 Jpeg壓縮是有損壓縮方案，其中壓縮設定決定影像質量和檔案大小。

使用2個命令調整jpeg壓縮設定，使jpeg影像質量與生成的檔案大小（以千位元組為單位）保持平衡，以提高頁面載入速度。 QLT通過調整jpeg壓縮質量設定來定義影像質量。 JPEG大小命令允許您指定使用壓縮需要達到的檔案大小。

## 將CC隱藏式字幕新增至Dynamic Media影片

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

借由附加複製URL以指向其他隱藏式字幕檔案檔案web.VTTsidecar檔案（包含任何視訊的CC資訊），輕鬆將隱藏式字幕新增至Dynamic Media視訊。

## 搭配AEM Dynamic Media使用影像銳利化

此影片說明銳利化影像對維持影像逼真度至關重要的原因，以及如何使用進階設定來製作完美影像。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
