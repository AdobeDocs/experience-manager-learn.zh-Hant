---
title: 瞭解在Adobe Managed Services上使用AdobeAEMIMS驗證
description: Adobe Experience Manager公司推出對實AEM例的Admin Console支援和基於AdobeIMS(Identity Management系統)的AEMManaged Services認證。   此整合可讓AEMManaged Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 用戶和組可以分配給與實例關聯的產品配置AEM檔案，從而授予對特定實例的集中管AEM理訪問權。
version: 6.4, 6.5
feature: 身份驗證
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---


# 瞭解在Adobe Managed Services上使用AEMAdobeIMS驗證{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager公司推出對實AEM例的Admin Console支援和基於AdobeIMS(Identity Management系統)的AEMManaged Services認證。   此整合可讓AEMManaged Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 用戶和組可以分配給與實例關聯的產品配置AEM檔案，從而授予對特定實例的集中管理AEM訪問權。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience ManagerIMS驗證支援僅適用於「內部」使用者（作者、審閱者、管理員、開發人員等），而非外部使用者（例如網站訪客）。
* [Admin ](https://adminconsole.adobe.com/) Console將將AEMManaged Services客戶表示為IMS組織，將例項AEM表示為產品內容。Admin Console系統和產品管理員可以定義和管理。
* AEMManaged Services將拓撲與Admin Console同步，建立產品上下文和實例之間的1對1映AEM射。
* Admin Console中的產品設定檔將決定使用AEM者可存取的例項。
* 驗證支援包括客戶SAML2相容的IDP，以進行SSO。
* 僅支援Enterprise ID或Federated ID（針對客戶SSO）(不支援個人AdobeID)。

** Adobe Managed Services客戶支AEM援6.4 SP3及更新版本的此功能。*

## 最佳做法{#best-practices}

### 在Admin Console中應用權限

在Admin Console和Adobe Experience Manager應避免在用戶級別應用權限和訪問權限。

在Admin Console中，使用者應透過「產品內容」層級的「使用者群組」獲得存取權。 使用者群組通常以組織內的邏輯角色來最佳化，以提升群組在Adobe Experience Cloud產品中的可重複使用性。

>[!NOTE]
>
> 如果使AEM用Cloud Service，請直接將Admin Console用戶分配給產品配置檔案。 不支援將Admin Console使用者透過Admin Console使用者群組轉換至產品設定檔的權AEM限做為Cloud Service。

### 在Adobe Experience Manager應用權限

在Adobe Experience Manager，與AdobeIMS同步的用戶組應以[提供的用戶組&lt;a1/AEM>的術語添加，這些用戶組預先配置了執行中特定任務集的適當權限AEM。 ](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)從AdobeIMS同步的使用者不應直接新增至[提供AEM的使用者群組](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)。
