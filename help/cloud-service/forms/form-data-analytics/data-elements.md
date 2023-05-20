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
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# 建立適當的資料元素

在「標籤」屬性中，我們添加了兩個新資料元素（RapcitsStateOfResidence和validationError）。

![自適應形式](assets/data_elements.png)

## 申請人居住狀態

的 **申請人居住狀態** 通過選擇 **核心** 在擴展下拉清單中 **自定義代碼** 顯示在下面螢幕抓圖中的資料元素類型
![申請人國家住所](assets/applicantstateofresidence.png)

以下自定義代碼用於從 **_狀態_** 自適應窗體欄位。

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## 驗證錯誤

的 **驗證錯誤** 通過選擇 **核心** 在擴展下拉清單中 **自定義代碼** 顯示在下面螢幕抓圖中的資料元素類型

![驗證錯誤](assets/validation-error.png)

編寫了以下自定義代碼以設定validationError資料元素值。

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
