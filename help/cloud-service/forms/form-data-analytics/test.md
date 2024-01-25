---
title: 使用Adobe Analytics提交表單資料欄位的相關報表
description: 將AEM Forms CS與Adobe Analytics整合以報告表單資料欄位
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 44
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# 測試您的解決方案

使用數個表單值組合來預覽及提交表單。 請等數到30分鐘，在Adobe Analytics報表中檢視您的資料。 設為prop的資料比設為eVar的資料更早顯示在報表中。

## 報表套裝

Adobe Analytics擷取的表單資料會以環形圖格式呈現

**依據州的提交內容**

![applicantsbystate](assets/donut.png)

欄位驗證錯誤

![field-validation-error](assets/donut-field-validation.png)

## 偵錯

確定最適化表單使用的設定容器與包含Adobe啟動設定的設定容器相同。

若要確認表單正在將資料傳送至Adobe Analytics，請執行下列動作

* 開啟瀏覽器中的開發人員工具。
* 在Console面板的下列文字中輸入。

```javascript
_satellite.setDebug(true)
```

與表單互動，同時保持主控台視窗開啟。 您應該會看到類似這樣的內容

![console-debug](assets/debug.png)

## 使用Adobe Experience Platform Debugger

新增 [AEP Debugger擴充功能](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) （您必須登入）以取得更多偵錯資訊

![platform-debugger](assets/platform-debugger.png)

## 恭喜

您已成功將AEM Formsas a Cloud Service與Adobe Analytics整合，以報告表單資料欄位。