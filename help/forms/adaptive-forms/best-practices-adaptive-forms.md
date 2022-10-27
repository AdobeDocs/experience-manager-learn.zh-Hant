---
title: 建立最適化表單時應遵循的命名慣例和最佳實務
description: 建立最適化表單時應遵循的命名慣例和最佳實務
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
ht-degree: 1%

---

# 最佳作法

Adobe Experience Manager(AEM)表單可協助您將複雜的交易轉換為簡單、令人愉悅的數位體驗。 以下檔案說明開發適用性Forms時需要遵循的一些其他最佳實務。 本檔案旨在搭配 [此文檔](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## 命名慣例

* **面板**
   * 面板名稱是以大寫字元開頭的駝峰式大小寫。

* **表單欄位**
   * 欄位名稱是以小寫字元開頭的駝峰式大小寫。
   * 請勿使用數字啟動欄位名稱
   * 請勿在名稱中加上破折號「 — 」。 這些等同於程式碼中的減號，並將在程式碼中當作運算子。
   * 名稱可包含字母、數字、底線和美元符號。
   * 名稱必須以字母開頭
   * 名稱區分大小寫
   * 保留的字詞（如JavaScript關鍵字）不能用作名稱。 請留意其他AF專屬保留字，例如「面板」、「名稱」。
   * 請勿在名稱中加上破折號「 — 」
* **開發Forms**
   * 開發大型表單時，應考量表單片段。 啟用延遲載入表單片段以縮短載入時間
   * **DataModel**
      * 建議將適用性表單與適當的資料模型關聯
   * **物件事件**
      * 與對象的可見性相關的代碼應始終放置在該對象的可見性事件中。
   * **指令碼**
      * 如果您在適用性表單中撰寫的程式碼延伸超過5行可見，您必須將程式碼移至用戶端程式庫。 最好將函式新增至用戶端程式庫，然後新增適當的jsdoc標籤，讓函式可在適用性表單規則編輯器中顯示。
