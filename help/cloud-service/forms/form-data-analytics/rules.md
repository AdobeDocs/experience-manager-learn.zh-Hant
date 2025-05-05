---
title: 使用Adobe Analytics提交表單資料欄位的相關報表
description: 將AEM Forms CS與Adobe Analytics整合以報告表單資料欄位
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# 定義規則

在Tags屬性中，我們建立了2個新的[規則](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) （**欄位驗證錯誤和FormSubmit**）。

![最適化表單](assets/rules.png)


## 欄位驗證錯誤

每當最適化表單欄位中出現驗證錯誤時，**欄位驗證錯誤**&#x200B;規則就會觸發。 例如，在我們的表單中，如果電話號碼或電子郵件不是預期的格式，則會顯示驗證錯誤訊息。

如熒幕擷取畫面所示，透過將事件設定為&#x200B;_&#x200B;**Adobe Experience Manager Forms-Error**&#x200B;_&#x200B;來設定[欄位驗證錯誤]規則



![applicant-state-residence](assets/field_validation_error_rule.png)

Adobe Analytics — 設定變數如下

![設定動作](assets/field_validation_action_rule.png)

## 表單提交規則

每次成功提交最適化表單時都會觸發表單提交規則。

表單提交規則是使用&#x200B;_&#x200B;**Adobe Experience Manager Forms — 提交**&#x200B;_&#x200B;事件設定

![form-submit-rule](assets/form-submit-rule.png)

在表單提交規則中，資料元素&#x200B;_&#x200B;**ApplicationsStateOfResidence**&#x200B;_&#x200B;的值對應至prop5，而資料元素FormTitle的值對應至prop8。

Adobe Analytics — 設定變數的設定如下。
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

當您準備好測試您的標籤程式碼時，請[使用發佈流程發佈您對標籤所做的變更](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html)。

## 後續步驟

[測試解決方案](./test.md)