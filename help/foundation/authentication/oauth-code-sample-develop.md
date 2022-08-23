---
title: 在中開發OAuth范AEM圍
description: Adobe Experience Manager的可擴展OAuth作用域允許從最終用戶授權的客戶端應用程式訪問資源。 下圖說明了在的上下文中的請求AEM流。
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# 開發OAuth範圍

Adobe Experience Manager的可擴展OAuth作用域允許對最終用戶授權的客戶端應用程式的資源進行訪問控制。 下圖說明了在的上下文中的請求AEM流。

![Oauth作用域流](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM提供三個範圍：

* 設定檔
* 離線訪問
* 複寫

可擴AEM展的OAuth作用域允許定義其他自定義作用域。 例如，可以開發並部署自定義範圍，AEM以允許通過OAuth授權的移動應用僅限於讀取，而不能寫入資產。

OAuth是授權客戶端應用程式的首選方法，因為它使用訪問令牌，而AEM不是要求向該應用程式提供用戶憑據。

* [查看代碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
