---
title: 建立元件以列出表單資料
description: 有關建立摘要元件的教學課程，以在提交前檢閱表單資料。
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 33
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# 建立元件以彙總表單資料

已建立簡單元件以列出表單資料以供檢閱。 [Guidebridge API的造訪函式](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit)已用於反複檢查表單欄位。 與此元件相關聯的clientlibrary程式碼會取得表單上的面板/表格元件。 此面板/表格元件表單欄位標題、值和SOM運算式中的子元素是使用GuidBridge API方法擷取。 然後會使用標題、值和SOM運算式建構簡單的HTML表格，以供一般使用者在提交表單前檢閱/編輯表單資料。

例如，下面的熒幕擷圖顯示您建立的表格，列出&#x200B;**YourDetails**&#x200B;的欄位及其值。 TR中的最後一個TD用於使用欄位SOM運算式編輯欄位值。

![visit-func](assets/visit-function.png)

## 後續步驟

[在本機系統上測試解決方案](./deploy-on-your-system.md)
