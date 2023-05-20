---
title: AEM使用本地SDK的無頭快速安AEM裝
description: 從Adobe Experience Manager和AEMGraphQL開始。 安裝AEMSDK、添加示例內容並部署使用其GraphQLAPI的內AEM容的應用程式。 瞭解全AEM渠道體驗的功能。
version: Cloud Service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 1%

---

# AEM使用本地SDK的無頭快速安AEM裝 {#setup}

無AEM頭快速設定可讓您使用WKND站點示例項目中的內容與無頭實AEM際操作，以及一個示例反應應用(aSPA)，該應用會通過無頭GraphQLAPIAEM消耗內容。 本指南使用 [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html)。

## 必備條件 {#prerequisites}

應在本地安裝以下工具：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;order=%40jcr%3內容%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
* [節點.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## 1。安裝AEMSDK {#aem-sdk}

此安裝程式使用 [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) 來探AEM索GraphQLAPI。 本節提供了安裝SDK並在「作者」AEM模式下運行SDK的快速指南。 設定本地開發環境的更詳細指南 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up)。

>[!NOTE]
>
> 也可以在本教程中 [AEMas a Cloud Service](./cloud-service.md)。 在本教程中，還包括有關使用雲環境的其他說明。

1. 導航到 **[軟體分發門戶](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEMas a Cloud Service** 下載 **SDKAEM**。

   ![Software Distribution 入口網站](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. 解壓縮下載並複製快速啟動jar(`aem-sdk-quickstart-XXX.jar`)到專用資料夾，即 `~/aem-sdk/author`。
1. 將jar檔案更名為 `aem-author-p4502.jar`。

   的 `author` name指定快速啟動jar在「作者」模式下啟動。 的 `p4502` 指定在埠4502上運行Quickstart。

1. 要安裝和啟動實AEM例，請在包含jar檔案的資料夾中開啟命令提示符，然後運行以下命令：

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 提供管理員密碼 `admin`。 任何管理員密碼都可接受，但建議使用 `admin` 以減少對重新配置的需求。
1. 服務完AEM成安裝後，應在 [http://localhost:4502](http://localhost:4502)。
1. 使用用戶名登錄 `admin` 和初始啟動時AEM選擇的密碼(通常 `admin`)。

## 2.安裝示例內容 {#install-sample-content}

示例內容 **WKND參考站點** 用於加速本教程。 WKND是一個虛構的生活風格品牌，經常用於培AEM訓。

WKND站點包括公開 [GraphQL端點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html)。 在實際實施中，請遵循記錄的步驟 [包括GraphQL端點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) 在客戶項目中。 A [CORS](#cors-config) 已打包為WKND站點的一部分。 需要CORS配置來授予對外部應用程式的訪問權限，有關 [CORS](#cors-config) 的下界。

1. 下載WKND站AEM點的最新編譯包： [aem輔助線 — wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 確保下載與AEMas a Cloud Service相容的 **不** 這樣 `classic` 。

1. 從 **開AEM始** 菜單，導航至 **工具** > **部署** > **包**。

   ![導航到包](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. 按一下 **上載包** 選擇上一步下載的WKND包。 按一下 **安裝** 安裝軟體包。

1. 從 **開AEM始** 菜單，導航至 **資產** > **檔案** > **WKND共用** > **英語** > **冒險**。

   ![冒險的資料夾視圖](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   這是WKND品牌推廣的各種冒險活動的所有資產的資料夾。 這包括傳統媒體類型（如影像和視頻）和特定於AEM的 **內容片段**。

1. 按一下 **懷俄明州高山滑雪** 資料夾，然後按一下 **懷俄明州高山滑雪內容片段** 卡：

   ![內容片段卡](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. 內容片段編輯器將開啟，用於Dowshing Skiing Wyoming探險。

   ![內容片段編輯器](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   觀察各個領域，如 **標題**。 **說明**, **活動** 定義片段。

   **內容片段** 是管理內容的一種方AEM式。 內容片段是可重用的、與演示無關的內容，由結構化資料元素組成，如文本、富格文本、日期或對其他內容片段的引用。 稍後在快速設定中將更詳細地探索內容片段。

1. 按一下 **取消** 來關閉碎片。 您可以輕鬆瀏覽其他資料夾並瀏覽其他Adventure內容。

>[!NOTE]
>
> 如果使用Cloud Service環境，請參閱有關如何 [將代碼庫（如WKND引用站點）部署到Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version)。

## 3.下載並運行WKND React應用 {#sample-app}

本教程的目標之一是說明如何使用AEMGraphQLAPI從外部應用程式中使用內容。 本教程使用一個示例React App。 React應用程式有意簡單，重點是與GraphQLAPIAEM的整合。

1. 開啟新的命令提示符，並從GitHub克隆示例React應用：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 在中開啟React應用 `aem-guides-wknd-graphql/react-app` 在您選擇的IDE中。
1. 在IDE中，開啟檔案 `.env.development` 在 `/.env.development`。 驗證 `REACT_APP_AUTHORIZATION` 行將取消注釋，檔案將聲明以下變數：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   確保 `REACT_APP_HOST_URI` 指向您的本地AEMSDK。 為方便起見，此快速入門將React應用連接到  **AEM作者**。 **作者** 服務需要身份驗證，因此應用使用 `admin` 用戶建立其連接。 將應用程式連接到AEM Author是開發過程中的常見做法，因為它便於快速迭代內容而無需發佈更改。

   >[!NOTE]
   >
   > 在生產方案中，應用程式將連接到AEM **發佈** 環境。 在 _生產部署_ 的子菜單。


1. 安裝並啟動React應用：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新瀏覽器窗口將自動在 [http://localhost:3000](http://localhost:3000)。

   ![反應入門應用](assets/quick-setup/aem-sdk/react-app__home-view.png)

   顯示來自的冒險內容AEM的清單。

1. 按一下其中一張冒險影像，查看冒險詳細資訊。 請求返AEM回探險的細節。

   ![「冒險詳細資訊」視圖](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. 使用瀏覽器的開發人員工具檢查 **網路** 請求。 查看 **XHR** 請求並觀察多個GET請求 `/graphql/execute.json/...`。 此路徑前置詞調用AEM永續查詢終結點，選擇要使用前置詞後的名稱和編碼參數執行的永續查詢。

   ![GraphQL端點XHR請求](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4.編輯內AEM容

在React應用程式正在運行時，對中的內容進行更新AEM，並查看應用中反映的更改。

1. 導航至AEM [http://localhost:4502](http://localhost:4502)。
1. 導航到 **資產** > **檔案** > **WKND共用** > **英語** > **冒險** > **[巴釐島衝浪營](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**。

   ![巴釐衝浪營地資料夾](assets/setup/bali-surf-camp-folder.png)

1. 按一下 **巴釐島衝浪營** 內容片段以開啟內容片段編輯器。
1. 修改 **標題** 和 **說明** 冒險的故事。

   ![修改內容片段](assets/setup/modify-content-fragment-bali.png)

1. 按一下 **保存** 的子菜單。
1. 在以下位置刷新React應用 [http://localhost:3000](http://localhost:3000) 要查看更改，請執行以下操作：

   ![更新巴釐島衝浪營探險](assets/setup/overnight-bali-surf-camp-changes.png)

## 5.瀏覽GraphiQL {#graphiql}

1. 開啟 [圖形QL](http://localhost:4502/aem/graphiql.html) 通過導航 **工具** > **常規** > **GraphQL查詢編輯器**
1. 選擇左側的現有永續查詢，然後運行這些查詢以查看結果。

   >[!NOTE]
   >
   > GraphiQL工具和GraphQLAPI [在本教程的後面更詳細地探討](../multi-step/explore-graphql-api.md)。

## 恭喜！{#congratulations}

祝賀您，您現在有了一個外部應用程式，它AEM正在與GraphQL共用內容。 您可以隨意檢查React應用程式中的代碼，並繼續嘗試修改現有內容片段。

### 後續步驟

* [啟動無AEM頭教程](../multi-step/overview.md)
