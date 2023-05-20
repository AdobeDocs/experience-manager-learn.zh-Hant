---
title: 建立要列出表單資料的元件
description: 用於建立摘要元件以在提交之前查看表單資料的教程。
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

建立了一個簡單元件以列出要查看的表單資料。 的 [導橋API的訪問函式](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) 用於在窗體域中循環。 與此元件關聯的客戶端庫中的代碼獲取窗體上的面板/表元件。 從此面板/表元件的子元素中，使用GuidBridge API方法提取欄位標題、值和SOM表達式。 然後，使用標題、值和SOM表達式構造一個簡單的HTML表，供最終用戶在提交表單之前檢視/編輯表單資料。

例如，下面的螢幕抓圖顯示了為列出 **您的詳細資訊**。 TR中的最後一個TD用於使用欄位SOM表達式編輯欄位的值。

![訪問函式](assets/visit-function.png)

## 後續步驟

[Test本地系統上的解決方案](./deploy-on-your-system.md)
