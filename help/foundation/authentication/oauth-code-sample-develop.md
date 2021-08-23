---
title: 在AEM中開發OAuth範圍
description: Adobe Experience Manager的可擴充OAuth範圍允許從一般使用者授權的用戶端應用程式存取資源。 下圖說明AEM內容中的請求流程。
version: 6.3, 6.4, 6.5
feature: 使用者和群組
topic: 開發
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---


# 開發OAuth範圍

Adobe Experience Manager可擴充的OAuth範圍允許從一般使用者授權的用戶端應用程式存取資源。 下圖說明AEM內容中的請求流程。

![Oauth作用域流](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM提供三個範圍：

* 設定檔
* 離線存取
* 複寫

AEM可擴充的OAuth範圍可定義其他自訂範圍。 例如，可開發自訂範圍並部署至AEM，讓透過OAuth授權的行動應用程式僅限於閱讀，而非撰寫資產。

OAuth是授權用戶端應用程式的偏好方法，因為它使用存取權杖，而非要求將AEM使用者的認證提供給該應用程式。

* [檢視程式碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
