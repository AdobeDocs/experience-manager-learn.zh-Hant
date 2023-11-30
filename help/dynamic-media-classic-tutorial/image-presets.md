---
title: 影像預設集
description: Dynamic Media Classic中的影像預設集包含以特定大小、格式、品質和銳利化建立影像所需的所有設定。 影像預設集是動態調整大小的關鍵元件。 當您在Dynamic Media Classic中檢視URL時，可以輕鬆檢視影像預設集是否在使用中。 瞭解影像預設集、它們為何如此實用以及如何建立一個。
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# 影像預設集 {#image-presets}

影像預設集基本上是包含以特定大小、格式、品質和銳利化建立影像所需的所有設定的配方。 影像預設集是動態調整大小的關鍵元件。

如果您檢視幾乎任何Dynamic Media Classic客戶的URL，您可能會看到使用中的影像預設集。 只要在URL結尾尋找$name$ （使用任何取代名稱的字詞或單字）。

影像預設集能縮短URL，因此與其在每個請求中寫出數個「影像伺服」指示，您可以撰寫單一影像預設集。 例如，這兩個URL會產生相同的300 x 300JPEG銳利化影像，但第二個使用影像預設集：

![影像](assets/image-presets/image-preset-2.png)

影像預設集的真正價值在於，任何公司管理員都可以更新該影像預設集的定義，並使用該格式影響每個影像，而無需變更任何網頁程式碼。 在URL的快取清除後，您將會看到影像預設集變更的結果。

>[!IMPORTANT]
>
>調整影像大小時，外觀比例（影像寬度與高度的比例）應一律保持成比例，以免影像失真。

影像預設集名稱兩側都有一個貨幣符號($)，並緊跟在問號(？)後面。 分隔符號.

>[!TIP]
>
>針對網站上每個不重複影像大小，建立一個影像預設集。 例如，如果您需要產品詳細資料頁面的350 X 350影像、瀏覽/搜尋頁面的120 X 120影像，以及交叉銷售/精選專案的90 X 90影像，則無論您有500個影像或500,000個影像，都需要三個影像預設集。

- 進一步瞭解 [影像預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- 瞭解如何 [建立影像預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## 影像預設集和銳利化

影像預設集通常會調整影像大小，而每次根據原始大小調整影像大小時，您都應該增加銳利化。 這是因為調整大小會導致許多畫素合併並混合到較小的空間，使影像看起來既柔和又模糊。 銳利化會增加影像中邊緣和高對比區域的對比。

我們期望您上傳至Dynamic Media Classic的高解析度影像在全尺寸檢視時不需要任何銳利化，當縮放至時。 不過，無論尺寸如何較小，通常都需要進行某些銳利化。

>[!TIP]
>
>調整影像大小時一律銳利化！ 這表示您必須在每個影像預設集（和檢視器預設集，稍後我們將討論）中新增銳利化。
>
>如果影像看起來不好，可能是因為影像需要銳利化，或可能一開始品質不佳。

銳利化程度的增加完全是主觀的。 有些人喜歡較柔和的影像，其他人則喜歡非常銳利的影像。 在影像上執行多種銳利化濾鏡的組合，可輕鬆增強影像。 不過，您也可以輕鬆將影像移到外部並過度銳利化。

下圖顯示三個銳利化等級。 從右至左，沒有銳利化，只有適當的數量，而且過多。

![影像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic允許三種型別的銳利化：簡單銳利化、重新取樣模式和遮色片銳利化。

進一步瞭解 [Dynamic Media Classic銳利化選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## 其他資源

[影像預設集指南](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). 用來最佳化影像品質和載入速度的設定。

[影像是第2部分：絕不僅僅是模糊 — 品質與速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). 討論使用影像預設集提供高品質、快速載入影像的部落格。
