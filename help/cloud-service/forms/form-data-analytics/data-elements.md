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
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 43
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# 建立資料元素

我們在Tags屬性中新增了兩個新資料元素（ApplicationsStateOfResidence和validationError）。

![adaptive-form](assets/data_elements.png)

## ApplicantStateOfResidence

此 **ApplicantStateOfResidence** 資料元素是透過選取 **核心** 「擴充功能」下拉式清單中的 **自訂程式碼** 的「資料元素型別」，如下方熒幕擷取畫面所示
![applicant-state-residence](assets/applicantstateofresidence.png)

下列自訂程式碼是用來從 **_state_** 最適化表單欄位。

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

此 **ValidationError** 資料元素是透過選取 **核心** 「擴充功能」下拉式清單中的 **自訂程式碼** 的「資料元素型別」，如下方熒幕擷取畫面所示

![validation-error](assets/validation-error.png)

下列自訂程式碼是用來設定 `validationError` 資料元素值。

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## 後續步驟

[建立規則](./rules.md)