---
title: 查詢表單提交
description: 多部分教學課程，逐步引導您完成查詢Azure入口網站中儲存之表單提交內容的步驟
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 1%

---

# 建立自訂提交

已寫入自訂提交處理常式，以處理表單提交。 從高層面來看，自訂提交處理常式會執行以下操作

* 擷取已提交表單的名稱。
* 擷取提交的資料。核心元件式表單的提交資料一律為JSON格式。
* 在Azure入口網站中擷取並儲存表單附件。 使用附件的URL更新提交的json資料。
* 建立blob索引標籤 — 尋找表單的可搜尋欄位清單，以及從提交的資料中找出其對應的值。
* 將blob索引標籤與提交的資料建立關聯，並將其儲存在Azure入口網站中。

以下熒幕擷取畫面顯示Azure入口網站中的blob索引標籤

![blob-index-tags](assets/blob-index-tags.png)

自訂提交代碼位於 **_StoreFormDataWithBlobIndexTagsInAzure_** 而且從Azure儲存及擷取資料的程式碼在元件中 **_SaveAndFetchFromAzure_**

## 後續步驟

[建置查詢介面](./part3.md)

