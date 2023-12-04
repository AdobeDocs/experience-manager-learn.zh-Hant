---
title: 設定對 AEM as a Cloud Service 的存取權限
description: AEMas a Cloud Service是雲端原生方式利用AEM應用程式，因此會利用Adobe IMS (Identity Management系統)協助使用者（包括管理員和一般使用者）登入AEM作者服務。 瞭解Adobe IMS使用者、使用者群組和產品設定檔如何與AEM群組和許可權搭配使用，以提供AEM Author的特定存取權。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 148
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 26%

---

# 設定對 AEM as a Cloud Service 的存取權限 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 簡介"
>abstract="AEM as a Cloud Service 使用 Adobe IMS (Identity Management 系統) 協助其使用者 (包括管理員和一般使用者) 登入到 AEM 作者服務。了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。"

AEMas a Cloud Service是雲端原生方式以運用AEM應用程式，因此會利用Adobe IMS (Identity Management系統)協助其使用者（包括管理員和一般使用者）登入AEM作者服務。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。

## Adobe IMS 使用者

需要存取AEM作者服務的使用者管理為 [Adobe IMS使用者](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html) 在 [Adobe的AdminConsole](https://adminconsole.adobe.com). 了解何謂 Adobe IMS 使用者，以及如何在 Admin Console 中對其進行存取和管理。

>[!NOTE]
>
>從AdminConsole刪除IMS使用者時，不會自動從AEM刪除，但AEM工作階段（代號）過期後，就無法登入AEM。


[瞭解Adobe IMS使用者](./adobe-ims-users.md)

## Adobe IMS 使用者群組

應該使用將存取AEM作者服務的使用者組織到邏輯群組中 [Adobe IMS使用者群組](https://helpx.adobe.com/tw/enterprise/using/user-groups.html) 在 [Adobe的AdminConsole](https://adminconsole.adobe.com). Adobe IMS使用者群組不提供直接許可權或對AEM的存取權(這是以下工作： [Adobe IMS產品設定檔](#adobe-ims-product-profiles))但是，它們是定義使用者的邏輯群組的絕佳方法，這些邏輯群組又可以使用AEM群組和許可權轉譯為AEM Author服務中的特定存取層級。

[瞭解Adobe IMS使用者群組](./adobe-ims-user-groups.md)

## Adobe IMS 產品設定檔

[Adobe IMS產品設定檔](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)，管理位置 [Adobe的AdminConsole](https://adminconsole.adobe.com)，是提供 [Adobe IMS使用者](#adobe-ims-users) 以基本存取層級登入AEM Author服務的存取權。

+ 此 __AEM使用者__ 產品設定檔可讓使用者透過AEM貢獻者群組的成員資格，以唯讀方式存取AEM。
+ 此 __AEM管理員__ 產品設定檔為使用者提供AEM的完整管理存取權。

[瞭解Adobe IMS產品設定檔](./adobe-ims-product-profiles.md)

## AEM使用者群組與許可權

Adobe Experience Manager 以 Adobe IMS 使用者、使用者群組和產品設定檔為建置基礎，以提供使用者可自訂的 AEM 存取權。瞭解如何建立AEM群組和許可權，以及它們如何與Adobe IMS抽象化功能搭配運作，以提供順暢且可自訂的AEM存取權。

[瞭解AEM使用者、群組和許可權](./aem-users-groups-and-permissions.md)

## 存取與許可權逐步說明

在Adobe AdminConsole中設定Adobe IMS使用者、使用者群組和產品設定檔的概要說明，以及如何在AEM Author中善用這些Adobe IMS抽象化功能，定義和管理特定的群組型許可權。

[AEM存取與許可權逐步說明](./walk-through.md)

## 其他Adobe Admin Console資源

下列檔案封面 [Adobe Admin Console](https://adminconsole.adobe.com) — 特定的詳細資訊和問題，可能有助於您進一步瞭解Adobe Admin Console，並藉此管理不同Experience Cloud產品的使用者和存取權。

+ [Adobe Admin Console Identity概述](https://helpx.adobe.com/tw/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
