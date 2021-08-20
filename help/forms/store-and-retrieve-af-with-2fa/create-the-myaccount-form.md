---
title: 建立MyAccountForm
description: 建立myaccount表單以在成功驗證應用程式id和電話號碼時檢索部分完成的表單。
feature: 適用性表單
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: 開發
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 1%

---



# 建立MyAccountForm

表單&#x200B;**MyAccountForm**&#x200B;用於在用戶驗證了應用程式ID和與應用程式ID關聯的移動號碼之後檢索部分完成的最適化表單。

![我的帳戶表單](assets/6599.JPG)

當用戶輸入應用程式id並按一下&#x200B;**FetchApplication**&#x200B;按鈕時，與應用程式id關聯的移動號碼將使用表單資料模型的Get操作從資料庫中獲取。

此表單利用表單資料模型的POST調用，使用OTP驗證移動號碼。 使用下列程式碼成功驗證行動號碼時，會觸發表單的提交動作。 我們正在觸發名為&#x200B;**submitForm**&#x200B;的提交按鈕的點擊事件。

>[!NOTE]
> 您需要在MyAccountForm的適當欄位中提供[Nexmo](https://dashboard.nexmo.com/)帳戶專屬的API金鑰和API密碼值

![trigger-submit](assets/trigger-submit.JPG)



此表單與自訂提交操作相關聯，該自訂提交操作會將表單提交轉發到裝載在&#x200B;**/bin/renderaf**&#x200B;上的servlet

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

裝載於&#x200B;**/bin/renderaf**&#x200B;的servlet中的代碼會轉送請求，以呈現預先填入儲存資料的storeafattachments最適化表單。


* MyAccountForm可從此處[下載](assets/my-account-form.zip)

* 範例表單以[自訂最適化表單範本](assets/custom-template-with-page-component.zip)為基礎，需要匯入至AEM，範例表單才能正確轉譯。

* [與MyAccountForm](assets/custom-submit-my-account-form.zip) 提交相關聯的自訂提交處理程式需要匯入至AEM。
