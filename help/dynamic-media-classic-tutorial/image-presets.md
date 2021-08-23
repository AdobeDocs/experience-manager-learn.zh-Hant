---
title: 影像預設集
description: Dynamic Media Classic中的影像預設集包含以特定大小、格式、品質和銳利化建立影像所需的所有設定。 影像預設集是動態調整大小的關鍵元件。 當您在Dynamic Media Classic中查看URL時，可以輕鬆查看影像預設集是否在使用中。 了解影像預設集、其為何如此實用，以及如何建立預設集。
sub-product: dynamic-media
feature: Dynamic Media Classic，影像預設集
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: 內容管理
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '706'
ht-degree: 1%

---


# 影像預設集 {#image-presets}

「影像預設集」本質上是一種方式，其中包含以特定大小、格式、品質和銳利化建立影像所需的所有設定。 影像預設集是動態調整大小的關鍵元件。

如果您查看幾乎任何Dynamic Media Classic客戶的URL，可能會看到使用中的影像預設集。 只需在URL的結尾處尋找$name$（任何字詞或字詞都會取代名稱）。

「影像預設集」會縮短URL，因此您不必針對每個請求寫出數個「影像伺服」指示，而是可以撰寫單一「影像預設集」。 例如，這兩個URL會以銳利化方式產生相同的300 x 300 JPEG影像，但第二個URL會使用影像預設集：

![影像](assets/image-presets/image-preset-2.png)

「影像預設集」的真正值是，任何公司管理員都可以更新該影像預設集的定義，並使用該格式影響每個影像，而無需更改任何Web代碼。 在URL的快取清除後，您會看到影像預設集的任何變更結果。

>[!IMPORTANT]
>
>調整影像大小時，應始終保持影像寬寬比與影像高度的比例，以使影像不失真。

影像預設集的名稱兩側都有一個美元符號($)，並且跟在問號(?)後面 分隔符號.

>[!TIP]
>
>根據網站上的不重複影像大小建立一個影像預設集。 例如，如果您需要產品詳細資訊頁面的350 X 350影像、瀏覽/搜尋頁面的120 X 120影像，以及交叉銷售/精選項目的90 X 90影像，則您需要三個影像預設集，無論您有500個影像或500,000個影像。

- 進一步了解[影像預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html)。
- 了解如何[建立影像預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset)。

## 影像預設集和銳利化

「影像預設集」通常會調整影像大小，而每當您從影像原始大小調整影像大小時，應該會新增銳利化。 這是因為調整大小會導致許多像素合併並混合到較小的空間中，使影像看起來柔和且模糊。 銳利化會增加影像中邊緣和高對比區域的對比度。

我們預期，您上傳至Dynamic Media Classic的高解析度影像若放大時，以完整大小檢視時不需要任何銳利化。 然而，在任何較小的尺寸下，通常都需要一些銳利化。

>[!TIP]
>
>調整影像大小時始終銳利化！ 這表示您需要將銳利化新增至每個影像預設集（和檢視器預設集，稍後我們會討論）。
>
>如果您的影像看起來不好，可能是因為需要銳利化，或者一開始畫質可能很差。

增加多少銳利化完全是主觀的。 有些人喜歡更柔和的影像，而有些人則非常銳利。 在影像上執行銳利化濾鏡組合可以很容易地增強影像。 然而，也很容易過頭，過度銳利化影像。

下圖顯示三個銳利化層級。 從右到左，您沒有銳利化，只有正確的數量，而且太多。

![影像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic允許三種銳利化類型：簡單銳利化、重新取樣模式和非銳利化遮色片。

深入了解[Dynamic Media Classic銳利化選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image)。

## 其他資源

[影像預設集指南](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf)。用於最佳化影像品質和載入速度的設定。

[影像是一切第2部分：這絕不只是模糊 — 質量與速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/)。討論使用影像預設集來傳送高品質、快速載入影像的部落格文章。

[影像是網路研討會的一切](https://dynamicmediaseries2019.enterprise.adobeevents.com/)。_影像是所有_&#x200B;系列中所有三個網路研討會的錄影連結。 [網路研討會2](https://seminars.adobeconnect.com/p6lqaotpjnd3) 探討影像預設集。
