---
title: 在AEM Forms中使用樣式系統
description: 定義按鈕元件的樣式原則
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 52205a93-d03c-430c-a707-b351ab333939
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 3%

---

# 在元件的原則中定義樣式

* 登入本機雲端就緒AEM執行個體，並導覽至「工具」 | 一般 | 範本 | 您的專案名稱。

* 在編輯模式中選取並開啟&#x200B;**使用核心元件空白**&#x200B;範本。
* 按一下按鈕元件的原則圖示，開啟原則編輯器。

* ![按鈕原則](assets/button-policy.png)

定義原則，如下所示

![button-policy-details](assets/styling-policy.png)

我們已定義2種樣式/變數，稱為「行銷」和「企業」。這些變數與對應的CSS類別相關聯。**請確定CSS類別名稱**前後沒有空格。
儲存您的變更。

| 樣式 | CSS 類別 |
|-----------|------------------------------------|
| 行銷 | cmp-adaptiveform-button — 行銷 |
| 企業 | cmp-adaptiveform-button—corporate |

這些CSS類別將在元件的scss檔案(_button.scss)中定義。

## 後續步驟

[定義CSS類別](./create-variations.md)
