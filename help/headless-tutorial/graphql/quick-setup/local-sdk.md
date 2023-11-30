---
title: 使用本機AEM SDK的AEM Headless快速設定
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 安裝AEM SDK、新增範例內容並使用其GraphQL API部署使用AEM內容的應用程式。 瞭解AEM如何支援全頻道體驗。
version: Cloud Service
mini-toc-levels: 1
jira: KT-6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 1%

---

# 使用本機AEM SDK的AEM Headless快速設定 {#setup}

AEM Headless快速設定讓您使用WKND Site範例專案中的內容，以AEM Headless進行操作，並提供在AEM Headless GraphQL API使用內容的範例React應用程式(SPA)。 本指南使用 [AEMAS A CLOUD SERVICESDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html).

## 先決條件 {#prerequisites}

下列工具應在本機安裝：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14444)
* [Node.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## 1.安裝AEM SDK {#aem-sdk}

此設定使用 [AEMAS A CLOUD SERVICESDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk) 探索AEM GraphQL API。 本節提供快速指南，說明如何安裝AEM SDK並以作者模式執行。 有關設定本機開發環境的更詳細指南 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up).

>[!NOTE]
>
> 您也可以在本教學課程之後，使用 [AEMas a Cloud Service環境](./cloud-service.md). 在本教學課程中，我們還將介紹使用雲端環境的其他注意事項。

1. 導覽至 **[軟體發佈入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEMas a Cloud Service** 並下載最新版的 **AEM SDK**.

   ![Software Distribution 入口網站](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. 解壓縮下載內容並複製Quickstart jar (`aem-sdk-quickstart-XXX.jar`)至專用的資料夾，例如 `~/aem-sdk/author`.
1. 將jar檔案重新命名為 `aem-author-p4502.jar`.

   此 `author` 名稱會指定快速入門Jar以作者模式啟動。 此 `p4502` 指定連線埠4502上的Quickstart執行。

1. 若要安裝和啟動AEM執行個體，請在包含jar檔案的資料夾中開啟命令提示字元，然後執行下列命令：

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 提供管理員密碼作為 `admin`. 可接受任何管理員密碼，但建議使用 `admin` 本機開發，降低重新設定的需求。
1. AEM服務安裝完畢後，新瀏覽器視窗應開啟於 [http://localhost:4502](http://localhost:4502).
1. 以使用者名稱登入 `admin` 和AEM初始啟動期間選取的密碼(通常是 `admin`)。

## 2.安裝範例內容 {#install-sample-content}

內容範例，來自 **WKND參考網站** 用於加速教學課程。 WKND是虛構的生活風格品牌，通常與AEM培訓一起使用。

WKND網站包含公開所需的設定 [GraphQL端點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html). 在真實世界的實作中，請依照檔案說明的步驟 [包含GraphQL端點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html) 位於您的客戶專案中。 A [CORS](#cors-config) 也封裝為WKND網站的一部分。 授予外部應用程式的存取權需要CORS設定，瞭解有關 [CORS](#cors-config) 可以在下方找到。

1. 下載適用於WKND網站的最新編譯的AEM套件： [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > 請務必下載與AEMas a Cloud Service相容的標準版本，並 **非** 此 `classic` 版本。

1. 從 **AEM開始** 功能表，導覽至 **工具** > **部署** > **封裝**.

   ![導覽至封裝](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. 按一下 **上傳套裝** 並選擇在先前步驟中下載的WKND套件。 按一下 **安裝** 以安裝套件。

1. 從 **AEM開始** 功能表，導覽至 **資產** > **檔案** > **WKND已共用** > **英文** > **冒險**.

   ![冒險的資料夾檢視](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   這是構成WKND品牌宣傳的各種冒險的所有資產的資料夾。 這包括影像和視訊等傳統媒體型別，以及AEM特有的媒體，例如 **內容片段**.

1. 按一下 **懷俄明州下山滑雪** 資料夾，然後按一下 **懷俄明州下山滑雪內容片段** 卡片：

   ![內容片段卡片](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. 內容片段編輯器隨即開啟，以進行「懷俄明州下山滑雪」冒險活動。

   ![內容片段編輯器](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   觀察各種欄位，例如 **標題**， **說明**、和 **活動** 定義片段。

   **內容片段** 是在AEM中管理內容的其中一種方式。 內容片段是可重複使用的、與呈現方式無關的內容，由結構化資料元素組成，例如文字、RTF文字、日期或其他內容片段的參考。 內容片段稍後會在快速設定中更詳細地探討。

1. 按一下 **取消** 以關閉片段。 您可以隨意瀏覽至其他資料夾，並探索其他冒險內容。

>[!NOTE]
>
> 如果使用Cloud Service環境，請參閱檔案以瞭解如何 [將程式碼基底（例如WKND參考網站）部署到Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version).

## 3.下載並執行WKND React應用程式 {#sample-app}

本教學課程的目標之一，是說明如何使用GraphQL API取用外部應用程式的AEM內容。 本教學課程使用範例React應用程式。 React應用程式經過刻意簡化，可專注與AEM GraphQL API整合。

1. 開啟新的命令提示字元，並從GitHub複製範例React應用程式：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 在中開啟React應用程式 `aem-guides-wknd-graphql/react-app` 在您選擇的IDE中。
1. 在IDE中，開啟檔案 `.env.development` 在 `/.env.development`. 驗證 `REACT_APP_AUTHORIZATION` 行已取消註解，且檔案會宣告以下變數：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   確定 `REACT_APP_HOST_URI` 指向您的本機AEM SDK。 為方便起見，此快速入門會將React應用程式連線至  **AEM作者**. **作者** 服務需要驗證，因此應用程式會使用 `admin` 建立連線的使用者。 將應用程式連線至AEM Author是開發期間的常見做法，因為這有助於在無需發佈變更的情況下快速迭代內容。

   >[!NOTE]
   >
   > 在生產案例中，應用程式將連線至AEM **發佈** 環境。 有關詳細資訊，請參閱 _生產部署_ 區段。


1. 安裝並啟動React應用程式：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器視窗會自動開啟應用程式，開啟日期： [http://localhost:3000](http://localhost:3000).

   ![React入門應用程式](assets/quick-setup/aem-sdk/react-app__home-view.png)

   隨即顯示AEM冒險內容清單。

1. 按一下其中一個冒險影像以檢視冒險詳細資料。 系統會向AEM提出要求，要求您傳回冒險的詳細資訊。

   ![冒險詳細資料檢視](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. 使用瀏覽器的開發人員工具來檢查 **網路** 要求。 檢視 **XHR** 要求並觀察多個GET要求 `/graphql/execute.json/...`. 此路徑首碼會叫用AEM持續查詢端點，並在首碼之後使用名稱和編碼引數選取要執行的持續查詢。

   ![GraphQL端點XHR要求](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4.在AEM中編輯內容

在React應用程式執行中，更新AEM中的內容，並檢視應用程式中是否反映變更。

1. 導覽至AEM [http://localhost:4502](http://localhost:4502).
1. 瀏覽至 **資產** > **檔案** > **WKND已共用** > **英文** > **冒險** > **[巴厘島衝浪營](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**.

   ![Bali Surf Camp資料夾](assets/setup/bali-surf-camp-folder.png)

1. 按一下 **巴厘島衝浪營** 內容片段，以開啟內容片段編輯器。
1. 修改 **標題** 和 **說明** 探險的魔法。

   ![修改內容片段](assets/setup/modify-content-fragment-bali.png)

1. 按一下 **儲存** 以儲存變更。
1. 重新整理React應用程式，位於 [http://localhost:3000](http://localhost:3000) 若要檢視您的變更：

   ![更新巴厘島衝浪營地冒險](assets/setup/overnight-bali-surf-camp-changes.png)

## 5.探索GraphiQL {#graphiql}

1. 開啟 [GraphiQL](http://localhost:4502/aem/graphiql.html) 瀏覽至 **工具** > **一般** > **GraphQL查詢編輯器**
1. 選取左側的現有持續查詢，並執行它們以檢視結果。

   >[!NOTE]
   >
   > GraphiQL工具和GraphQL API是 [在教學課程稍後章節中更詳細地探討](../multi-step/explore-graphql-api.md).

## 恭喜！{#congratulations}

恭喜，您現在有外部應用程式使用GraphQL的AEM內容。 歡迎在React應用程式中檢查程式碼，並繼續嘗試修改現有的內容片段。

### 後續步驟

* [開始AEM Headless教學課程](../multi-step/overview.md)
