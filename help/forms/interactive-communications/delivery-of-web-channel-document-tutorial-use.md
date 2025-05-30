---
title: 互動式通訊檔案的傳送 — Web Channel AEM Forms
description: 透過電子郵件中的連結傳遞Web Channel檔案
feature: Interactive Communication
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 60
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# Web Channel檔案的電子郵件傳送

定義並測試Web Channel互動式通訊檔案後，您需要一種傳遞機制來將Web Channel檔案傳遞給收件者。

在本文中，我們將電子郵件視為Web Channel檔案的傳送機制。 收件者將透過電子郵件取得網路頻道檔案的連結。按一下連結時，系統會要求使用者進行驗證，且網路頻道檔案會填入登入使用者特有的資料。

讓我們來看看下列程式碼片段。 此程式碼是GET.jsp的一部分，當使用者按一下電子郵件中連結的「 」以檢視Web Channel檔案時，就會觸發此程式碼。 我們會使用jackrabbit UserManager取得登入使用者。 取得登入使用者後，就會取得與使用者設定檔相關聯的accountNumber屬性值。

然後，我們會將accountNumber值與對應中稱為accountnumber的索引鍵建立關聯。 索引鍵&#x200B;**accountnumber**&#x200B;在表單資料模式中定義為請求屬性。 此屬性的值會作為輸入引數傳遞至表單資料模組讀取服務方法。

第7行：我們將根據互動式通訊檔案URL所識別的資源型別，將收到的請求傳送給另一個servlet。 此第二個servlet傳回的回應會包含在第一個servlet的回應中。

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![包含方法方法](assets/includemethod.jpg)

第7行代碼的視覺化表示法

![要求參陣列態](assets/requestparameter.png)

為表單資料強制回應視窗的讀取服務定義的請求屬性

[範例AEM封裝](assets/webchanneldelivery.zip)。
