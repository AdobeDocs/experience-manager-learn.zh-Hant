---
title: 交付互動式通訊檔案 — Web Channel AEM Forms
description: 透過電子郵件中的連結傳送Web通道檔案
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

# Web頻道檔案的電子郵件傳送

在定義並測試了Web通道互動式通信文檔後，您需要一種傳遞機制將Web通道文檔傳遞給收件人。

在本文中，我們將電子郵件視為Web通道檔案的傳遞機制。 收件者將通過電子郵件獲得指向Web通道文檔的連結。按一下該連結時，將要求用戶進行身份驗證，並且Web通道文檔將填充登錄用戶的特定資料。

讓我們來看看下列程式碼片段。 此程式碼是GET.jsp的一部分，當使用者點按電子郵件中連結上的以檢視Web頻道檔案時，就會觸發此程式碼。 我們使用jackrabbit UserManager取得登入使用者。 取得登入的使用者後，我們就會取得與使用者設定檔相關聯的accountNumber屬性值。

然後，我們將accountNumber值與映射中名為accountnumber的鍵關聯。 金鑰 **accountnumber** 在表單資料強制回應視窗中定義為「請求屬性」。 此屬性的值會作為輸入參數傳遞至「表單資料模組讀取」服務方法。

第7行：我們會根據互動式通訊檔案url所識別的資源類型，將收到的要求傳送至其他servlet。 第二servlet傳回的回應包含在第一servlet的回應中。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![包含方法方法方法](assets/includemethod.jpg)

第7行代碼的可視表示

![請求參數設定](assets/requestparameter.png)

為表單資料強制回應的讀取服務定義的請求屬性

[範例AEM套件](assets/webchanneldelivery.zip).
