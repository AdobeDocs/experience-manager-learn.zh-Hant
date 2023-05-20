---
title: 使用Adobe Analytics提交的表單資料欄位報告
description: 將AEM FormsCS與Adobe Analytics整合以報告表單資料欄位
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# Test您的解決方案

使用多個表單值組合預覽和提交表單。 允許幾到30分鐘在Adobe Analytics報告中查看資料。 要使用的資料集在報告中顯示的時間早於資料集到eVars。

## 報表套裝

在Adobe Analytics捕獲的表格資料以甜圈形式呈現

**各國提交**

![應用程式bystate](assets/donut.png)

欄位驗證錯誤

![欄位驗證錯誤](assets/donut-field-validation.png)

## 偵錯

確保自適應表單使用的配置容器與包含Adobe啟動配置的容器相同。

要確認表單正在向Adobe Analytics發送資料，請執行以下操作

* 在瀏覽器中開啟「開發人員工具」。
* 在「控制台」面板中輸入以下文本。

```javascript
_satellite.setDebug(true)
```

在保持控制台窗口開啟的同時與表單交互。 你應該看到這樣的

![控制台調試](assets/debug.png)

## 使用Adobe Experience Platform調試器

添加 [AEP調試器擴展](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) 瀏覽器（需要您登錄才能獲取更多調試資訊）

![平台調試器](assets/platform-debugger.png)
