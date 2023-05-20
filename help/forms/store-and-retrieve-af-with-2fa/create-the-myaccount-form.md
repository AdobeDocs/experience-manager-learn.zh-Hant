---
title: 建立MyAccountForm
description: 建立myaccount表單以在成功驗證應用程式ID和電話號碼時檢索部分完成的表單。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# 建立MyAccountForm

窗體 **我的帳戶表單** 在用戶驗證了應用程式ID和與應用程式ID相關聯的移動號碼之後，使用該格式來檢索部分完成的自適應格式。

![我的帳戶表](assets/6599.JPG)

當用戶輸入應用程式ID並按一下 **提取應用程式** 按鈕，使用表單資料模型的「獲取」操作從資料庫中獲取與應用程式id關聯的移動號碼。

此表單利用表單資料模型的POST調用來使用OTP驗證移動號碼。 使用以下代碼成功驗證移動號碼時，將觸發表單的提交操作。 我們正在觸發名為的提交按鈕的按一下事件 **提交表單**。

>[!NOTE]
> 您需要提供特定於您的API密鑰和API密碼值 [內克莫](https://dashboard.nexmo.com/) 帳戶在MyAccountForm的相應欄位中

![觸發提交](assets/trigger-submit.JPG)



此表單與將表單提交轉發到裝載在上的Servlet的自定義提交操作關聯 **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

Servlet中裝載的代碼 **/bin/renderaf** 轉發請求，以呈現預填有已保存資料的帶附件的自適應表單。


* MyAccountForm可以是 [從此處下載](assets/my-account-form.zip)

* 示例表單基於 [自定義自適應表單模板](assets/custom-template-with-page-component.zip) 要正確呈現樣AEM式，需要導入到。

* [自定義提交處理程式](assets/custom-submit-my-account-form.zip) 需要將與MyAccountForm提交關聯的檔案導入AEM。

## 後續步驟

[通過部署示例資產test解決方案](./deploy-this-sample.md)
