---
title: 了解Adobe Managed Services上的AEM IMS驗證
description: Adobe Experience Manager推出AEM例項和Managed Services上AEM驗證的Adobe IMS(Identity Management系統)Admin Console支援。   此整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可將使用者和群組指派給與AEM例項相關聯的產品設定檔，以授予對特定AEM例項的集中管理存取權。
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# 了解Adobe Managed Services上的AEM IMS驗證{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager推出AEM例項和Managed Services上AEMAdobeIdentity Management系統(IMS)驗證的Admin Console支援。   此整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可將使用者和群組指派給與AEM例項相關聯的產品設定檔，以授予對特定AEM例項的集中管理存取權。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS驗證支援僅適用於「內部」使用者（作者、審核者、管理員、開發人員等），而不適用於外部一般使用者，例如網站訪客。
* [Admin Console](https://adminconsole.adobe.com/) 將AEM Managed Services客戶表示為IMS組織，並將AEM例項表示為產品內容。 Admin Console系統和產品管理員可以定義和管理。
* AEM Managed Services會將拓撲與Admin Console同步，在產品內容與AEM例項之間建立1對1對應。
* Admin Console中的產品設定檔會決定使用者可存取的AEM例項。
* 驗證支援包括符合客戶SAML2的IDP，以執行SSO。
* 僅支援Enterprise ID或Federated ID（適用於客戶SSO）(不支援個人AdobeID)。

*&#42;AEM 6.4 SP3及更新版本支援Adobe Managed Services客戶使用此功能。*

## 最佳實務 {#best-practices}

### 在Admin Console中套用權限

在Admin Console和Adobe Experience Manager中，都應避免在使用者層級套用權限和存取。

在中，Admin Console使用者應透過產品內容層級的使用者群組來授予存取權。 使用者群組通常最能以組織內的邏輯角色來表示，以提升群組在Adobe Experience Cloud產品間的可重複使用性。

>[!NOTE]
>
> 如果使用AEMas a Cloud Service，請直接將Admin Console使用者指派至產品設定檔。 AEMas a Cloud Service不支援透過Admin Console使用者群組將Admin Console使用者之間的轉換權限傳送至產品設定檔。

### 在Adobe Experience Manager中套用權限

在Adobe Experience Manager中，從Adobe IMS同步的使用者群組應以新增至的辭彙表示 [AEM提供的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html)，此設定已預先設定適當權限，以在AEM中執行特定任務集。 從Adobe IMS同步的使用者不應直接新增至 [AEM提供的使用者群組](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html).
