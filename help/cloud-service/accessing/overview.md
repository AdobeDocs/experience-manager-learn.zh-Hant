---
title: 設定存取權限至 AEM as a Cloud Service
description: AEMas a Cloud Service是利用應用程式的雲本機方AEM式，因此，利用Adobe IMS(Identity Management系統)來方便用戶（包括管理員和普通用戶）登錄AEM Author服務。 瞭解Adobe IMS用戶、用戶組和產品配置檔案如何與組和權限一起AEM使用，以提供對AEM作者的特定訪問。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 26%

---

# 設定存取權限至 AEM as a Cloud Service {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 簡介"
>abstract="AEM as a Cloud Service 使用 Adobe IMS (Identity Management 系統) 協助其使用者 (包括管理員和一般使用者) 登入到 AEM 作者服務。了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。"

AEMas a Cloud Service是利用應用程式的雲本機方AEM式，因此，利用Adobe IMS(Identity Management系統)來方便其用戶（包括管理員和普通用戶）登錄AEM Author服務。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限一起使用，以提供對 AEM 作者服務的精細存取。

## Adobe IMS用戶

需要訪問AEM作者服務的用戶被管理為 [Adobe IMS用戶](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html) 在 [Adobe的管理控制台](https://adminconsole.adobe.com)。 了解何謂 Adobe IMS 使用者，以及如何在 Admin Console 中對其進行存取和管理。

>[!NOTE]
>
>當從AdminConsole中刪除IMS用戶時，它不會自動從中刪除AEM，但一旦會話（令牌）AEM過期，它們就無法登錄AEM。


[瞭解Adobe IMS用戶](./adobe-ims-users.md)

## Adobe IMS用戶組

訪問AEM作者服務的用戶應使用 [Adobe IMS用戶組](https://helpx.adobe.com/tw/enterprise/using/user-groups.html) 在 [Adobe的管理控制台](https://adminconsole.adobe.com)。 Adobe IMS用戶組不提供直接權限或訪問權AEM限(這是 [Adobe IMS產品配置檔案](#adobe-ims-product-profiles))，但是，它們是定義用戶邏輯分組的極好方法，這些分組可以通過使用組和權限轉換到AEM Author服務中的特定訪AEM問級別。

[瞭解Adobe IMS用戶組](./adobe-ims-user-groups.md)

## Adobe IMS產品配置檔案

[Adobe IMS產品配置檔案](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)，管理 [Adobe的管理控制台](https://adminconsole.adobe.com)，是提供 [Adobe IMS用戶](#adobe-ims-users) 具有基本訪問級別的AEM Author服務的登錄權限。

+ 的 __用AEM戶__ product配置檔案為用戶提供通過參與者組中AEM的成員身份進行AEM的只讀訪問。
+ 的 __管AEM理員__ 產品配置檔案為用戶提供完全的管理訪AEM問。

[瞭解Adobe IMS產品配置檔案](./adobe-ims-product-profiles.md)

## AEM用戶組和權限

Adobe Experience Manager 以 Adobe IMS 使用者、使用者群組和產品設定檔為建置基礎，以提供使用者可自訂的 AEM 存取權。瞭解如何構AEM建組和權限，以及它們如何與Adobe IMS抽象協調工作，以提供無縫、可自定義的訪問AEM權限。

[瞭解AEM用戶、組和權限](./aem-users-groups-and-permissions.md)

## 訪問和權限瀏覽

在AdobeAdminConsole中配置Adobe IMS用戶、用戶組和產品配置檔案，以及如何利用AEM作者中的這些Adobe IMS抽象來定義和管理基於組的特定權限的簡略演練。

[訪AEM問和權限瀏覽](./walk-through.md)

## Adobe Admin Console額外資源

以下文檔說明 [Adobe Admin Console](https://adminconsole.adobe.com) — 特定的詳細資訊和關注事項，有助於更好地瞭解Adobe Admin Console，並使用它管理用戶和跨Experience Cloud產品訪問。

+ [Adobe Admin Console 身分識別概觀](https://helpx.adobe.com/tw/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
