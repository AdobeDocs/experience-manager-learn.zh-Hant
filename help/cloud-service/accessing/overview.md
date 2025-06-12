---
title: 設定對 AEM as a Cloud Service 的存取權
description: AEM as a Cloud Service 是透過雲端原生方式利用 AEM 應用程式，因此，便能透過 Adobe IMS (身分管理系統) 讓使用者 (包含管理員和一般使用者) 登入 AEM Author 服務時更為方便。了解 Adobe IMS 使用者、使用者群組以及產品設定檔全部如何與 AEM 群組和權限結合使用，以提供對 AEM Author 的特定存取權。
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# 設定對 AEM as a Cloud Service 的存取權限 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 簡介"
>abstract="AEM as a Cloud Service 使用 Adobe IMS (Identity Management 系統) 協助其使用者 (包括管理員和一般使用者) 登入到 AEM 作者服務。了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限搭配使用，以提供對 AEM Author 服務的詳細存取權。"

AEM as a Cloud Service 是透過雲端原生方式利用 AEM 應用程式，因此，便能透過 Adobe IMS (身分管理系統) 讓使用者 (包含管理員和一般使用者) 登入 AEM Author 服務時更為方便。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 使用者、群組和產品設定檔如何與 AEM 群組和權限搭配使用，以提供對 AEM Author 服務的詳細存取權。

## Adobe IMS 使用者

在 [Adobe 的 Admin Console](https://adminconsole.adobe.com) 中，會將需要存取 AEM Author 服務的使用者視為 [Adobe IMS 使用者](https://helpx.adobe.com/tw/enterprise/using/set-up-identity.html)進行管理。了解何謂 Adobe IMS 使用者，以及如何在 Admin Console 中對其進行存取和管理。

>[!NOTE]
>
>從 AdminConsole 中刪除 IMS 使用者時，並不會自動從 AEM 中刪除該使用者，但是在 AEM 工作階段 (權杖) 過期後，該使用者便無法再登入 AEM。


[了解關於 Adobe IMS 使用者的資訊](./adobe-ims-users.md)

## Adobe IMS 使用者群組

應使用 [Adobe AdminConsole](https://adminconsole.adobe.com) 中的 [Adobe IMS 使用者群組](https://helpx.adobe.com/tw/enterprise/using/user-groups.html)，將存取 AEM Author 服務的使用者分組成為邏輯群組。Adobe IMS 使用者群組不會提供 AEM 的直接權限或存取權 (這是 [Adobe IMS 產品設定檔](#adobe-ims-product-profiles)的工作)，不過，這是定義使用者邏輯分組的好方法，因為這些分組可以透過 AEM 群組和權限，再轉換為 AEM Author 服務中特定層級的存取權。

[了解關於 Adobe IMS 使用者群組的資訊](./adobe-ims-user-groups.md)

## Adobe IMS 產品設定檔

[Adobe IMS 產品設定檔](https://helpx.adobe.com/tw/enterprise/using/manage-permissions-and-roles.html)由 [Adobe AdminConsole](https://adminconsole.adobe.com) 進行管理，是讓 [Adobe IMS 使用者](#adobe-ims-users)能夠以基本層級的存取權登入 AEM Author 服務的機制。

+ __AEM 使用者__&#x200B;產品設定檔讓使用者能透過 AEM 的投稿人群組會籍獲得 AEM 的唯讀存取權限。
+ __AEM 管理員__&#x200B;產品設定檔提供使用者 AEM 完整的管理存取權。

[了解關於 Adobe IMS 產品設定檔的資訊](./adobe-ims-product-profiles.md)

## AEM 使用者群組和權限

Adobe Experience Manager 以 Adobe IMS 使用者、使用者群組和產品設定檔為基礎進行建置，以提供使用者可自訂的 AEM 存取權。了解如何建構 AEM 群組和權限，以及其如何與 Adobe IMS 抽象化功能搭配運作，以提供順暢且可自訂的 AEM 存取權。

[了解關於 AEM 使用者、群組和權限的資訊](./aem-users-groups-and-permissions.md)

## 存取權和權限逐步解說

一段精簡的逐步解說，說明在 Adobe AdminConsole 中如何設定 Adobe IMS 使用者、使用者群組和產品設定檔，以及如何在 AEM Author 中善用這些 Adobe IMS 抽象化功能來定義和管理基於特定群組的權限。

[AEM 存取權和權限逐步解說](./walk-through.md)

## 其他 Adobe Admin Console 資源

以下文件涵蓋 [Adobe Admin Console](https://adminconsole.adobe.com) 特定的詳細資訊和考量事項，有助於更加理解 Adobe Admin Console，以及如何用來管理各個 Experience Cloud 產品的使用者和存取權。

+ [Adobe Admin Console 身分識別概觀](https://helpx.adobe.com/tw/enterprise/using/identity.html)
+ [Adobe Admin Console 管理員角色](https://helpx.adobe.com/tw/enterprise/using/admin-roles.html)
+ [Adobe Admin Console 開發人員角色](https://helpx.adobe.com/tw/enterprise/using/manage-developers.html)
