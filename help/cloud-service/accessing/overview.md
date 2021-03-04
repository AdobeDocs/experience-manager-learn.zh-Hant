---
title: 將訪AEM問配置為Cloud Service
description: '因AEM為Cloud Service是運用應用程式的雲端原生方式AEM，因此，會運用AdobeIMS(Identity Management系統)來協助管理員和一般使用者登入AEM Author服務。 瞭解AdobeIMS使用者、使用者群組和產品設定檔如何與群組和權限搭配使用，AEM以提供AEM Author的特定存取權。  '
feature: 使用者和群組
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: 管理、安全性
role: 管理員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---


# 將訪AEM問配置為Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="AdobeIMS簡介"
>abstract="由於AEMCloud Service運用AdobeIMS(Identity Management系統)來協助其管理員和一般使用者的使用者登入AEM Author服務。 瞭解AdobeIMS使用者、群組和產品設定檔如何與群組和權限搭配使AEM用，以提供對AEM Author服務的精細存取。"

因AEM為Cloud Service是運用應用程式的雲端原生方式AEM，因此，它會運用AdobeIMS(Identity Management系統)來協助其使用者（包括管理員和一般使用者）登入AEM Author服務。 瞭解AdobeIMS使用者、群組和產品設定檔如何與群組和權限搭配使AEM用，以提供對AEM Author服務的精細存取。

## AdobeIMS使用者

需要存取AEM Author服務的使用者在[Adobe的AdminConsole](https://adminconsole.adobe.com)中，被管理為[AdobeIMS使用者](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html)。 瞭解哪些AdobeIMS使用者，以及如何在Admin Console中存取和管理他們。

[瞭解AdobeIMS使用者](./adobe-ims-users.md)

## AdobeIMS使用者群組

存取AEM Author服務的使用者應使用[AdobeIMS使用者群組[Adobe的AdminConsole](https://adminconsole.adobe.com)組織成邏輯群組。 ](https://helpx.adobe.com/enterprise/using/user-groups.html)AdobeIMS使用者群組不提供直接權限或存取權AEM(這是[AdobeIMS產品設定檔](#adobe-ims-product-profiles)的工作)，但是，它們是定義使用者邏輯群組的絕佳方式，而邏輯群組又可使用群組和權限，在AEM Author服務中轉譯為特定存取AEM層級。

[瞭解AdobeIMS使用者群組](./adobe-ims-user-groups.md)

## AdobeIMS產品設定檔

[AdobeIMS產品設定檔](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)(在 [Adobe的AdminConsole中管理](https://adminconsole.adobe.com))是提供 [](#adobe-ims-users) AdobeIMS使用者存取權，以基本存取權登入AEM Author服務的修理工。

+ __AEM Users__&#x200B;產品設定檔可讓使用者透過「參與者」群組的會籍，以唯AEM讀方式存AEM取。
+ __管理員AEM__&#x200B;產品設定檔可讓使用者存取完整的管理AEM權限。

[瞭解AdobeIMS產品設定檔](./adobe-ims-product-profiles.md)

## AEM使用者群組和權限

Adobe Experience Manager以AdobeIMS使用者、使用者群組和產品設定檔為基礎，讓使用者可自訂存取權AEM。 瞭解如何建AEM構群組和權限，以及它們如何與AdobeIMS抽象化搭配運作，提供順暢且可自訂的存取AEM。

[瞭解使AEM用者、群組和權限](./aem-users-groups-and-permissions.md)

## 存取與權限逐步說明

簡略的說明，可在AdobeAdminConsole中設定AdobeIMS使用者、使用者群組和產品設定檔，以及如何運用AEM Author中的這些AdobeIMS抽象化來定義和管理特定群組的權限。

[AEM存取與權限逐步](./walk-through.md)

## Adobe Admin Console其他資源

以下文檔涉及[Adobe Admin Console](https://adminconsole.adobe.com)的具體細節和顧慮，這些細節和顧慮有助於更好地瞭解Adobe Admin Console，並使用它來管理用戶並訪問Experience Cloud產品。

+ [Adobe Admin Console身份概觀](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)