---
title: 設定存取權限至 AEM as a Cloud Service
description: AEM as a Cloud Service是運用AEM應用程式的雲端原生方式，因此可運用Adobe IMS(Identity Management系統)來方便管理員和一般使用者登入AEM Author服務。 了解Adobe IMS使用者、使用者群組和產品設定檔如何與AEM群組和權限搭配使用，以提供AEM作者的特定存取權。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 27%

---

# 設定存取權限至 AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 簡介"
>abstract="AEM as a Cloud Service 使用 Adobe IMS (Identity Management 系統) 協助其使用者 (包括管理員和一般使用者) 登入到 AEM 作者服務。了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。"

AEM as a Cloud Service是運用AEM應用程式的雲端原生方式，因此可運用Adobe IMS(Identity Management系統)來加速其使用者（包括管理員和一般使用者）登入AEM Author服務。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。

## Adobe IMS使用者

需要存取AEM作者服務的使用者管理方式為 [Adobe IMS使用者](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html) in [Adobe的AdminConsole](https://adminconsole.adobe.com). 了解何謂 Adobe IMS 使用者，以及如何在 Admin Console 中對其進行存取和管理。

[了解Adobe IMS使用者](./adobe-ims-users.md)

## Adobe IMS使用者群組

存取AEM作者服務的使用者應使用 [Adobe IMS使用者群組](https://helpx.adobe.com/tw/enterprise/using/user-groups.html) in [Adobe的AdminConsole](https://adminconsole.adobe.com). Adobe IMS使用者群組不提供直接權限或AEM的存取權(這是 [Adobe IMS產品設定檔](#adobe-ims-product-profiles))，不過這是定義使用者邏輯群組的絕佳方式，而邏輯群組又可透過AEM群組和權限，轉譯為AEM Author服務中的特定存取層級。

[了解Adobe IMS使用者群組](./adobe-ims-user-groups.md)

## Adobe IMS產品設定檔

[Adobe IMS產品設定檔](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)，管理 [Adobe的AdminConsole](https://adminconsole.adobe.com)，是提供 [Adobe IMS使用者](#adobe-ims-users) 以基本存取權登入AEM製作服務。

+ 此 __AEM使用者__ 產品設定檔可讓使用者透過AEM貢獻者群組的成員資格，以唯讀方式存取AEM。
+ 此 __AEM管理員__ 產品設定檔可讓使用者完全存取AEM的管理權限。

[了解Adobe IMS產品設定檔](./adobe-ims-product-profiles.md)

## AEM使用者群組和權限

Adobe Experience Manager 以 Adobe IMS 使用者、使用者群組和產品設定檔為建置基礎，以提供使用者可自訂的 AEM 存取權。了解如何建構AEM群組和權限，以及這些群組和權限如何與Adobe IMS抽象化功能搭配運作，以提供順暢且可自訂的AEM存取權。

[了解AEM使用者、群組和權限](./aem-users-groups-and-permissions.md)

## 存取與權限逐步說明

在AdminConsole中設定Adobe IMS使用者、使用者群組和產品設定檔的概要說明，以及如何在AEM作者中運用這些Adobe IMS抽象化功能來定義和管理特定群組型權限。

[AEM存取與權限逐步說明](./walk-through.md)

## 其他Adobe Admin Console資源

以下文檔涵蓋 [Adobe Admin Console](https://adminconsole.adobe.com) — 特定詳細資訊和疑慮，有助於進一步了解Adobe Admin Console，並使用它管理使用者和存取不同Experience Cloud產品。

+ [Adobe Admin Console 身分識別概觀](https://helpx.adobe.com/tw/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
