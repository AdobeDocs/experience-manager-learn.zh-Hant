---
title: 快速設定 — AEM無周邊功能快速入門 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 安裝AEM SDK、新增範例內容，以及部署使用其GraphQL API從AEM取用內容的應用程式。 了解AEM如何提供全管道體驗。
version: cloud-service
mini-toc-levels: 1
kt: 6386
thumbnail: KT-6386.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1814'
ht-degree: 1%

---


# 快速設定 {#setup}

本章提供本機環境的快速設定，以便查看外部應用程式使用AEM GraphQL API從AEM使用內容。 教學課程的後續章節將以此設定為基礎。

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

* [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14)
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

## 目標 {#objectives}

1. 下載並安裝AEM SDK。
1. 從WKND參考網站下載並安裝範例內容。
1. 下載並安裝範例應用程式，以使用GraphQL API來使用內容。

## 安裝AEM SDK {#aem-sdk}

本教學課程使用[AEM作為Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#aem-as-a-cloud-service-sdk)來探索AEM GraphQL API。 本節提供安裝AEM SDK並以製作模式執行的快速指南。 若需設定本機開發環境[的更詳細指南，請前往](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)。

>[!NOTE]
>
> 您也可以透過AEM as a Cloud Service環境，遵循教學課程。 使用雲端環境的其他附註會納入整個教學課程中。

1. 導覽至&#x200B;**[Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as aCloud Service**，並下載最新版&#x200B;**AEM SDK**。

   ![Software Distribution入口網站](assets/setup/software-distribution-portal-download.png)

   >[!CAUTION]
   >
   > GraphQL功能預設僅在2021-02-04（含）以上版本的AEM SDK上啟用。

1. 解壓縮下載並將Quickstart Jar(`aem-sdk-quickstart-XXX.jar`)複製到專用資料夾，即`~/aem-sdk/author`。
1. 將jar檔案重新命名為`aem-author-p4502.jar`。

   `author`名稱指定Quickstart Jar將以「製作」模式啟動。 `p4502`指定Quickstart伺服器將在埠4502上運行。

1. 開啟新的終端機視窗，並導覽至包含jar檔案的資料夾。 執行下列命令以安裝並啟動AEM執行個體：

   ```shell
   $ cd ~/aem-sdk/author
   $ java -jar aem-author-p4502.jar
   ```

1. 提供管理員密碼作為`admin`。 任何管理員密碼都是可接受的，但建議使用本機開發的預設密碼，以減少重新設定的需求。
1. 幾分鐘後，AEM執行個體將完成安裝，而新的瀏覽器視窗應會在[http://localhost:4502](http://localhost:4502)開啟。
1. 使用用戶名`admin`和密碼`admin`登錄。

## 安裝範例內容和GraphQL端點 {#wknd-site-content-endpoints}

將安裝&#x200B;**WKND參考網站**&#x200B;中的範例內容，以加速教學課程。 WKND是虛構的生活風格品牌，通常與AEM訓練搭配使用。

WKND參考站點包括公開[GraphQL終結點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint)所需的配置。 在實際實作中，請依照記錄的步驟，在您的客戶專案中[包含GraphQL端點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=en#graphql-aem-endpoint)。 [CORS](#cors-config)也作為WKND站點的一部分打包。 需要CORS配置才能授予對外部應用程式的訪問權，有關[CORS](#cors-config)的更多資訊，請參閱下文。

1. 下載WKND站點的最新編譯AEM包：[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 請務必下載與AEM as aCloud Service相容的標準版本，而&#x200B;**不**`classic`版本。

1. 從&#x200B;**AEM Start**&#x200B;菜單導航至&#x200B;**Tools** > **Deployment** > **Packages**。

   ![導覽至套件](assets/setup/navigate-to-packages.png)

1. 按一下「**上傳套件**」 ，然後選擇上一步驟中下載的WKND套件。 按一下&#x200B;**Install**&#x200B;以安裝軟體包。

1. 從&#x200B;**AEM Start**&#x200B;功能表導覽至&#x200B;**Assets** > **Files**。
1. 按一下資料夾以導覽至&#x200B;**WKND Site** > **English** > **Adventures**。

   ![歷險資料夾檢視](assets/setup/folder-view-adventures.png)

   此資料夾包含WKND品牌所推廣之各種歷險項目的所有資產。 這包括傳統媒體類型，例如影像和視訊，以及AEM專用的媒體，例如&#x200B;**內容片段**。

1. 按一下&#x200B;**Swooksing Wyoming**&#x200B;資料夾，然後按一下&#x200B;**Swooksing Wyoming內容片段**&#x200B;卡：

   ![下游滑雪內容片段卡](assets/setup/downhill-skiing-cntent-fragment.png)

1. 內容片段編輯器UI將開啟，供Sowking Wyoming滑坡探險之用。

   ![下山滑雪內容片段](assets/setup/down-hillskiing-fragment.png)

   觀察&#x200B;**Title**、**Description**&#x200B;和&#x200B;**Activity**&#x200B;等各種欄位定義片段。

   **內** 容片段是在AEM中管理內容的其中一種方式。內容片段是可重複使用且不受展示的內容，由結構化資料元素（例如文字、RTF、日期或其他內容片段的參考）組成。 稍後的教學課程將會詳細說明內容片段。

1. 按一下&#x200B;**取消**&#x200B;以關閉片段。 您可以導覽至其他資料夾，並探索其他Adventure內容。

>[!NOTE]
>
> 如果使用Cloud Service環境，請參閱如何[將程式碼基底（如WKND參考網站）部署至Cloud Service環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=en#deploying)的檔案。

## 安裝範例應用程式{#sample-app}

本教學課程的其中一個目標是說明如何使用GraphQL API從外部應用程式使用AEM內容。 本教學課程使用已部分完成的React應用程式範例來加速本教學課程。 以iOS、Android或任何其他平台建立的應用程式也適用相同的課程和概念。 React應用程式刻意簡單，以避免不必要的複雜性；這不是參考實作。

1. 開啟新的終端機視窗，並使用Git複製教學課程啟動器分支：

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在您選擇的IDE中，在`aem-guides-wknd-graphql/react-app/.env.development`開啟檔案`.env.development`。 確認`REACT_APP_AUTHORIZATION`行未加上註解，且檔案看起來如下：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   # Use Authorization when connecting to an AEM Author environment
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   確定`React_APP_HOST_URI`符合您的本機AEM例項。 在本章中，我們將直接將React應用程式連結至AEM **Author**&#x200B;環境。 **** 依預設，授權需要驗證，因此我們的應用程式會以使用者身分 `admin` 連線。這是開發期間的常見作法，因為它可讓我們快速變更AEM環境，並立即在應用程式中反映。

   >[!NOTE]
   >
   > 在生產案例中，應用程式會連線至AEM **Publish**&#x200B;環境。 這在[生產部署](production-deployment.md)章節中有更詳細的說明。

1. 導覽至`aem-guides-wknd-graphql/react-app`資料夾。 安裝並啟動應用程式：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器視窗應會自動啟動應用程式，網址為[http://localhost:3000](http://localhost:3000)。

   ![React入門應用程式](assets/setup/react-starter-app.png)

   應會顯示來自AEM的目前「探險」內容清單。

1. 按一下其中一個冒險影像，查看冒險詳細資訊。 系統會向AEM要求，傳回冒險的詳細資訊。

   ![冒險詳細資訊視圖](assets/setup/adventure-details-view.png)

1. 使用瀏覽器的開發人員工具來檢查&#x200B;**網路**&#x200B;請求。 檢視&#x200B;**XHR**&#x200B;請求，並觀察對`/content/graphql/global/endpoint.json`(為AEM設定的GraphQL端點)發出的多個POST請求。

   ![GraphQL端點XHR請求](assets/setup/endpoint-gql.png)

1. 您也可以檢查網路要求，以檢視參數和JSON回應。 為Chrome安裝[GraphQL網路偵測器](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln)等瀏覽器擴充功能，有助於您更清楚了解查詢和回應。

## 修改內容片段

現在React應用程式正在執行，請更新AEM中的內容，並查看應用程式中反映的變更。

1. 導覽至AEM [http://localhost:4502](http://localhost:4502)。
1. 導覽至&#x200B;**Assets** > **Files** > **WKND Site** > **English** > **Adventures** > **[Bali Surf Camp](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)**。

   ![巴釐島衝浪營資料夾](assets/setup/bali-surf-camp-folder.png)

1. 按一下進入&#x200B;**Bali Surf Camp**&#x200B;內容片段，以開啟內容片段編輯器。
1. 修改探險的&#x200B;**Title**&#x200B;和&#x200B;**Description**

   ![修改內容片段](assets/setup/modify-content-fragment-bali.png)

1. 按一下&#x200B;**儲存**&#x200B;以儲存變更。
1. 導覽回[http://localhost:3000](http://localhost:3000)的React應用程式，然後重新整理，查看您的變更：

   ![更新巴釐島衝浪夏令營探險](assets/setup/overnight-bali-surf-camp-changes.png)

## 安裝GraphiQL工具 {#install-graphiql}

[](https://github.com/graphql/graphiql) GraphiQL是開發工具，僅在開發或本機執行個體等較低層級環境中需要。GraphiQL IDE允許您快速測試和調整返回的查詢和資料。 GraphiQL還可輕鬆存取說明檔案，讓您輕鬆了解和了解可用的方法。

1. 導覽至&#x200B;**[Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as aCloud Service**。
1. 搜索「GraphiQL」(請務必在&#x200B;**GraphiQL**&#x200B;中包含&#x200B;**i**。
1. 下載最新&#x200B;**GraphiQL內容包v.x.x.x**

   ![下載GraphiQL包](assets/explore-graphql-api/software-distribution.png)

   zip檔案是可直接安裝的AEM套件。

1. 從&#x200B;**AEM Start**&#x200B;菜單導航至&#x200B;**Tools** > **Deployment** > **Packages**。
1. 按一下「**上傳套件**」 ，然後選擇上一步驟中下載的套件。 按一下&#x200B;**Install**&#x200B;以安裝軟體包。

   ![安裝GraphiQL包](assets/explore-graphql-api/install-graphiql-package.png)
1. 導覽至位於[http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)的GraphiQL IDE，然後開始探索GraphQL API。

   >[!NOTE]
   >
   > 在教學課程](./explore-graphql-api.md)中稍後將更詳細地介紹GraphiQL工具和GraphQL API。[

## 恭喜！ {#congratulations}

恭喜，您現在有外部應用程式使用GraphQL的AEM內容。 歡迎在React應用程式中檢查程式碼，並繼續嘗試修改現有內容片段。

## 後續步驟 {#next-steps}

在下一章[定義內容片段模型](content-fragment-models.md)中，了解如何使用&#x200B;**內容片段模型**&#x200B;來建立內容模型和架構。 將審閱現有模型並建立新模型。 您也將了解可用來定義模型中結構的不同資料類型。

## （額外）CORS設定 {#cors-config}

AEM預設為安全，會封鎖跨原始請求，防止未經授權的應用程式連線及呈現其內容。

為了允許本教學課程的React應用程式與AEM GraphQL API端點互動，WKND網站參考專案中已定義跨原始資源共用設定。

![跨原始資源共用設定](assets/setup/cross-origin-resource-sharing-configuration.png)

要查看已部署的配置，請執行以下操作：

1. 導覽至AEM SDK的Web主控台，網址為[http://localhost:4502/system/console](http://localhost:4502/system/console)。

   >[!NOTE]
   >
   > Web主控台僅可在SDK上使用。 在AEM as aCloud Service環境中，此資訊可透過[開發人員控制台](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console.html)檢視。

1. 在頂端功能表中，按一下「**OSGI** > **Configuration**」以開啟所有[OSGi Configurations](http://localhost:4502/system/console/configMgr)。
1. 向下捲動頁面&#x200B;**AdobeGranite跨原始資源共用**。
1. 按一下`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`的配置。
1. 已更新下列欄位：
   * 允許的原始項(Regex):`http://localhost:.*`
      * 允許所有本地主機連接。
   * 允許的路徑: `/content/graphql/global/endpoint.json`
      * 這是當前配置的唯一GraphQL終結點。 作為最佳做法，變革和組織振興方案的配置應盡可能限制。
   * 允許的方法：`GET`, `HEAD`, `POST`
      * GraphQL只需要`POST` ，但其他方法在以無周邊方式與AEM互動時非常有用。
   * 支援的標題：已新增&#x200B;**authorization**&#x200B;以在製作環境上傳遞基本驗證。
   * 支援憑據：`Yes`
      * 這是必要操作，因為我們的React應用程式會與AEM製作服務上受保護的GraphQL端點通訊。

此配置和GraphQL端點是AEM WKND項目的一部分。 您可以在此處](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)查看所有[OSGi配置。
