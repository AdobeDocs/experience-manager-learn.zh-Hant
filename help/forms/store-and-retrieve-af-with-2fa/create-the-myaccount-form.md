---
title: 建立MyAccountForm
description: 建立myaccount表單以在成功驗證應用程式id和電話號碼時檢索部分完成的表單。
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

表單 **MyAccountForm** 用於在使用者驗證應用程式id和與應用程式id相關聯的行動號碼後，擷取部分完成的最適化表單。

![我的帳戶表單](assets/6599.JPG)

當使用者輸入應用程式ID並按一下 **FetchApplication** 按鈕，則使用表單資料模型的Get操作從資料庫中提取與應用程式ID關聯的移動號碼。

此表單利用表單資料模型的POST調用，使用OTP驗證移動號碼。 使用下列程式碼成功驗證行動號碼時，會觸發表單的提交動作。 我們會觸發提交按鈕(名為 **submitForm**.

>[!NOTE]
> 您必須提供專屬於您 [Nexmo](https://dashboard.nexmo.com/) 帳戶（在MyAccountForm的相應欄位中）

![trigger-submit](assets/trigger-submit.JPG)



此表單與自訂提交動作相關聯，該自訂提交動作會將表單提交轉送至裝載於的servlet **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

裝載於的Servlet中的程式碼 **/bin/renderaf** 轉送請求以呈現預先填入已儲存資料的storeafwithattachments適用性表單。


* MyAccountForm可以是 [從此處下載](assets/my-account-form.zip)

* 範例表單以 [自訂最適化表單範本](assets/custom-template-with-page-component.zip) 需要匯入至AEM，範例表單才能正確轉譯。

* [自訂提交處理常式](assets/custom-submit-my-account-form.zip) 與MyAccountForm提交相關聯的項目必須匯入至AEM。

## 後續步驟

[部署範例資產以測試解決方案](./deploy-this-sample.md)
