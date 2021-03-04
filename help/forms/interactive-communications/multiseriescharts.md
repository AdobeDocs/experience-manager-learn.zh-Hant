---
title: AEM Forms多系列圖
seo-title: AEM Forms多系列圖
description: 建立適當的表單資料模型，以在列印和Web頻道檔案中建立多系列圖表。
seo-description: 建立適當的表單資料模型，以在列印和Web頻道檔案中建立多系列圖表。
feature: 互動式通訊
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 1%

---


# 多系列圖

AEM Forms6.5引入了建立和設定多個系列圖表的功能。 多個系列圖通常與折線圖、橫條圖、圓柱圖類型關聯使用。 下圖是多系列圖的一個好範例。 圖表顯示了3個不同共同基金在一段時間內的1萬美元增長。 若要在AEM Forms建立和使用此類圖表，您必須建立適當的表單資料模型。

![多系列](assets/seriescharts.jfif)

要在AEM Forms建立多系列圖表，您需要建立具有必要實體和實體之間關聯的適當表單資料模型。 以下螢幕擷取會反白顯示3個實體之間的實體和關聯。 在頂層，我們有一個實體，叫做&quot;組織&quot;，它與基金實體有一對多的聯繫。 基金實體又與業績實體有一對多關係。

![formdatamodel](assets/formdatamodel.jfif)


## 建立多系列圖表的表單資料模型

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 配置折線圖

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


要在系統上測試此功能，請遵循以下步驟

* [使用Package Manager下載並匯入MutualFundFactSheetAEM.zip。](assets/mutualfundfactsheet.zip)
* [將SeriesChartSampleData.json下載至硬碟。](assets/serieschartsampledata.json) 這是用於填入圖表的範例資料。
* [導覽至「Forms」和「檔案」。](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* 輕輕選擇「MutualFundGrowthFactSheet」互動式通訊範本。
* 按一下「預覽」 |上傳範例資料。
* 瀏覽至本文中提供的範例資料檔案。
* 預覽「MutualFundGrowthFactSheet」互動式通訊的列印渠道，以及在前一步驟中下載的範例資料。
