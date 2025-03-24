---
title: 查詢表單提交
description: 多部分教學課程，逐步引導您完成查詢Azure入口網站中儲存之表單提交內容的步驟
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
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

自訂提交程式碼位於&#x200B;**_StoreFormDataWithBlobIndexTagsInAzure_**&#x200B;中，而用於儲存及擷取Azure資料的程式碼位於元件&#x200B;**_SaveAndFetchFromAzure_**&#x200B;中

## 後續步驟

[建置查詢介面](./part3.md)
