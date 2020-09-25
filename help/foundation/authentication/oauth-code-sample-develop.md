---
title: 在AEM中開發OAuth示波器
description: Adobe Experience Manager的可擴充OAuth示波器可讓使用者從用戶授權的用戶端應用程式存取資源。 下圖說明AEM內容中的請求流程。
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: develop
audience: developer
doc-type: code
translation-type: tm+mt
source-git-commit: b351a57e6e5be0fe5696dc09842fa77fdd036a27
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---


# 開發OAuth示波器

Adobe Experience Manager的可擴充OAuth範圍可讓使用者從用戶授權的用戶端應用程式存取資源。 下圖說明AEM內容中的請求流程。

![Oauth示波器流](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM提供3種範圍：

* 設定檔
* 離線存取
* 複寫

AEM的可擴充OAuth範圍可讓您定義其他自訂範圍。 例如，您可以開發自訂範圍並將其部署至AEM，讓透過OAuth授權的行動應用程式只能閱讀，而不能寫入資產。

OAuth是授權用戶端應用程式的偏好方法，因為它使用存取Token，而不需要提供AEM使用者的認證給該應用程式。

* [檢視程式碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
