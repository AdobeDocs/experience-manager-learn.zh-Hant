---
title: 建立MyAccountForm
description: 建立myaccount表單，在成功驗證應用程式ID和電話號碼時擷取部分完成的表單。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# 建立MyAccountForm

在使用者 **驗證應用程式ID和與應用程式ID相關的行動電話號碼後，表單MyAccountForm** 可用來擷取部分完成的最適化表單。

![我的帳戶表單](assets/6599.JPG)

當使用者輸入應用程式ID並按一下 **FetchApplication** 按鈕時，會使用表單資料模型的「取得」操作，從資料庫擷取與應用程式ID相關的行動號碼。

此表單利用表單資料模型的POST調用來驗證使用OTP的移動號碼。 使用下列程式碼成功驗證行動電話號碼時，會觸發表單的提交動作。 我們會觸發名為submitForm的submit按鈕的click **事件**。

>[!NOTE]
> 您必須在MyAccountForm的適當欄位中，提供您 [Nexmo](https://dashboard.nexmo.com/) 帳戶專屬的API金鑰和API密碼值

![trigger-submit](assets/trigger-submit.JPG)



此表單與自訂提交操作關聯，該操作會將表單提交轉發到掛載在 **/bin/renderaf上的servlet**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

裝載於 **** /bin/renderaf的servlet中的代碼會轉送請求，以呈現已預先填入已儲存資料的附件最適化表單。


* MyAccountForm可從此 [處下載](assets/my-account-form.zip)

* 範例表單是以自訂 [的最適化表單範本為基礎](assets/custom-template-with-page-component.zip) ，這些範例表單需要匯入AEM才能正確呈現。

* [與MyAccountForm提交](assets/custom-submit-my-account-form.zip) 關聯的自訂提交處理常式必須匯入AEM。
