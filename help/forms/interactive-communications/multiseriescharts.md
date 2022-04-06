---
title: AEM Forms
seo-title: Multi Series Charts in AEM Forms
description: 建立適當的表單資料模型以在打印文檔和Web渠道文檔中建立多系列圖表。
seo-description: Create appropriate Form Data Model to create multi series charts in print and web channel documents.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# 多系列圖表

AEM Forms6.5引入了建立和配置多個系列圖表的能力。 多個系列圖表通常與折線圖、條形圖、柱形圖類型關聯使用。 下圖是多系列圖的一個很好的示例。 圖表顯示了3隻不同共同基金在一段時間內的1萬美元增長。 要能夠在AEM Forms建立和使用此類圖表，您需要建立相應的表單資料模型。

![多序列](assets/seriescharts.jfif)

要在AEM Forms建立多系列圖表，您需要建立具有必要實體和實體間關聯的適當表單資料模型。 以下螢幕抓圖將加亮圖元和3個圖元之間的關聯。 在最高一級，我們有一個實體，稱為&quot;組織&quot;，它與基金實體有一對多的聯繫。 基金實體與業績實體有一對多的聯繫。

![formdatamodel](assets/formdatamodel.jfif)


## 為多系列圖建立表單資料模型

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 配置折線圖

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


要在系統上test此功能，請執行以下步驟

* [使用包管理器下載並導AEM入MutualFundFactSheet.zip。](assets/mutualfundfactsheet.zip)
* [將SeriesChartSampleData.json下載到硬碟。](assets/serieschartsampledata.json) 這是將用於填充圖表的示例資料。
* [導航到「Forms」和「文檔」。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 輕輕選擇「MutualFundGrowthFactSheet」互動式通信模板。
* 按一下預覽 |打印通道 |上載示例資料。
* 瀏覽到作為本文一部分提供的示例資料檔案。
* 預覽「MutualFundGrowthFactSheet」互動式通信的打印渠道，以及上一步下載的示例資料。
