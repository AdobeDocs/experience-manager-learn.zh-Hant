---
title: 使用Analytics報告已提交的表單資料欄位
description: 將AEM Forms CS與Adobe Analytics整合，以報告表單資料欄位
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 4100061624bd8955bee392f1eced20f388f2902c
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# 定義規則

我們在標籤屬性中建立2個新 [規則](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html)(**欄位驗證錯誤和表單提交**)。
![適用性表單](assets/rules.png)


## 欄位驗證錯誤

此 **欄位驗證錯誤** 每當適用性表單欄位中出現驗證錯誤時，就會觸發規則。 例如，在我們的表單中，如果電話號碼或電子郵件未以預期格式顯示驗證錯誤訊息。
欄位驗證錯誤規則是透過將事件設為 _**Adobe Experience Manager Forms錯誤**_ 如螢幕擷取畫面所示

![申請人 — 國家居住地](assets/field_validation_error_rule.png)

Adobe Analytics — 設定變數的設定如下

![設定動作](assets/field_validation_action_rule.png)

## 表單提交規則

每次成功提交適用性表單時，都會觸發表單提交規則。
表單提交規則是使用 _**Adobe Experience Manager Forms — 提交**_ 事件

![form-submit-rule](assets/form-submit-rule.png)

在「表單提交」規則中，資料元素的值 _**ApplicationsStateOfResidence**_ 會對應至prop5，而資料元素FormTitle的值會對應至prop8。

Adobe Analytics — 設定變數的設定如下。
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

當您準備好測試標籤程式碼時，[發佈您對標籤所做的變更](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) 使用發佈流程。
