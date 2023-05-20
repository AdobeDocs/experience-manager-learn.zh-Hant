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
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# 定義規則

在「標籤」屬性中，我們建立了2個新 [規則](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**欄位驗證錯誤和表單提交**)。

![自適應形式](assets/rules.png)


## 欄位驗證錯誤

的 **欄位驗證錯誤** 每次在自適應表單域中出現驗證錯誤時都會觸發規則。 例如，在我們的表單中，如果電話號碼或電子郵件的格式不是預期格式，則會顯示驗證錯誤消息。

通過將事件設定為來配置欄位驗證錯誤規則 _**Adobe Experience Manager Forms錯誤**_ 如螢幕抓圖所示



![申請人國家住所](assets/field_validation_error_rule.png)

Adobe Analytics — 設定變數的配置如下

![設定操作](assets/field_validation_action_rule.png)

## 表單提交規則

每次成功提交自適應表單時都會觸發「表單提交」規則。

表單提交規則是使用 _**Adobe Experience Manager Forms — 提交**_ 事件

![表單提交規則](assets/form-submit-rule.png)

在表單提交規則中，資料元素的值 _**申請人居住狀態**_ 映射到prop5，資料元素FormTitle的值映射到prop8。

Adobe Analytics — 設定變數的配置如下。
![表單提交規則集變數](assets/form-submit-set-variable.png)

當您準備好test標籤代碼時，[發佈對標籤所做的更改](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) 使用發佈流。
