---
title: 建置查詢介面
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
source-wordcount: '130'
ht-degree: 1%

---

# 建置查詢介面

已建置簡單的查詢介面，以允許「管理員」輸入搜尋條件，以擷取特定表單提交。 然後結果會以簡單的表格格式顯示，並附上檢視特定表單提交的選項。

![query-submissions](assets/query-submissions.png)

此介面中的下拉式清單為階層式下拉式清單。 下拉式清單中可用的選項會根據上一個下拉式清單中所做的選擇而變更。

下拉式清單是使用RESTful資料來源填入。

搜尋結果會顯示在名為「SearchResults」的自訂元件中。 當使用者按一下檢視按鈕時，表單會預先填入提交的資料和附件。

## 後續步驟

[撰寫預填服務](./part4.md)