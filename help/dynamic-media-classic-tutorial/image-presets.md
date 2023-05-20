---
title: 影像預設集
description: Dynamic Media Classic的「影像預設」包含建立特定大小、格式、質量和銳化的影像所需的所有設定。 「影像預設」是動態調整的關鍵元件。 當您查看Dynamic Media Classic的URL時，可以輕鬆查看是否使用了影像預設。 瞭解「影像預設」、它們為何如此有用以及如何建立它們。
feature: Dynamic Media Classic, Image Presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# 影像預設集 {#image-presets}

「影像預設」實質上是包含建立特定大小、格式、質量和銳化的影像所需的所有設定的配方。 「影像預設」是動態調整的關鍵元件。

如果您查看幾乎任何Dynamic Media Classic客戶的URL，您可能會看到正在使用的影像預設。 只需在URL的末尾查找$name$（用任何詞或詞替換名稱）。

「影像預設」可縮短URL，因此您不必為每個請求寫出多個「影像服務」說明，而可以編寫單個「影像預設」。 例如，這兩個URL會生成具有銳化功能的300 x 300JPEG影像，但第二個URL會使用「影像預設」：

![影像](assets/image-presets/image-preset-2.png)

「影像預設」的真實值是，任何公司管理員可以更新該影像預設的定義，並使用該格式影響每個影像，而無需更改任何Web代碼。 清除URL的快取後，您將看到對「影像預設」進行任何更改的結果。

>[!IMPORTANT]
>
>調整影像大小時，應始終保持影像寬高比、影像寬度與影像高度的比例成比例，以使影像不失真。

影像預設在其名稱的兩側都有一個美元符號($)，並跟在問號(?)之後 分隔符號.

>[!TIP]
>
>在站點上按唯一影像大小建立一個影像預設。 例如，如果您需要產品詳細資訊頁面的350 X 350影像、瀏覽/搜索頁面的120 X 120影像和交叉銷售/特色項目的90 X 90影像，則您需要三個影像預設，無論您有500個影像還是500,000個影像。

- 瞭解有關 [影像預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html)。
- 瞭解如何 [建立影像預設](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset)。

## 影像預設和銳化

「影像預設」通常會調整影像的大小，而每當從原始大小調整影像大小時，應添加銳化。 這是因為調整大小會導致許多像素合併並混合到一個較小的空間中，使影像看起來柔和且模糊。 銳化可增加影像中邊緣和高對比度區域的對比度。

我們預計，在全尺寸觀看時，您上傳到Dynamic Media Classic的高解析度影像不需要任何銳化功能 — 當放大後。 但是，在任何較小的尺寸下，通常都需要加強一些。

>[!TIP]
>
>調整影像大小時始終銳化！ 這意味著您需要將銳化功能添加到每個影像預設（以及稍後我們將討論的查看器預設）。
>
>如果你的影像看起來不好，可能是因為需要銳化，或者一開始質量很差。

增加多少銳化完全是主觀的。 有些人喜歡更柔和的影像，而另一些人則非常敏銳。 通過在影像上運行銳化濾波器的組合來容易增強影像。 但是，也容易過頭，過銳化。

下圖顯示了三個銳化級別。 從右到左，你沒有銳化，只有正確的數量，而且太多。

![影像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic允許三種銳化：簡單銳化、重新取樣模式和非銳化蒙版。

瞭解有關 [Dynamic Media Classic銳化選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image)。

## 其他資源

[影像預設指南](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf)。 用於優化影像質量和載入速度的設定。

[影像是所有內容之二：質量與速度決非模糊](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/)。 討論使用影像預設來提供高質量、快速載入影像的部落格。
