---
title: 瞭解Adobe Managed Services上AEM的Adobe IMS驗證
description: Adobe Experience Manager針對AEM例項和Adobe IMS（身分管理系統）在受管理服務上的AEM提供Admin Console支援。   此整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 「使用者」和「群組」可指派給與AEM例項相關聯的產品設定檔，以授與特定AEM例項的集中管理存取權。
version: 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---


# 瞭解Adobe Managed Services上AEM的Adobe IMS驗證{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager針對AEM例項和Adobe IMS（身分管理系統）在受管理服務上的AEM提供Admin Console支援。   此整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 使用者和群組可指派給與AEM例項相關聯的產品設定檔，以授與特定AEM例項的集中管理存取權。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS驗證支援僅適用於「內部」使用者（作者、審閱者、管理員、開發人員等），而不適用於外部使用者，例如網站訪客。
* [Admin ](https://adminconsole.adobe.com/) Console會將AEM Managed Services客戶代表為IMS組織，而AEM例項則代表為產品內容。管理控制台系統和產品管理員可以定義和管理。
* AEM Managed Services會將您的拓撲與Admin Console同步，以建立「產品內容」與AEM例項之間的1對1對應。
* Admin Console中的產品設定檔將決定使用者可存取的AEM例項。
* 驗證支援包括客戶SAML2相容的IDP，以進行SSO。
* 僅支援Enterprise ID或Federated ID（針對客戶SSO）（不支援個人Adobe ID）。

** AEM 6.4 SP3及更新版本支援Adobe Managed Services客戶使用此功能。*

## 最佳做法{#best-practices}

### 在管理控制台中套用權限

在Admin Console和Adobe Experience Manager中，應避免在使用者層級套用權限和存取權。

在Admin Console中，使用者應透過「產品內容」層級的「使用者群組」來授與存取權。 使用者群組通常以組織內的邏輯角色來表示，以提升群組在Adobe Experience Cloud產品中的可重複性。

>[!NOTE]
>
> 如果將AEM當做雲端服務使用，請直接將Admin Console使用者指派至「產品設定檔」。 AEM不支援將Admin Console使用者透過Admin Console使用者群組轉換為產品設定檔的權限，做為雲端服務。

### 在Adobe Experience Manager中套用權限

在Adobe Experience Manager中，與Adobe IMS同步的使用者群組應以辭彙新增至[AEM提供的使用者群組](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)，這些使用者群組已預先設定好在AEM中執行特定工作組的適當權限。 從Adobe IMS同步的使用者不應直接新增至[AEM提供的使用者群組](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)。
