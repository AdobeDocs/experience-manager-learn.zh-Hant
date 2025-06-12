---
title: AEM Headless 的進階概念 - GraphQL
description: 端對端教學課程，說明 Adobe Experience Manager (AEM) GraphQL API 的進階概念。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '1052'
ht-degree: 100%

---

# AEM Headless 的進階概念

本端對端教學課程延續[基礎教學課程](../multi-step/overview.md)的內容，而基礎課程的內容涵蓋 Adobe Experience Manager (AEM) Headless 和 GraphQL 的基礎知識。進階教學課程則會深入說明使用內容片段模型、內容片段和 AEM GraphQL 持續性查詢的情形，包括在用戶端應用程式中使用 GraphQL 持續性查詢。

## 先決條件

完成 [AEM as a Cloud Service 快速設定](../quick-setup/cloud-service.md)，將 AEM as a Cloud Service 環境設定好。

強烈建議您先完成前面的[基礎教學課程](../multi-step/overview.md)和[影片系列](../video-series/modeling-basics.md)教學課程，然後再繼續學習本進階教學課程。雖然您可以使用本機 AEM 環境完成本教學課程，但本教學課程僅涵蓋 AEM as a Cloud Service 的工作流程。

>[!CAUTION]
>
>若您無法存取 AEM as a Cloud Service 環境，可以[使用本機 SDK 完成 AEM Headless 快速設定](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html?lang=zh-Hant)。但需注意，有些產品使用者介面的頁面，例如內容片段導覽，會有所差異。



## 目標

本教學課程涵蓋以下主題：

* 使用驗證規則和更進階的資料類型 (例如索引標籤預留位置、巢狀片段參照、JSON 物件) 及日期與時間資料類型來建立內容片段模型。
* 在使用巢狀內容和片段參照時製作內容片段，並設定內容片段製作治理的資料夾原則。
* 使用具有變數和指令的 GraphQL 查詢探索 AEM GraphQL API 功能。
* 使用 AEM 中的參數持續進行 GraphQL 查詢，並了解如何使用快取控制參數進行持續性查詢。
* 使用 AEM Headless JavaScript SDK 將持續性查詢要求與範例 WKND GraphQL React 應用程式整合。

## AEM Headless 的進階概念概觀

以下影片針對本教學課程所涵蓋之概念提供整體性介紹。本教學課程內容包括使用更進階的資料類型定義內容片段模型、將內容片段巢狀化，以及在 AEM 中進行持續性 GraphQL 查詢。

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>影片 (2:25 處) 提到透過套件管理程式安裝 GraphiQL 查詢編輯器，以便了解 GraphQL 查詢。不過，較新版本的 AEM as Cloud Service 中提供內建的 **GraphiQL Explorer**，因此不需要透過套件來安裝。如需詳細資訊，請參閱[使用 GraphiQL IDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html?lang=zh-Hant)。


## 專案設定

WKND 網站專案已包含所有必要的設定，因此您可以在完成[快速設定](../quick-setup/cloud-service.md)後直接開始進行教學課程。此區段僅重點說明您在建立自己的 AEM Headless 專案時可以使用的一些重要步驟。


### 檢閱現有設定

在 AEM 中開始任何新專案的第一步是建立其作為工作區的設定，以及建立 GraphQL API 端點。若要檢閱或建立設定，請導覽至「**工具**」>「**一般**」>「**設定瀏覽器**」。

![導覽至設定瀏覽器](assets/overview/create-configuration.png)

請注意到，本教學課程已經建立 `WKND Shared` 網站設定。若要為您自己的專案建立設定，請選取右上角的「**建立**」，並填寫出現在「建立設定」模態視窗中的表單。

![檢閱 WKND 共用設定](assets/overview/review-wknd-shared-configuration.png)

### 檢閱 GraphQL API 端點

接下來，您必須設定 API 端點，作為傳送 GraphQL 查詢的目的地。若要檢閱現有的端點或建立一個端點，請導覽至「**工具**」>「**一般**」>「**GraphQL**」。

![設定端點](assets/overview/endpoints.png)

請注意，`WKND Shared Endpoint` 已經建立。若要建立您專案的端點，請選取右上角的「**建立**」，然後按照工作流程進行。

![檢閱 WKND 共用端點](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> 儲存端點後，您會看到一個關於瀏覽安全性主控台的模態視窗，若您想要設定對於端點的存取權，可利用此模態視窗調整安全性設定。不過，本教學課程並未討論安全性權限本身。如需詳細資訊，請參閱 [AEM 文件](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=zh-Hant)。

### 檢閱 WKND 內容結構和語言根資料夾

明確定義的內容結構是 AEM Headless 能成功實施的關鍵，這對於您的內容在擴充性、可用性與權限管理方面都非常有幫助。

語言根資料夾是以 ISO 語言代碼為名稱 (例如 EN 或 FR) 的資料夾。AEM 翻譯管理系統使用這些資料夾來定義內容的主要語言，以及內容翻譯的語言。

前往「**導覽**」>「**資產**」>「**檔案**」。

![導覽至檔案](assets/overview/files.png)

導覽至 **WKND 共用**&#x200B;資料夾。請注意到標題為「英文」而名稱為「EN」的資料夾。此資料夾是 WKND 網站專案的語言根資料夾。

![英文資料夾](assets/overview/english.png)

針對您自己的專案，請在您的設定中建立語言根資料夾。如需詳細資訊，請參閱[建立資料夾](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders)區段。

### 指派設定至巢狀資料夾

最後，您必須將專案的設定指派至語言根資料夾。透過這樣的指派，便能夠根據您的專案設定中所定義的內容片段模型建立內容片段。

若要將語言根資料夾指派至設定，請選取該資料夾，然後選取頂端導覽列中的「**屬性**」。

![選取屬性](assets/overview/properties.png)

接下來，導覽至「**雲端服務**」索引標籤，然後選取「**雲端設定**」欄位中的資料夾圖示。

![雲端設定](assets/overview/cloud-conf.png)

在出現的模態視窗中，選取您先前建立的設定，並將語言根資料夾指派給該設定。

### 最佳實務

在 AEM 中建立您自己的專案時，請遵守以下最佳實務：

* 資料夾階層應根據本地化和翻譯需求進行建模。換句話說，語言資料夾應該嵌套在設定資料夾中，如此便能輕鬆翻譯這些設定資料夾中的內容。
* 資料夾階層應保持扁平和直接。之後應避免移動或重新命名資料夾和片段，尤其是在發佈供即時使用之後，因為路徑會變更而可能影響片段參照和 GraphQL 查詢。

## 入門和解決方案套件

有兩個 AEM **套件**&#x200B;可供使用，且能透過[套件管理程式](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)進行安裝

* 本教學課程的後段會用到 [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)，其中包含範例影像和資料夾。
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) 內有第 1-4 章所用的已完成解決方案，包括新的內容片段模型、內容片段和持續性 GraphQL 查詢。對於想直接跳到[用戶端應用程式整合](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)章節的人來說很有用。


[React 應用程式 - 進階教學課程 - WKND Adventures](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) 專案可供檢閱和探索範例應用程式。此範例應用程式會藉由叫用持續性 GraphQL 查詢從 AEM 擷取內容，並將其轉譯為沉浸式體驗。

## 快速入門

若要開始使用本進階教學課程，請依照以下步驟進行：

1. 使用 [AEM as a Cloud Service](../quick-setup/cloud-service.md) 建立開發環境。
1. 開始進行教學課程的[建立內容片段模型](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)章節。
