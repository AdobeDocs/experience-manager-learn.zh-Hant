---
title: 交付互動式通信文檔 — Web渠道AEM Forms
description: 通過電子郵件中的連結傳送Web渠道文檔
feature: Interactive Communication
audience: developer
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# Web渠道文檔的電子郵件傳遞

定義並測試Web渠道交互通信文檔後，您需要一個傳遞機制將Web渠道文檔傳遞給收件人。

本文將電子郵件作為Web渠道文檔的傳遞機制來進行研究。 收件人將通過電子郵件獲取指向Web通道文檔的連結。按一下該連結時，將要求用戶進行身份驗證，並且Web通道文檔將填充特定於登錄用戶的資料。

讓我們看一下以下代碼段。 此代碼是GET.jsp的一部分，當用戶按一下電子郵件中的連結以查看Web渠道文檔時，該代碼將觸發。 我們使用jackrabbit UserManager獲取登錄用戶。 獲得登錄用戶後，我們將獲取與用戶配置檔案關聯的accountNumber屬性的值。

然後，我們將accountNumber值與映射中名為accountnumber的鍵關聯。 鍵 **帳號** 在表單資料模式中定義為「請求屬性」。 此屬性的值將作為輸入參數傳遞給表單資料模式讀取服務方法。

第7行：我們將根據互動式通信文檔url標識的資源類型將接收的請求發送到另一個servlet。 由第二servlet返回的響應包括在第一servlet的響應中。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![包括方法方法](assets/includemethod.jpg)

第7行代碼的可視表示

![請求參數配置](assets/requestparameter.png)

為表單資料模式的讀取服務定義的請求屬性

[示例包AEM](assets/webchanneldelivery.zip)。
