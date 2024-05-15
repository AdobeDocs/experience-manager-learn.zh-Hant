---
title: 建立MyAccountForm
description: 建立myaccount表單以在成功驗證應用程式ID和電話號碼時擷取部分完成的表單。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# 建立MyAccountForm

表單 **我的帳戶表單** 用於在使用者驗證應用程式id以及與應用程式id相關聯的行動電話號碼後，擷取部分完成的最適化表單。

![我的帳戶表單](assets/6599.JPG)

當使用者輸入應用程式ID並按一下 **FetchApplication** 按鈕，會使用表單資料模型的「取得」操作，從資料庫擷取與應用程式ID關聯的行動裝置號碼。

此表單會使用表單資料模型的POST引動，利用OTP驗證行動電話號碼。 使用下列程式碼成功驗證行動電話號碼時，會觸發表單的提交動作。 我們正在觸發提交按鈕的點選事件，按鈕名稱為 **submitform**.

>[!NOTE]
> 您需要提供API金鑰以及特定於您的 [Nexmo](https://dashboard.nexmo.com/) MyAccountForm適當欄位中的帳戶

![trigger-submit](assets/trigger-submit.JPG)



此表單與自訂提交動作相關聯，該動作會將表單提交轉送至掛載的servlet **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

掛載在servlet中的程式碼 **/bin/renderaf** 轉寄請求以轉譯已預先填入已儲存資料的storeafwithattachments最適化表單。


* 我的帳戶表單可以 [已從此處下載](assets/my-account-form.zip)

* 範例表單基於 [自訂自適應表單範本](assets/custom-template-with-page-component.zip) 這些區段需要匯入至AEM，範例表單才能正確呈現。

* [自訂提交處理常式](assets/custom-submit-my-account-form.zip) 需要將與提交的MyAccountForm建立關聯，匯入AEM。

## 後續步驟

[部署範例資產以測試解決方案](./deploy-this-sample.md)
