---
title: 建立MyAccountForm
description: 建立myaccount表單以在成功驗證應用程式ID和電話號碼時擷取部分完成的表單。
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# 建立MyAccountForm

表單&#x200B;**MyAccountForm**&#x200B;用於在使用者驗證應用程式ID以及與應用程式ID關聯的行動電話號碼後，擷取部分完成的最適化表單。

![我的帳戶表單](assets/6599.JPG)

當使用者輸入應用程式ID並按一下&#x200B;**FetchApplication**&#x200B;按鈕時，會使用表單資料模型的「取得」操作，從資料庫擷取與應用程式ID關聯的行動電話號碼。

此表單會利用表單資料模型的POST引動過程，利用OTP驗證行動電話號碼。 使用下列程式碼成功驗證行動電話號碼時，會觸發表單的提交動作。 我們正在觸發名為&#x200B;**submitForm**&#x200B;之提交按鈕的點選事件。

>[!NOTE]
> 您必須在MyAccountForm的適當欄位中提供您[Nexmo](https://dashboard.nexmo.com/)帳戶專屬的API金鑰和API機密值

![觸發器提交](assets/trigger-submit.JPG)



此表單與自訂提交動作相關聯，該自訂提交動作會將表單提交轉寄到掛載在&#x200B;**/bin/renderaf**&#x200B;上的servlet

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

掛接在&#x200B;**/bin/renderaf**&#x200B;上的servlet中的程式碼會轉寄請求，以轉譯storeafwithattachments已預先填入已儲存資料的最適化表單。


* MyAccountForm可從這裡[下載](assets/my-account-form.zip)

* 範例表單是以[自訂最適化表單範本](assets/custom-template-with-page-component.zip)為基礎，必須匯入AEM才能正確呈現範例表單。

* [與MyAccountForm提交相關的自訂提交處理常式](assets/custom-submit-my-account-form.zip)需要匯入至AEM。

## 後續步驟

[部署範例資產以測試解決方案](./deploy-this-sample.md)
