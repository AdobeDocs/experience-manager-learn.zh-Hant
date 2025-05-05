---
title: 使用本機AEM SDK的AEM Headless快速設定
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 安裝AEM SDK、新增範例內容，以及使用其GraphQL API部署從AEM使用內容的應用程式。 瞭解AEM如何提供全頻道體驗。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d2da6efa-1f77-4391-adda-e3180c42addc
duration: 242
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 1%

---

# 使用本機AEM SDK的AEM Headless快速設定 {#setup}

AEM Headless快速設定可讓您使用AEM Headless的實際操作，其中包含來自WKND Site範例專案的內容，以及透過AEM Headless GraphQL API使用內容的範例React應用程式(SPA)。 本指南使用[AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html)。

## 先決條件 {#prerequisites}

下列工具應在本機安裝：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14&amp;p.limit=144)
* [Node.js v18](https://nodejs.org/en/)
* [Git](https://git-scm.com/)

## 1.安裝AEM SDK {#aem-sdk}

此設定使用[AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?#aem-as-a-cloud-service-sdk)來探索AEM的GraphQL API。 本節提供快速指南，說明如何安裝AEM SDK，以及如何在作者模式中執行。 如需設定本機開發環境[的詳細指南，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html#local-development-environment-set-up)。

>[!NOTE]
>
> 您也可以使用[AEM as a Cloud Service環境](./cloud-service.md)來學習本教學課程。 在本教學課程中，我們還將介紹使用雲端環境的其他注意事項。

1. 導覽至&#x200B;**[軟體發佈入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)** > **AEM as a Cloud Service**，然後下載最新版本的&#x200B;**AEM SDK**。

   ![軟體發佈入口網站](assets/quick-setup/aem-sdk/downloads__aem-sdk.png)

1. 解壓縮下載內容並將Quickstart jar (`aem-sdk-quickstart-XXX.jar`)複製到專用資料夾，即`~/aem-sdk/author`。
1. 將jar檔案重新命名為`aem-author-p4502.jar`。

   `author`名稱指定Quickstart jar會以作者模式啟動。 `p4502`指定快速入門在連線埠4502上執行。

1. 若要安裝並啟動AEM執行個體，請在包含jar檔案的資料夾中開啟命令提示字元，然後執行下列命令：

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 提供管理員密碼做為`admin`。 可接受任何管理員密碼，但建議使用`admin`進行本機開發，以減少重新設定的需要。
1. 當AEM服務完成安裝時，新的瀏覽器視窗應在[http://localhost:4502](http://localhost:4502)開啟。
1. 使用AEM初始啟動期間選取的使用者名稱`admin`和密碼登入（通常是`admin`）。

## 2.安裝範例內容 {#install-sample-content}

來自&#x200B;**WKND參考網站**&#x200B;的範例內容是用來加速教學課程。 WKND是虛構的生活風格品牌，通常與AEM培訓搭配使用。

WKND網站包含公開[GraphQL端點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html)所需的設定。 在真實世界的實作中，請依照檔案說明的步驟，將GraphQL端點[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments.html)納入您的客戶專案。 [CORS](#cors-config)也已封裝為WKND網站的一部分。 需要CORS設定才能授與外部應用程式的存取權，有關[CORS](#cors-config)的詳細資訊可在下方找到。

1. 下載適用於WKND網站的最新編譯的AEM套件： [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 請確定下載與AEM as a Cloud Service相容的標準版本，**不** `classic`版本。

1. 從&#x200B;**AEM開始**&#x200B;功能表，瀏覽至&#x200B;**工具** > **部署** > **套件**。

   ![瀏覽至封裝](assets/quick-setup/aem-sdk/aem-sdk__packages.png)

1. 按一下&#x200B;**上傳封裝**，然後選擇在先前步驟中下載的WKND封裝。 按一下&#x200B;**安裝**&#x200B;以安裝封裝。

1. 從&#x200B;**AEM Start**&#x200B;功能表，導覽至&#x200B;**Assets** > **檔案** > **WKND共用** > **英文** > **冒險**。

   ![冒險的資料夾檢視](assets/quick-setup/aem-sdk/aem-sdk__assets-folder.png)

   這是構成WKND品牌宣傳的各種冒險的所有資產的資料夾。 這包括影像和視訊等傳統媒體型別，以及&#x200B;**內容片段**&#x200B;等AEM特有的媒體。

1. 按一下&#x200B;**Downhill Skiing Wyoming**&#x200B;資料夾，然後按一下&#x200B;**Downhill Skiing Wyoming內容片段**&#x200B;卡片：

   ![內容片段卡片](assets/quick-setup/aem-sdk/aem-sdk__content-fragment.png)

1. 內容片段編輯器隨即開啟，以進行「懷俄明州下山滑雪」冒險活動。

   ![內容片段編輯器](assets/quick-setup/aem-sdk/aem-sdk__content-fragment-editor.png)

   觀察各種欄位，例如&#x200B;**Title**、**Description**&#x200B;和&#x200B;**Activity**&#x200B;定義片段。

   **內容片段**&#x200B;是在AEM中管理內容的方式之一。 內容片段是可重複使用的、與呈現方式無關的內容，由結構化資料元素組成，例如文字、RTF文字、日期或其他內容片段的參考。 內容片段稍後會在快速設定中更詳細地探討。

1. 按一下&#x200B;**取消**&#x200B;以關閉片段。 您可以隨意瀏覽至其他資料夾，並探索其他冒險內容。

>[!NOTE]
>
> 如果使用Cloud Service環境，請參閱檔案，瞭解如何[將程式碼基底（例如WKND參考網站）部署到Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version)。

## 3.下載並執行WKND React應用程式 {#sample-app}

本教學課程的目標之一，是說明如何使用AEM API從外部應用程式使用GraphQL內容。 本教學課程使用範例React應用程式。 React應用程式經過刻意簡化，可專注與AEM的GraphQL API整合。

1. 開啟新的命令提示字元，並從GitHub複製範例React應用程式：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 在您選擇的IDE中開啟`aem-guides-wknd-graphql/react-app`中的React應用程式。
1. 在IDE中，在`/.env.development`開啟檔案`.env.development`。 確認`REACT_APP_AUTHORIZATION`行已取消註解，且檔案宣告下列變數：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   確保`REACT_APP_HOST_URI`指向您本機的AEM SDK。 為方便起見，此快速入門會將React應用程式連線至&#x200B;**AEM作者**。 **作者**&#x200B;服務需要驗證，所以應用程式使用`admin`使用者建立連線。 將應用程式連線至AEM作者是開發期間的常見做法，因為這有助於在無需發佈變更的情況下快速迭代內容。

   >[!NOTE]
   >
   > 在生產案例中，應用程式將連線至AEM **發佈**&#x200B;環境。 _生產部署_&#x200B;區段將對此進行詳細介紹。


1. 安裝並啟動React應用程式：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器視窗會在[http://localhost:3000](http://localhost:3000)上自動開啟應用程式。

   ![React入門應用程式](assets/quick-setup/aem-sdk/react-app__home-view.png)

   隨即顯示AEM中的冒險內容清單。

1. 按一下其中一個冒險影像以檢視冒險詳細資料。 系統會向AEM提出要求，要求您傳回冒險的詳細資訊。

   ![冒險詳細資料檢視](assets/quick-setup/aem-sdk/react-app__adventure-view.png)

1. 使用瀏覽器的開發人員工具來檢查&#x200B;**網路**&#x200B;要求。 檢視&#x200B;**XHR**&#x200B;要求，並觀察`/graphql/execute.json/...`的多個GET要求。 此路徑首碼會叫用AEM的持久查詢端點，並在首碼之後使用名稱和編碼引數選取要執行的持久查詢。

   ![GraphQL端點XHR要求](assets/quick-setup/aem-sdk/react-app__graphql-request.png)

## 4.在AEM中編輯內容

在React應用程式執行中，更新AEM中的內容，並檢視應用程式中是否反映變更。

1. 導覽至AEM [http://localhost:4502](http://localhost:4502)。
1. 導覽至&#x200B;**Assets** > **檔案** > **共用的WKND** > **英文** > **冒險** > **[巴厘島衝浪營](http://localhost:4502/assets.html/content/dam/wknd-shared/en/adventures/bali-surf-camp)**。

   ![巴厘島衝浪營資料夾](assets/setup/bali-surf-camp-folder.png)

1. 按一下&#x200B;**Bali Surf Camp**&#x200B;內容片段以開啟內容片段編輯器。
1. 修改冒險的&#x200B;**標題**&#x200B;和&#x200B;**描述**。

   ![修改內容片段](assets/setup/modify-content-fragment-bali.png)

1. 按一下「**儲存**」以儲存變更。
1. 在[http://localhost:3000](http://localhost:3000)重新整理React應用程式以檢視您的變更：

   ![已更新巴厘島衝浪營地冒險活動](assets/setup/overnight-bali-surf-camp-changes.png)

## 5.探索GraphiQL {#graphiql}

1. 導覽至&#x200B;**工具** > **一般** > **GraphQL查詢編輯器**，以開啟[GraphiQL](http://localhost:4502/aem/graphiql.html)
1. 選取左側的現有持續查詢，並執行它們以檢視結果。

   >[!NOTE]
   >
   > GraphiQL工具和GraphQL API將在教學課程的後期[&#128279;](../multi-step/explore-graphql-api.md)中詳細探討。

## 恭喜！{#congratulations}

恭喜，您現在有外部應用程式透過GraphQL使用AEM內容。 歡迎在React應用程式中檢查程式碼，並繼續嘗試修改現有的內容片段。

### 後續步驟

* [開始AEM Headless教學課程](../multi-step/overview.md)
