---
title: 建立元件以列出表單資料
description: 提交前建立摘要元件以檢閱表單資料的教學課程。
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
source-git-commit: d3531e76d3341e0964e5ed878fc72037024a11fd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 建立元件以匯總表單資料

已建立簡單元件，列出表單資料以供檢閱。 此 [指南橋API的造訪功能](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) 用於反覆查看表單欄位。 與此元件相關聯的clientlibrary中的程式碼會取得表單上的面板/表格元件。 從此面板/表元件的子元素中，使用GuidBridge API方法提取表單欄位標題、值和SOM表達式。 然後，使用標題、值和SOM表達式構建一個簡單的HTML表，以便最終用戶在提交表單之前檢視/編輯表單資料。

例如，下方的螢幕擷取畫面會顯示建立的表格，以列出 **YourDetails**. TR中的最後一個TD用於使用欄位SOM表達式編輯欄位的值。

![visit-func](assets/visit-function.png)

