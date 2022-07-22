---
title: 配置對AEMas a Cloud Service的訪問
description: AEMas a Cloud Service是利用應用程式的雲本機方AEM式，因此，利用Adobe IMS(Identity Management系統)來方便用戶（包括管理員和普通用戶）登錄AEM Author服務。 瞭解Adobe IMS用戶、用戶組和產品配置檔案如何與組和權限一起AEM使用，以提供對AEM作者的特定訪問。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 3%

---

# 配置對AEMas a Cloud Service的訪問 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS簡介"
>abstract="AEMas a Cloud Service利用Adobe IMS(Identity Management系統)來方便其用戶（包括管理員和普通用戶）登錄AEM Author服務。 瞭解Adobe IMS用戶、組和產品配置檔案與組和權限一起使用AEM的方式，以便提供對AEM Author服務的細粒度訪問。"

AEMas a Cloud Service是利用應用程式的雲本機方AEM式，因此，利用Adobe IMS(Identity Management系統)來方便其用戶（包括管理員和普通用戶）登錄AEM Author服務。

![Adobe Admin Console](./assets/hero.png)

瞭解Adobe IMS用戶、組和產品配置檔案與組和權限一起使用AEM的方式，以便提供對AEM Author服務的細粒度訪問。

## Adobe IMS用戶

需要訪問AEM作者服務的用戶被管理為 [Adobe IMS用戶](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html) 在 [Adobe的管理控制台](https://adminconsole.adobe.com)。 瞭解Adobe IMS用戶是什麼，以及如何以Admin Console訪問和管理這些用戶。

[瞭解Adobe IMS用戶](./adobe-ims-users.md)

## Adobe IMS用戶組

訪問AEM作者服務的用戶應使用 [Adobe IMS用戶組](https://helpx.adobe.com/enterprise/using/user-groups.html) 在 [Adobe的管理控制台](https://adminconsole.adobe.com)。 Adobe IMS用戶組不提供直接權限或訪問權AEM限(這是 [Adobe IMS產品配置檔案](#adobe-ims-product-profiles))，但是，它們是定義用戶邏輯分組的極好方法，這些分組可以通過使用組和權限轉換到AEM Author服務中的特定訪AEM問級別。

[瞭解Adobe IMS用戶組](./adobe-ims-user-groups.md)

## Adobe IMS產品配置檔案

[Adobe IMS產品配置檔案](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)，管理 [Adobe的管理控制台](https://adminconsole.adobe.com)，是提供 [Adobe IMS用戶](#adobe-ims-users) 具有基本訪問級別的AEM Author服務的登錄權限。

+ 的 __用AEM戶__ product配置檔案為用戶提供通過參與者組中AEM的成員身份進行AEM的只讀訪問。
+ 的 __管AEM理員__ 產品配置檔案為用戶提供完全的管理訪AEM問。

[瞭解Adobe IMS產品配置檔案](./adobe-ims-product-profiles.md)

## AEM用戶組和權限

Adobe Experience Manager以Adobe IMS用戶、用戶組和產品配置檔案為基礎，為用戶提供可自定義的訪問AEM。 瞭解如何構AEM建組和權限，以及它們如何與Adobe IMS抽象協調工作，以提供無縫、可自定義的訪問AEM權限。

[瞭解AEM用戶、組和權限](./aem-users-groups-and-permissions.md)

## 訪問和權限瀏覽

在AdobeAdminConsole中配置Adobe IMS用戶、用戶組和產品配置檔案，以及如何利用AEM作者中的這些Adobe IMS抽象來定義和管理基於組的特定權限的簡略演練。

[訪AEM問和權限瀏覽](./walk-through.md)

## Adobe Admin Console額外資源

以下文檔說明 [Adobe Admin Console](https://adminconsole.adobe.com) — 特定的詳細資訊和關注事項，有助於更好地瞭解Adobe Admin Console，並使用它管理用戶和跨Experience Cloud產品訪問。

+ [Adobe Admin Console身份概述](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
