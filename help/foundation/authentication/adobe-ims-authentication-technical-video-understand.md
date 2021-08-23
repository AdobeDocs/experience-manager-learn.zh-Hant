---
title: 了解Adobe Managed Services上的AdobeIMS驗證與AEM
description: Adobe Experience Manager推出AEM例項和AdobeIMS(Identity Management系統)型驗證的Admin Console支援，適用於Managed Services上的AEM。   此整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可將使用者和群組指派給與AEM例項相關聯的產品設定檔，以授予對特定AEM例項的集中管理存取權。
version: 6.4, 6.5
feature: 使用者和群組
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: 架構
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---


# 了解Adobe Managed Services上的AdobeIMS驗證與AEM{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager推出AEM例項和AdobeIMS(Identity Management系統)型驗證的Admin Console支援，適用於Managed Services上的AEM。   此整合可讓AEM Managed Services客戶在單一統一的Web主控台中管理所有Experience Cloud使用者。 可將使用者和群組指派給與AEM例項相關聯的產品設定檔，以授予對特定AEM例項的集中管理存取權。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS驗證支援僅適用於「內部」使用者（作者、審核者、管理員、開發人員等），而不適用於外部一般使用者，例如網站訪客。
* [Admin ](https://adminconsole.adobe.com/) Console會將AEM Managed Services客戶顯示為IMS組織，並將AEM例項表示為產品內容。Admin Console系統和產品管理員可以定義和管理。
* AEM Managed Services會將拓撲與Admin Console同步，在產品內容與AEM例項之間建立1對1對應。
* Admin Console中的產品設定檔將決定使用者可存取的AEM例項。
* 驗證支援包括符合客戶SAML2的IDP，以執行SSO。
* 僅支援Enterprise ID或Federated ID（適用於客戶SSO）(不支援個人AdobeID)。

** AEM 6.4 SP3及更新版本支援Adobe Managed Services客戶使用此功能。*

## 最佳實務 {#best-practices}

### 在Admin Console中套用權限

在Admin Console和Adobe Experience Manager中，都應避免在使用者層級套用權限和存取。

在「Admin Console」中，應透過產品內容層級的使用者群組，授與使用者存取權。 使用者群組通常最能以組織內的邏輯角色來表示，以提升各群組在Adobe Experience Cloud產品中的可重複使用性。

>[!NOTE]
>
> 如果使用AEM作為Cloud Service，請直接將Admin Console使用者指派給產品設定檔。 AEM as a Cloud Service不支援透過Admin Console使用者群組將Admin Console使用者之間的轉換權限傳送至產品設定檔。

### 在Adobe Experience Manager中套用權限

在Adobe Experience Manager中，從AdobeIMS同步的使用者群組應以[AEM提供的使用者群組](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)新增的辭彙表示，這些使用者群組已預先設定適當權限，以在AEM中執行特定任務集。 從AdobeIMS同步的使用者不應直接新增至[AEM提供的使用者群組](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)。
