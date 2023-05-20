---
title: 建立自適應表單時要遵循的命名約定和最佳做法
description: 建立自適應表單時要遵循的命名約定和最佳做法
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 2%

---

# 最佳做法

Adobe Experience Manager(AEM)表格可幫助您將複雜事務轉換為簡單、令人愉快的數字型驗。 以下檔案介紹了在開發適應性Forms時需要遵循的一些其他最佳做法。 本文檔將與 [此文檔](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## 命名慣例

* **面板**
   * 面板名稱是以大寫字元開頭的駝峰大寫。

* **窗體域**
   * 欄位名稱是以小寫字元開頭的駝峰大寫。
   * 不使用數字開始欄位名
   * 不要在名稱中包含短划線「 — 」。 這些將等同於代碼中的減號，並將在代碼中充當運算子。
   * 名稱可以包含字母、數字、下划線和美元符號。
   * 名稱必須以字母開頭
   * 名稱區分大小寫
   * 保留字（如JavaScript關鍵字）不能用作名稱。 注意其他特定於AF的保留字，如「panel」、「name」。
   * 不要在名稱中包含短划線「 — 」
* **發展Forms**
   * 在開發大型形體時，應考慮形體碎片。 啟用表單片段的延遲載入，以加快載入時間
   * **資料模型**
      * 建議將自適應表單與相應的資料模型關聯
   * **對象事件**
      * 與對象的可見性相關的代碼應始終放置在對象的可見性事件中。
   * **指令碼**
      * 如果您在自適應表單中編寫的代碼超過5個可見行，則必須將代碼移到客戶端庫。 理想情況下，將函式添加到客戶端庫，然後添加相應的jsdoc標籤，以允許在「自適應表單」規則編輯器中顯示該函式。
