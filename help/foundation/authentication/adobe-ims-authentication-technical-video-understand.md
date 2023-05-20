---
title: 在Adobe Managed Services上瞭解Adobe IMSAEM驗證
description: Adobe Experience Manager公司推出對實AEM例和基於Adobe IMS(Identity Management系統)的Admin Console支援，以用於AEMManaged Services.   此整合使AEMManaged Services客戶能夠在一個統一的Web控制台中管理所有Experience Cloud用戶。 用戶和組可以分配給與實例關聯的產品配置AEM檔案，從而授予對特定實例的集中管AEM理訪問權。
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
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# 在Adobe Managed Services上瞭解Adobe IMSAEM驗證{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager公司推出對實AEM例和基於AdobeIdentity Management系統(IMS)的Admin Console支援，以AEM便於Managed Services.   此整合使AEMManaged Services客戶能夠在一個統一的Web控制台中管理所有Experience Cloud用戶。 用戶和組可以分配給與實例關聯的產品配置AEM檔案，從而授予對特定實例的集中管理AEM訪問權限。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience ManagerIMS驗證支援僅針對「內部」用戶（作者、審閱者、管理員、開發者等），而不針對外部最終用戶，如網站訪問者。
* [Admin Console](https://adminconsole.adobe.com/) 將AEMManaged Services客戶表示為IMS組織，將實AEM例表示為產品上下文。 Admin Console系統和產品管理員可以定義和管理。
* AEMManaged Services將拓撲與Admin Console同步，在產品上下文和實例之間建立1-1映AEM射。
* Admin Console中的產品配置檔案AEM確定用戶可以訪問哪些實例。
* 身份驗證支援包括客戶符合SAML2的IDP，用於SSO。
* 僅支援企業ID或聯合ID（對於客戶SSO）(不支援個人AdobeID)。

*&#42;6.4 SP3及更AEM高版本支援Adobe Managed Services客戶此功能。*

## 最佳做法 {#best-practices}

### 在Admin Console中應用權限

在Admin Console和Adobe Experience Manager都應避免在用戶級別應用權限和訪問權限。

在中，應通過「產品上下文」級別的「用戶組」授予Admin Console用戶訪問權限。 用戶組通常最好通過組織內的邏輯角色來表達，以提升組在Adobe Experience Cloud產品中的可重用性。

>[!NOTE]
>
> 如果使用AEMas a Cloud Service，請直接將Admin Console用戶分配給產品配置檔案。 對於as a Cloud Service，不支援Admin Console用戶通過Admin Console用戶組到產品配置檔案之間的傳遞AEM權限。

### 在Adobe Experience Manager應用權限

在Adobe Experience Manager，從Adobe IMS同步的用戶組應按術語添加到 [AEM提供的用戶組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)中，預配置了執行特定任務集的適當權限AEM。 不應將從Adobe IMS同步的用戶直接添加到 [AEM提供的用戶組](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)。
