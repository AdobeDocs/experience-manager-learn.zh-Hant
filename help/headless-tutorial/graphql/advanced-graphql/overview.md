---
title: AEM Headless的進階概念 — GraphQL
description: 端對端教學課程，說明Adobe Experience Manager (AEM) GraphQL API的進階概念。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
duration: 441
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 0%

---

# AEM Headless的進階概念

此端對端教學課程繼續進行[基本教學課程](../multi-step/overview.md)，涵蓋Adobe Experience Manager (AEM) Headless和GraphQL的基礎知識。 進階教學課程說明使用內容片段模型、內容片段和AEM GraphQL持續查詢的深入層面，包括在使用者端應用程式中使用GraphQL持續查詢。

## 先決條件

完成AEM as a Cloud Service ](../quick-setup/cloud-service.md)的[快速設定，以設定您的AEM as a Cloud Service環境。

強烈建議您先完成先前的[基本教學課程](../multi-step/overview.md)和[影片系列](../video-series/modeling-basics.md)教學課程，再繼續此進階教學課程。 雖然您可以使用本機AEM環境完成本教學課程，但本教學課程僅涵蓋AEM as a Cloud Service的工作流程。

>[!CAUTION]
>
>如果您無法存取AEM as a Cloud Service環境，可以使用本機SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html)完成[AEM Headless快速設定。 不過，請務必注意，某些產品UI頁面（例如內容片段導覽）是不同的。



## 目標

本教學課程涵蓋下列主題：

* 使用驗證規則和更進階的資料型別（例如Tab預留位置、巢狀片段參照、JSON物件以及日期和時間資料型別）建立內容片段模型。
* 使用巢狀內容和片段參考時製作內容片段，並為內容片段製作控管設定資料夾原則。
* 使用具有變數和指令的AEM查詢，探索GraphQL GraphQL API功能。
* 在AEM中使用引數保留GraphQL查詢，並瞭解如何將快取控制引數用於保留查詢。
* 使用AEM Headless JavaScript SDK將持續查詢請求整合到範例WKND GraphQL React應用程式中。

## AEM Headless的進階概念概覽

以下影片提供本教學課程中所涵蓋概念的高層級概觀。 此教學課程包括使用更進階的資料型別定義內容片段模型、巢狀內容片段，以及在AEM中保留GraphQL查詢。

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>這段影片（於2:25）提到了透過封裝管理員安裝GraphiQL查詢編輯器以探索GraphQL查詢。 但在較新版本的AEM as Cloud Service中，提供了內建的&#x200B;**GraphiQL Explorer**，因此不需要安裝套件。 如需詳細資訊，請參閱[使用GraphiQL IDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html)。


## 專案設定

WKND網站專案具有所有必要的設定，因此您可在完成[快速設定](../quick-setup/cloud-service.md)後立即啟動教學課程。 本節僅重點說明您在建立自己的AEM Headless專案時可以使用的一些重要步驟。


### 檢閱現有設定

在AEM中開始任何新專案的第一步是建立其設定，作為工作區並建立GraphQL API端點。 若要檢閱或建立設定，請瀏覽至&#x200B;**工具** > **一般** > **設定瀏覽器**。

![瀏覽至設定瀏覽器](assets/overview/create-configuration.png)

請注意，`WKND Shared`網站組態已經為教學課程建立。 若要為您自己的專案建立設定，請選取右上角的&#x200B;**建立**，並完成出現的「建立設定」強制回應視窗中的表單。

![檢閱WKND共用組態](assets/overview/review-wknd-shared-configuration.png)

### 檢閱GraphQL API端點

接下來，您必須設定API端點，以將GraphQL查詢傳送至。 若要檢閱現有端點或建立端點，請瀏覽至&#x200B;**工具** > **一般** > **GraphQL**。

![設定端點](assets/overview/endpoints.png)

請注意，`WKND Shared Endpoint`已經建立。 若要建立專案的端點，請選取右上角的&#x200B;**建立**，然後依照工作流程進行。

![檢閱WKND共用端點](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> 儲存端點後，您將會看到造訪安全性控制檯的強制回應視窗，在您想要設定端點的存取許可權時，此功能可讓您調整安全性設定。 然而，安全性許可權本身並不在本教學課程的討論範圍內。 如需詳細資訊，請參閱[AEM檔案](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)。

### 檢閱WKND內容結構和語言根資料夾

明確定義的內容結構是AEM Headless實施成功的關鍵。 它有助於您管理內容的可擴充性、可用性和許可權。

語言根資料夾是以ISO語言代碼作為其名稱（例如EN或FR）的資料夾。 AEM翻譯管理系統使用這些資料夾來定義內容的主要語言和內容翻譯的語言。

移至&#x200B;**導覽** > **Assets** > **檔案**。

![瀏覽至檔案](assets/overview/files.png)

導覽至&#x200B;**WKND共用**&#x200B;資料夾。 觀察標題為「English」且名稱為「EN」的資料夾。 此資料夾是WKND網站專案的語言根資料夾。

![英文資料夾](assets/overview/english.png)

針對您自己的專案，在設定中建立語言根資料夾。 如需詳細資訊，請參閱[建立資料夾](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders)的相關章節。

### 將設定指派給巢狀資料夾

最後，您必須將專案的設定指派給語言根資料夾。 此指派可讓您根據專案設定中定義的內容片段模型來建立內容片段。

若要將語言根資料夾指派給組態，請選取資料夾，然後在頂端導覽列中選取&#x200B;**屬性**。

![選取屬性](assets/overview/properties.png)

接著，導覽至&#x200B;**雲端服務**&#x200B;標籤，並在&#x200B;**雲端設定**&#x200B;欄位中選取資料夾圖示。

![雲端設定](assets/overview/cloud-conf.png)

在出現的強制回應視窗中，選取您先前建立的設定，以將語言根資料夾指派給它。

### 最佳做法

以下是在AEM中建立您自己的專案時的最佳實務：

* 資料夾階層模型化時應考慮到本地化和翻譯。 換言之，語言資料夾應巢狀內嵌於設定資料夾中，如此可讓您輕鬆翻譯這些設定資料夾中的內容。
* 資料夾階層應保持平坦且直接。 請避免在稍後移動或重新命名資料夾和片段，尤其是在發佈以供即時使用後，因為這會變更路徑，進而影響片段參考和GraphQL查詢。

## 入門和解決方案套件

有兩個AEM **套件**&#x200B;可供使用，並可透過[套件管理員](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)安裝

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip)稍後會在教學課程中使用，並包含範例影像和資料夾。
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip)包含第1-4章的完成解決方案，包括新的內容片段模式、內容片段和持續的GraphQL查詢。 對於想要直接跳到[使用者端應用程式整合](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)章節的人很有用。


[React應用程式 — 進階教學課程 — WKND Adventures](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md)專案可供檢閱和探索範例應用程式。 此範例應用程式會叫用持續存在的AEM查詢，以從GraphQL擷取內容，並在沈浸式體驗中呈現。

## 快速入門

若要開始使用這個進階教學課程，請依照下列步驟進行：

1. 使用[AEM as a Cloud Service](../quick-setup/cloud-service.md)設定開發環境。
1. 開始有關[建立內容片段模型](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md)的教學課程章節。
