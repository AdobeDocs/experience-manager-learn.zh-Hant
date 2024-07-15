---
title: 設定對 AEM as a Cloud Service 的存取權限
description: AEM as a Cloud Service是雲端原生運用AEM應用程式的方式，因此會利用Adobe IMS (Identity Management系統)協助使用者（包括管理員和一般使用者）登入AEM Author服務。 瞭解Adobe IMS使用者、使用者群組和產品設定檔如何與AEM群組和許可權搭配使用，以提供AEM Author的特定存取權。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 26%

---

# 設定對 AEM as a Cloud Service 的存取權限 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 簡介"
>abstract="AEM as a Cloud Service 使用 Adobe IMS (Identity Management 系統) 協助其使用者 (包括管理員和一般使用者) 登入到 AEM 作者服務。了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。"

AEM as a Cloud Service是雲端原生運用AEM應用程式的方式，因此會利用Adobe IMS (Identity Management系統)協助其使用者（管理員和一般使用者）登入AEM作者服務。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。

## Adobe IMS 使用者

需要存取AEM Author服務的使用者在[Adobe的AdminConsole](https://adminconsole.adobe.com)中被管理為[Adobe IMS使用者](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html)。 了解何謂 Adobe IMS 使用者，以及如何在 Admin Console 中對其進行存取和管理。

>[!NOTE]
>
>從AdminConsole刪除IMS使用者時，不會自動從AEM刪除，但AEM工作階段（代號）過期後，就無法登入AEM。


[瞭解Adobe IMS使用者](./adobe-ims-users.md)

## Adobe IMS 使用者群組

應該在[Adobe的AdminConsole](https://adminconsole.adobe.com)中，使用[Adobe IMS使用者群組](https://helpx.adobe.com/tw/enterprise/using/user-groups.html)將存取AEM作者服務的使用者組織成邏輯群組。 Adobe IMS使用者群組不提供直接許可權或對AEM的存取權（這是[Adobe IMS產品設定檔](#adobe-ims-product-profiles)的工作），但是，它們很適合用來定義使用者的邏輯群組，進而可以使用AEM群組和許可權轉換為AEM Author服務中的特定存取層級。

[瞭解Adobe IMS使用者群組](./adobe-ims-user-groups.md)

## Adobe IMS 產品設定檔

[在[Adobe的AdminConsole](https://adminconsole.adobe.com)中管理的Adobe IMS產品設定檔](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)是提供[Adobe IMS使用者](#adobe-ims-users)登入AEM Author服務基本存取層級存取權的機械。

+ __AEM使用者__&#x200B;產品設定檔可讓使用者透過AEM貢獻者群組的成員資格以唯讀方式存取AEM。
+ __AEM管理員__&#x200B;產品設定檔可提供使用者對AEM的完整管理存取權。

[瞭解Adobe IMS產品設定檔](./adobe-ims-product-profiles.md)

## AEM使用者群組與許可權

Adobe Experience Manager 以 Adobe IMS 使用者、使用者群組和產品設定檔為建置基礎，以提供使用者可自訂的 AEM 存取權。瞭解如何建立AEM群組和許可權，以及它們如何與Adobe IMS抽象化功能搭配運作，以提供順暢且可自訂的AEM存取權。

[瞭解AEM使用者、群組和許可權](./aem-users-groups-and-permissions.md)

## 存取與許可權逐步說明

在Adobe AdminConsole中設定Adobe IMS使用者、使用者群組和產品設定檔的概要說明，以及如何在AEM Author中善用這些Adobe IMS抽象化功能，定義和管理特定的群組型許可權。

[AEM存取與許可權逐步說明](./walk-through.md)

## 其他Adobe Admin Console資源

下列檔案涵蓋[Adobe Admin Console](https://adminconsole.adobe.com)特定的詳細資訊和顧慮，可能有助於您更深入瞭解Adobe Admin Console，並使用它來管理使用者和跨不同Experience Cloud產品的存取權。

+ [Adobe Admin Console身分總覽](https://helpx.adobe.com/tw/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
