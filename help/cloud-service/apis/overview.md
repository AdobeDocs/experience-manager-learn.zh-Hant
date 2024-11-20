---
title: AEM API概述
description: 瞭解Adobe Experience Manager (AEM)中的各種API型別，並取得以OpenAPI規格為基礎的API (通常稱為「以OpenAPI為基礎的AEM API」)的概觀。
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 316e08e6647d6fd731cd49ae1bc139ce57c3a7f4
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 1%

---

# AEM API概述{#aem-apis-overview}

瞭解Adobe Experience Manager (AEM)as a Cloud Service中的各種API型別，並取得以[OpenAPI Specification (OAS)](https://swagger.io/specification/)為基礎的AEM API (通常稱為OpenAPI為基礎的AEM API)的概觀。

AEM as a Cloud Service提供各種API，用於建立、讀取、更新和刪除內容、資產和表單。 這些API可讓開發人員建立與AEM互動的自訂應用程式。

讓我們來探索AEM中的各種API型別，並瞭解存取AdobeAPI的主要概念。

## AEM API的型別{#types-of-aem-apis}

AEM同時提供舊版和現代API，以便與其作者和發佈服務型別互動。

- **舊版API**：在舊版AEM中推出，舊版API仍支援回溯相容性。

- **現代API**：根據REST、OpenAPI規格，這些API會遵循目前的API設計最佳實務，建議用於新整合。


| AEM API型別 | 規格 | 可用性 | 使用案例 | 範例 |
| --- | --- | --- | --- | --- |
| 傳統（非RESTful） API | Sling Servlet | AEM 6.X、AEM as a Cloud Service | 舊版整合、回溯相容性 | [查詢產生器API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api)及其他 |
| RESTful API | HTTP、JSON | AEM 6.X、AEM as a Cloud Service | CRUD作業，現代應用程式 | [Assets HTTP API](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)、[工作流程REST API](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api)、[內容服務的JSON匯出工具](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter)及其他 |
| GRAPHQL API | GraphQL | AEM 6.X、AEM as a Cloud Service | Headless CMS、SPA、行動應用程式 | [GraphQL API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| OpenAPI型AEM API | REST， OpenAPI | **僅限AEM as a Cloud Service** | API優先開發，現代應用程式 | [Assets作者API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)、[資料夾API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)、[AEM Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/)、[Forms Acrobat服務](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)及其他 |

>[!IMPORTANT]
>
>以OpenAPI為基礎的AEM API僅可在AEM as a Cloud Service中使用，且與AEM 6.X不相容。

如需AEM API的詳細資訊，請參閱[Adobe Experience Manager as a Cloud Service API](https://developer.adobe.com/experience-cloud/experience-manager-apis/)。

讓我們深入瞭解以OpenAPI為基礎的AEM API，以及存取AdobeAPI的重要概念。

## OpenAPI型AEM API{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>以OpenAPI為基礎的AEM API屬於搶先存取計畫的一部分。 如果您有興趣存取這些檔案，建議您傳送電子郵件至[aem-apis@adobe.com](mailto:aem-apis@adobe.com)，並提供使用案例的說明。

[OpenAPI規格](https://swagger.io/specification/) （先前稱為Swagger）是廣泛使用的RESTful API定義標準。 AEM as a Cloud Service提供數種OpenAPI規格型API (或只是OpenAPI型AEM API)，可讓您更輕鬆地建立與AEM的作者或發佈服務型別互動的自訂應用程式。 以下是一些範例：

**個網站**

- [網站API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/)：使用內容片段的API。

**Assets**

- [資料夾API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)：用於處理資料夾的API，例如建立、列出和刪除資料夾。

- [Assets作者API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)：使用資產及其中繼資料的API。

**Forms**

- [Forms Communications API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)：處理表單與檔案的API。

在未來的版本中，將會新增更多以OpenAPI為基礎的AEM API，以支援其他使用案例。

## 驗證支援{#authentication-support}

以OpenAPI為基礎的AEM API支援下列驗證方法：

- **OAuth伺服器對伺服器認證**：適用於需要API存取而不需使用者互動的後端服務。 它使用&#x200B;_client_credentials_&#x200B;授權型別，在伺服器層級啟用安全存取管理。 如需詳細資訊，請參閱[OAuth伺服器對伺服器認證](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential)。

- **OAuth Web App認證**：適用於具有代表使用者存取AEM API的前端和&#x200B;_後端_&#x200B;元件的網頁應用程式。 它使用&#x200B;_authorization_code_&#x200B;授權型別，後端伺服器可在此安全地管理密碼和權杖。 如需詳細資訊，請參閱[OAuth Web App認證](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential)。

- **OAuth單頁應用程式認證**：專為瀏覽器中執行的SPA所設計，需要代表沒有後端伺服器的使用者存取API。 它使用&#x200B;_authorization_code_&#x200B;授權型別，並依賴使用PKCE （代碼交換的證明金鑰）的使用者端安全性機制來保護授權代碼流程。 如需詳細資訊，請參閱[OAuth單頁應用程式認證](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential)。

## 存取Adobe API和相關概念{#accessing-adobe-apis-and-related-concepts}

在存取Adobe API之前，您必須瞭解以下重要概念：

- **[Adobe Developer Console](https://developer.adobe.com/)**：存取AdobeAPI、SDK、即時事件、無伺服器函式等的開發人員中心。 請注意，它與用來偵錯AEM應用程式的&#x200B;_AEM_ Developer Console不同。

- **[Adobe Developer Console專案](https://developer.adobe.com/developer-console/docs/guides/projects/)**：管理API整合、事件和執行階段函式的中心位置。 在這裡，您可以設定API、設定驗證並產生所需的認證。

- **[產品設定檔](https://helpx.adobe.com/tw/enterprise/using/manage-product-profiles.html)**：產品設定檔提供許可權預設集，可讓您控制使用者或應用程式對AEM、Adobe Target、Adobe Analytics等Adobe產品的存取權。 每個Adobe產品都有與其相關的預先定義產品設定檔。

- **服務**：服務會定義實際許可權，並與產品設定檔相關聯。 若要減少或增加許可權預設集，您可以取消選取或選取與產品設定檔關聯的服務。 因此，可讓您控制產品及其API的存取層級。 在AEM as a Cloud Service中，服務代表具有存放庫節點預先定義存取控制清單(ACL)的使用者群組，允許精細的許可權管理。

## 後續步驟{#next-steps}

瞭解不同的AEM API型別，包括
以OpenAPI為基礎的AEM API以及存取AdobeAPI的重要概念，讓您現在可以開始建立與AEM互動的自訂應用程式。

讓我們開始使用[如何叫用OpenAPI型AEM API](invoke-openapi-based-aem-apis.md)教學課程。
