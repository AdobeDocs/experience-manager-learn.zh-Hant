---
title: 建立MyAccountForm
description: 建立myaccount表單，在成功驗證應用程式ID和電話號碼時擷取部分完成的表單。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 1%

---



# 建立MyAccountForm

在用戶驗證了應用程式ID和與應用程式ID相關聯的移動號碼之後，使用&#x200B;**MyAccountForm**&#x200B;格式來檢索部分完成的自適應表單。

![我的帳戶表單](assets/6599.JPG)

當用戶輸入應用程式ID並按一下&#x200B;**FetchApplication**&#x200B;按鈕時，使用表單資料模型的「獲取」操作從資料庫中獲取與應用程式ID相關的移動號碼。

此表單利用表單資料模型的POST調用來驗證使用OTP的移動號碼。 使用下列程式碼成功驗證行動電話號碼時，會觸發表單的提交動作。 我們正在觸發名為&#x200B;**submitForm**&#x200B;的提交按鈕的click事件。

>[!NOTE]
> 您必須在MyAccountForm的適當欄位中提供您[Nexmo](https://dashboard.nexmo.com/)帳戶專屬的API金鑰和API密碼值

![trigger-submit](assets/trigger-submit.JPG)



此表單與將表單提交轉發到&#x200B;**/bin/renderaf**&#x200B;上裝載的servlet的自定義提交操作相關聯

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

裝載於&#x200B;**/bin/renderaf**&#x200B;的servlet中的代碼會轉送請求，以呈現已預先填入儲存資料的附件自適應表單。


* MyAccountForm可從此處[下載](assets/my-account-form.zip)

* 範例表單以[自訂最適化表單範本](assets/custom-template-with-page-component.zip)為基礎，需要匯入至範例表AEM單才能正確呈現。

* [與MyAccountForm](assets/custom-submit-my-account-form.zip) 提交關聯的自訂提交處理器需要匯入至AEM。
