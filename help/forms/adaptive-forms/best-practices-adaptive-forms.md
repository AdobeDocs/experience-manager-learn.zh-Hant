---
title: 建立最適化表單時應遵循的命名慣例和最佳實務
seo-title: 建立最適化表單時應遵循的命名慣例和最佳實務
description: 建立最適化表單時應遵循的命名慣例和最佳實務
seo-description: 建立最適化表單時應遵循的命名慣例和最佳實務
feature: 適用性表單
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# 最佳作法

Adobe Experience Manager(AEM)表單可協助您將複雜的交易轉換為簡單、令人愉悅的數位體驗。 以下檔案說明開發適用性Forms時需要遵循的一些其他最佳實務。 本文檔應與[本文檔](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)一起使用

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
   * 保留的字詞（如JavaScript關鍵字）不能用作名稱。 請留意其他AF專屬保留字，例如   為&quot;panel&quot;、&quot;name&quot;。
   * 請勿在名稱中加上破折號「 — 」
* **開發Forms**
   * 開發大型表單時，應考量表單片段。 啟用延遲載入表單片段以加快載入速度   times
   * **DataModel**
      * 建議將適用性表單與適當的資料模型關聯
   * **物件事件**
      * 與對象的可見性相關的代碼應始終放置在該對象的可見性事件中。
   * **指令碼**
      * 如果您在適用性表單中撰寫的程式碼延伸超過5行可見，您必須將程式碼移至用戶端程式庫。 最好將函式新增至用戶端程式庫，然後新增適當的jsdoc標籤，讓函式可在適用性表單規則編輯器中顯示。


