---
title: AEM Forms中的多序圖
seo-title: AEM Forms中的多序圖
description: 建立適當的表單資料模型，以在打印和Web通道文檔中建立多系列圖表。
seo-description: 建立適當的表單資料模型，以在打印和Web通道文檔中建立多系列圖表。
feature: 互動式通訊
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 1%

---


# 多序圖

AEM Forms 6.5導入了建立和設定多個系列圖表的功能。 多個系列圖表通常與折線圖、橫條圖、直條圖類型關聯使用。 下圖是多系列圖表的好範例。 圖表顯示，在一段時間內，3個不同的共同基金中，1萬美元的增長。 若要在AEM Forms中建立和使用這類圖表，您需要建立適當的表單資料模型。

![多序列](assets/seriescharts.jfif)

要在AEM Forms中建立多系列圖表，需要建立適當的表單資料模型，其中包含實體之間的必要實體和關聯。 以下螢幕截圖將加亮3個實體之間的實體和關聯。 在頂層，我們有一個稱為&quot;組織&quot;的實體，它與基金實體有一對多的關聯。 基金實體又與業績實體有一對多的關聯。

![formdatamodel](assets/formdatamodel.jfif)


## 為多序圖建立表單資料模型

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### 配置折線圖

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


要在您的系統上測試，請執行以下步驟

* [使用AEM Package Manager下載並匯入MutualFundFactSheet.zip。](assets/mutualfundfactsheet.zip)
* [將SeriesChartSampleData.json下載至硬碟。](assets/serieschartsampledata.json) 這是將用來填入圖表的範例資料。
* [導覽至Forms和檔案。](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* 輕輕選擇「MutualFundGrowthFactSheet」互動式通信模板。
* 按一下預覽 |上傳範例資料。
* 瀏覽至本文提供的範例資料檔案。
* 預覽「MutualFundGrowthFactSheet」交互通信的打印渠道，其中包含在前一步下載的示例資料。
