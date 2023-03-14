---
title: 使用Adobe Analytics報告已提交的表單資料欄位
description: 將AEM Forms CS與Adobe Analytics整合，以報告表單資料欄位
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 439167be96959baea54f50a221c6d26f8fab78b2
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# 建立適當的資料元素

在「標籤」屬性中，我們新增了兩個新資料元素（ApplicationsStateOfResidence和validationError）。
![適用性表單](assets/data_elements.png)

## ApplicantStateOfResidence

此 **ApplicantStateOfResidence** 資料元素的設定方式為選取 **核心** 在擴充功能下拉式清單中， **自訂程式碼** 資料元素類型，如下方螢幕擷取畫面所示
![申請人 — 國家居住地](assets/applicantstateofresidence.png)

下列自訂程式碼用於從 **_state_** 最適化表單欄位。

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

此 **驗證錯誤** 資料元素的設定方式為選取 **核心** 在擴充功能下拉式清單中， **自訂程式碼** 資料元素類型，如下方螢幕擷取畫面所示

![驗證錯誤](assets/validation-error.png)

已編寫下列自訂程式碼以設定validationError資料元素值。

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```
