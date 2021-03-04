---
title: 建立最適化表單時應遵循的命名慣例和最佳實務
seo-title: 建立最適化表單時應遵循的命名慣例和最佳實務
description: 建立最適化表單時應遵循的命名慣例和最佳實務
seo-description: 建立最適化表單時應遵循的命名慣例和最佳實務
feature: 自適應表單
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# 最佳作法

Adobe Experience Manager(AEM)表格可協助您將複雜的交易轉換為簡單、愉悅的數位體驗。 以下檔案說明在開發適應性Forms時需要遵循的一些其他最佳做法。 本文檔旨在與[本文檔](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)一起使用

## 命名慣例

* **面板**
   * 面板名稱是大寫，開頭為大寫字元。

* **表單欄位**
   * 欄位名稱是大寫，開頭為小寫字元。
   * 不用數字開頭欄位名
   * 請勿將破折號&quot;-&quot;包含在您的名稱中。 這些值等同於程式碼中的減號，並在程式碼中充當運算元。
   * 名稱可以包含字母、數字、下划線和美元符號。
   * 名稱必須以字母開頭
   * 名稱區分大小寫
   * 保留字詞（如JavaScript關鍵字）不能用作名稱。 請留意其他AF專用的保留字詞，例如   &quot;panel&quot;,&quot;name&quot;。
   * 請勿在您的名稱中加入虛線&quot;-&quot;
* **發展Forms**
   * 在開發大型表單時，應考慮表單片段。 啟用表格片段的延遲載入，以加快載入速度   times
   * **DataModel**
      * 建議將最適化表單與適當的資料模型關聯
   * **物件事件**
      * 與對象的可見性相關的代碼應始終放置在該對象的可見性事件中。
   * **指令碼**
      * 如果您在「最適化表單」中編寫的程式碼超過5行，您必須將程式碼移至「用戶端程式庫」。 最理想的是，將函式添加到客戶端庫，然後添加適當的jsdoc標籤，以便在「最適化表單」規則編輯器中顯示該函式。


