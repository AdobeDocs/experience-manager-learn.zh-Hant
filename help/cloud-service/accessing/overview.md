---
title: 將AEM的存取設定為Cloud Service
description: 'AEM as aCloud Service是運用AEM應用程式的雲端原生方式，因此可運用AdobeIMS(Identity Management系統)來方便管理員和一般使用者登入AEM製作服務。 了解AdobeIMS使用者、使用者群組和產品設定檔如何與AEM群組和權限搭配使用，以提供AEM作者的特定存取權。  '
feature: 使用者和群組
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: 管理、安全
role: Admin
level: Beginner
source-git-commit: 36346a8a45fc20b5e6d71d6d00345d74b6b04c2a
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 1%

---


# 將AEM的存取設定為Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="AdobeIMS簡介"
>abstract="AEM as aCloud Service運用AdobeIMS(Identity Management系統)來加速其使用者（包括管理員和一般使用者）登入AEM製作服務。 了解AdobeIMS使用者、群組和產品設定檔如何與AEM群組和權限搭配使用，以提供對AEM作者服務的微調存取。"

AEM as aCloud Service是運用AEM應用程式的雲端原生方式，因此可運用AdobeIMS(Identity Management系統)來便利管理員和一般使用者登入AEM製作服務。

![Adobe Admin Console](./assets/hero.png)

了解AdobeIMS使用者、群組和產品設定檔如何與AEM群組和權限搭配使用，以提供對AEM作者服務的微調存取。

## AdobeIMS使用者

需要存取AEM製作服務的使用者，在[Adobe的AdminConsole](https://adminconsole.adobe.com)中會以[AdobeIMS使用者](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html)來管理。 了解哪些AdobeIMS使用者，以及如何在Admin Console中存取和管理他們。

[了解AdobeIMS使用者](./adobe-ims-users.md)

## AdobeIMS使用者群組

存取AEM作者服務的使用者應使用[AdobeIMS使用者群組](https://helpx.adobe.com/enterprise/using/user-groups.html)(位於[Adobe的AdminConsole](https://adminconsole.adobe.com)中)，組織為邏輯群組。 AdobeIMS使用者群組不提供AEM的直接權限或存取權(這是[AdobeIMS產品設定檔](#adobe-ims-product-profiles)的工作)，不過，這是定義使用者邏輯群組的絕佳方式，而這些邏輯群組又可透過AEM群組和權限，轉譯為AEM製作服務中的特定存取層級。

[了解AdobeIMS使用者群組](./adobe-ims-user-groups.md)

## AdobeIMS產品設定檔

[AdobeIMS產品設定檔](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)(在Adobe的 [AdminConsole](https://adminconsole.adobe.com)中管理)是技工，提供AdobeIMS使 [用](#adobe-ims-users) 者存取權，以基本層級的存取權登入AEM製作服務。

+ __AEM使用者__&#x200B;產品設定檔可讓使用者透過AEM貢獻者群組的成員資格，以唯讀方式存取AEM。
+ __AEM管理員__&#x200B;產品設定檔可讓使用者完整存取AEM。

[了解AdobeIMS產品設定檔](./adobe-ims-product-profiles.md)

## AEM使用者群組和權限

Adobe Experience Manager以AdobeIMS使用者、使用者群組和產品設定檔為基礎，提供使用者可自訂的AEM存取權。 了解如何建構AEM群組和權限，以及這些群組和權限如何與AdobeIMS抽象化功能搭配運作，以提供順暢且可自訂的AEM存取權。

[了解AEM使用者、群組和權限](./aem-users-groups-and-permissions.md)

## 存取與權限逐步說明

在AdminConsole中設定AdobeIMS使用者、使用者群組和產品設定檔的概要說明，以及如何在AEM作者中運用這些AdobeIMS抽象化功能來定義和管理特定群組型權限。

[AEM存取與權限逐步說明](./walk-through.md)

## 其他Adobe Admin Console資源

下列檔案涵蓋[Adobe Admin Console](https://adminconsole.adobe.com)特定詳細資訊和疑慮，有助於進一步了解Adobe Admin Console，並運用它管理使用者和存取Experience Cloud產品。

+ [Adobe Admin Console Identity概述](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)