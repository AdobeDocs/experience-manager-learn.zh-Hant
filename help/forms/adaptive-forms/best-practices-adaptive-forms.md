---
title: 建立調適型表單時應遵循的命名慣例和最佳實務
description: 建立調適型表單時應遵循的命名慣例和最佳實務
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 74
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# 最佳做法

Adobe Experience Manager (AEM)表單可協助您將複雜的交易轉換為簡單、愉快的數位體驗。 以下檔案說明開發最適化Forms時需要遵循的一些其他最佳實務。 本檔案旨在結合 [本檔案](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## 命名慣例

* **面板**
   * 面板名稱是以大寫字元開頭的駝峰式大小寫。

* **表單欄位**
   * 欄位名稱是以小寫字元開頭的駝峰式大小寫。
   * 欄位名稱不能以數字開頭
   * 請勿在名稱中包含破折號「 — 」。 這些將等於程式碼中的減號，並做為程式碼中的運運算元。
   * 名稱可包含字母、數字、底線和美元符號。
   * 名稱必須以字母開頭
   * 名稱區分大小寫
   * 保留字（例如JavaScript關鍵字）不能作為名稱使用。 請留意其他AF專屬保留字詞，例如「panel」、「name」。
   * 請勿在您的名稱中包含破折號「 — 」
* **開發Forms**
   * 開發大型表單時應考量表單片段。 啟用緩慢載入表單片段，以加快載入時間
   * **資料模型**
      * 建議將最適化表單與適當的資料模型建立關聯

   * **物件事件**
      * 與物件可視性相關的程式碼應始終置於所述物件的可視性事件中。
   * **指令碼**
      * 如果您在最適化表單中撰寫的程式碼延伸超過5行可見，您必須將程式碼移至使用者端資料庫。 理想情況下，請將函式新增至使用者端程式庫，然後新增適當的jsdoc標籤，讓函式顯示在最適化表單規則編輯器中。
