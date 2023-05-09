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
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# 建立元件以匯總表單資料

已建立簡單元件，列出表單資料以供檢閱。 此 [指南橋API的造訪功能](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) 用於反覆查看表單欄位。 與此元件相關聯的clientlibrary中的程式碼會取得表單上的面板/表格元件。 從此面板/表元件的子元素中，使用GuidBridge API方法提取表單欄位標題、值和SOM表達式。 然後，使用標題、值和SOM表達式構建一個簡單的HTML表，以便最終用戶在提交表單之前檢視/編輯表單資料。

例如，下方的螢幕擷取畫面會顯示建立的表格，以列出 **YourDetails**. TR中的最後一個TD用於使用欄位SOM表達式編輯欄位的值。

![visit-func](assets/visit-function.png)

## 後續步驟

[在本地系統上測試解決方案](./deploy-on-your-system.md)
