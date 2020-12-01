---
title: 將AEM的存取權設定為雲端服務
description: 'AEM做為雲端服務是運用AEM應用程式的雲端原生方式，因此，利用Adobe IMS(Identity Management System)來協助管理員和一般使用者登入AEM Author服務。 瞭解Adobe IMS使用者、使用者群組和產品設定檔如何與AEM群組和權限搭配使用，以提供AEM Author的特定存取權。  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---


# 將AEM的存取權設定為雲端服務

AEM做為雲端服務是運用AEM應用程式的雲端原生方式，因此，利用Adobe IMS(Identity Management System)來協助其使用者（包括管理員和一般使用者）登入AEM Author服務。 瞭解Adobe IMS使用者、群組和產品設定檔如何與AEM群組和權限搭配使用，以提供AEM Author服務的精細存取。

## Adobe IMS使用者

需要存取AEM Author服務的使用者在[Adobe的AdminConsole](https://adminconsole.adobe.com)中，會以[Adobe IMS使用者](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html)的身分管理。 瞭解Adobe IMS使用者的身分，以及如何在Admin Console中存取和管理他們。

[瞭解Adobe IMS使用者](./adobe-ims-users.md)

## Adobe IMS使用者群組

存取AEM Author服務的使用者應使用[Adobe IMS使用者群組](https://helpx.adobe.com/enterprise/using/user-groups.html)（位於[Adobe的AdminConsole](https://adminconsole.adobe.com)），組織成邏輯群組。 Adobe IMS使用者群組不提供AEM的直接權限或存取權（這是[Adobe IMS產品設定檔](#adobe-ims-product-profiles)的工作），但是，它們是定義使用者邏輯群組的絕佳方式，進而可在AEM Author服務中，使用AEM群組和權限，將其轉譯為特定存取層級。

[瞭解Adobe IMS使用者群組](./adobe-ims-user-groups.md)

## Adobe IMS產品設定檔

[Adobe IMS產品設定檔](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)(在 [Adobe的AdminConsole中管理)是提供](https://adminconsole.adobe.com)Adobe IMS使用者存取權的機械工，可 [](#adobe-ims-users) 讓Adobe IMS使用者存取AEM Author服務，並具備基本的存取權。

+ __AEM Users__&#x200B;產品設定檔透過AEM的「參與者」群組的會籍，為使用者提供AEM的唯讀存取權。
+ __AEM Administrators__&#x200B;產品設定檔可讓使用者取得對AEM的完整管理存取權。

[瞭解Adobe IMS產品設定檔](./adobe-ims-product-profiles.md)

## AEM使用者群組和權限

Adobe Experience Manager以Adobe IMS使用者、使用者群組和產品設定檔為基礎，讓使用者可自訂存取AEM。 瞭解如何建構AEM群組和權限，以及它們如何與Adobe IMS抽象化搭配運作，以提供順暢且可自訂的AEM存取。

[瞭解AEM使用者、群組和權限](./aem-users-groups-and-permissions.md)

## 存取與權限逐步說明

簡略的說明，在Adobe AdminConsole中設定Adobe IMS使用者、使用者群組和產品設定檔，以及如何運用AEM Author中的這些Adobe IMS抽象化來定義和管理特定群組的權限。

[AEM存取與權限逐步說明](./walk-through.md)

## 其他Adobe Admin Console資源

以下檔案涵蓋[Adobe Admin Console](https://adminconsole.adobe.com)特定的詳細資訊，以及有助於進一步瞭解Adobe Admin Console，並運用它來管理使用者和存取Experience Cloud產品的顧慮。

+ [Adobe Admin Console身分識別總覽](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)