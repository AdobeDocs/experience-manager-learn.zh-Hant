---
title: 互動式通訊檔案的傳送- Web Channel AEM Forms
seo-title: 互動式通訊檔案的傳送- Web Channel AEM Forms
description: 透過電子郵件中的連結傳送網路通路檔案
seo-description: 透過電子郵件中的連結傳送網路通路檔案
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 0%

---


# Web Channel檔案的電子郵件傳送

在您定義並測試網路頻道互動式通訊檔案後，您需要一種傳送機制，將網路頻道檔案傳送給收件者。

在本文中，我們將電子郵件視為Web通路檔案的傳遞機制。 收件者會透過電子郵件取得Web頻道檔案的連結。按一下連結後，系統會要求使用者驗證，並填入Web頻道檔案中的登入使用者專屬資料。

讓我們來看看以下程式碼片段。 此程式碼是GET.jsp的一部分，當使用者按一下電子郵件中連結以檢視網頁頻道檔案時，就會觸發此程式碼。 我們使用Jackrabbit UserManager取得登入的使用者。 在取得登入的使用者後，我們會取得與使用者描述檔相關聯的accountNumber屬性值。

然後，我們會將accountNumber值與地圖中名為accountnumber的索引鍵建立關聯。 關鍵帳 **戶編號** ，在表單資料模式中定義為「請求屬性」。 此屬性的值作為輸入參數傳遞給表單資料模態讀取服務方法。

第7行：我們根據「互動式通訊檔案」URL所識別的資源類型，將收到的請求傳送至另一個servlet。 此第二servlet返回的響應包含在第一servlet的響應中。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![includemethod](assets/includemethod.jpg)

第7行程式碼的視覺呈現

![請求參數](assets/requestparameter.png)

為表單資料模式的讀取服務定義的請求屬性


[範例AEM套件](assets/webchanneldelivery.zip)。
