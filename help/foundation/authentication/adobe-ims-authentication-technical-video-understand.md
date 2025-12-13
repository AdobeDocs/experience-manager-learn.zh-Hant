---
title: 瞭解Adobe Managed Services上使用AEM進行Adobe IMS驗證
description: Adobe Experience Manager推出適用於AEM例項的Admin Console支援，以及適用於Managed Services上AEM的Adobe IMS (Identity Management系統)驗證功能。   這項整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可以將使用者和群組指派給與AEM執行個體相關聯的產品設定檔，授與對特定AEM執行個體的集中管理存取權。
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Developer
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---

# 瞭解Adobe Managed Services上使用AEM進行Adobe IMS驗證{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager推出適用於AEM例項的Admin Console支援，以及適用於Managed Services上Adobe Identity Management System (IMS)的AEM驗證。   這項整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可以將使用者和群組指派給與AEM執行個體相關聯的產品設定檔，授與對特定AEM執行個體的集中管理存取權。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS驗證支援僅適用於「內部」使用者（作者、檢閱者、管理員、開發人員等），不適用於網站訪客等外部一般使用者。
* [Admin Console](https://adminconsole.adobe.com/)將AEM Managed Services客戶表示為IMS組織，並將AEM執行個體表示為產品內容。 Admin Console系統和產品管理員可以定義和管理。
* AEM Managed Services會將您的拓撲與Admin Console同步，在產品內容和AEM執行個體之間建立1對1的對應。
* Admin Console中的產品設定檔能決定使用者可存取的AEM執行個體。
* 驗證支援包括適用於SSO的客戶SAML2相容IDP。
* 僅支援Enterprise ID或Federated ID （適用於客戶SSO） (不支援個人Adobe ID)。

*&#42;Adobe Managed Services客戶的AEM 6.4 SP3和更新版本支援此功能。*

## 最佳做法 {#best-practices}

### 在Admin Console中套用許可權

在Admin Console和Adobe Experience Manager中，都應避免在使用者層級套用許可權和存取權。

在Admin Console中，應透過產品內容層級的使用者群組授予使用者存取權。 使用者群組通常以組織內的邏輯角色最佳表示，以促進群組在Adobe Experience Cloud產品中的重複使用性。

### 在Adobe Experience Manager中套用許可權

在Adobe Experience Manager中，從Adobe IMS同步的使用者群組應以條件新增至[AEM提供的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)，這些群組已預先設定適當許可權，可在AEM中執行特定工作集。 從Adobe IMS同步的使用者不應直接新增至[AEM提供的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)。
