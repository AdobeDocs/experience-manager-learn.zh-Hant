---
title: 使用AEM Publish服務進行生產部署——開始使用AEM Headless - GraphQL
description: 瞭解AEM Author和Publish服務，以及建議的無頭應用程式部署模式。 在本教程中，學習如何使用環境變數根據目標環境動態更改GraphQL端點。 瞭解如何正確設定AEM以進行跨原始資源共用(CORS)。
sub-product: 資產
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
translation-type: tm+mt
source-git-commit: ce4a35f763862c6d6a42795fd5e79d9c59ff645a
workflow-type: tm+mt
source-wordcount: '2361'
ht-degree: 1%

---


# 使用AEM Publish服務進行生產部署

在本教學課程中，您將設定本機環境，以模擬內容從「作者」例項散發至「發佈」例項。 您也會使用GraphQL API，產生React App的生產組建版本，此應用程式設定為使用AEM Publish環境中的內容。 在此過程中，您將學習如何有效使用環境變數，以及如何更新AEM CORS組態。

## 必備條件

本教學課程是多部分教學課程的一部分。 假定已完成前些部件中概述的步驟。

## 目標

瞭解如何：

* 瞭解AEM Author和Publish架構。
* 瞭解管理環境變數的最佳做法。
* 瞭解如何正確設定AEM以進行跨來源資源共用(CORS)。

## 作者發佈部署模式{#deployment-pattern}

完整的AEM環境由「作者」、「發佈」和「Dispatcher」組成。 「作者」服務是內部使用者建立、管理和預覽內容的地方。 「發佈」服務被視為「即時」環境，一般是使用者與之互動的環境。 內容在「作者」服務上經過編輯和核准後，會分發至「發佈」服務。

AEM無頭應用程式最常見的部署模式是讓生產版應用程式連接至AEM Publish服務。

![高階部署模式](assets/publish-deployment/high-level-deployment.png)

上圖描述了此常見的部署模式。

1. **內容作者**&#x200B;使用AEM作者服務來建立、編輯和管理內容。
2. **內容作者**&#x200B;和其他內部使用者可直接在作者服務上預覽內容。 您可以設定應用程式的預覽版本，以連線至「作者」服務。
3. 內容獲得核准後，就可以是&#x200B;**published**&#x200B;的AEM Publish服務。
4. **使用** 者可與應用程式的生產版互動。生產應用程式會連線至發佈服務，並使用GraphQL API來請求和使用內容。

本教學課程會將AEM Publish例項新增至目前的設定，以模擬上述部署。 在前幾章中，React App會直接連線至「作者」例項，做為預覽。 React App的生產版本將部署至靜態Node.js伺服器，以連線至新的Publish執行個體。

最後，將運行三台本地伺服器：

* http://localhost:4502 - Author實例
* http://localhost:4503 —— 發佈例項
* http://localhost:5000 —— 在生產模式下回應應用程式，連線至「發佈」例項。

## 安裝AEM SDK —— 發佈模式{#aem-sdk-publish}

目前，我們在&#x200B;**Author**&#x200B;模式下有執行中的SDK例項。 SDK也可以在&#x200B;**Publish**&#x200B;模式中啟動，以模擬AEM Publish環境。

有關設定本地開發環境[的更詳細指南，請參閱](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)。

1. 在您的本機檔案系統上，建立專用資料夾以安裝Publish執行個體，亦即`~/aem-sdk/publish`。
1. 複製前幾章中用於Author實例的Quickstart jar檔案，並將其貼上到`publish`目錄中。 或者，導覽至[軟體散發入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)並下載最新的SDK並擷取Quickstart jar檔案。
1. 將jar檔案更名為`aem-publish-p4503.jar`。

   `publish`字串指定快速啟動jar在「發佈」模式下啟動。 `p4503`指定Quickstart伺服器在埠4503上運行。

1. 開啟新的終端窗口並導航到包含jar檔案的資料夾。 安裝並啟動AEM例項：

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 提供管理員密碼作為`admin`。 任何管理員密碼都可接受，但建議使用本機開發的預設密碼，以避免額外的設定。
1. 當AEM例項完成安裝後，新的瀏覽器視窗將會在[http://localhost:4503/content.html](http://localhost:4503/content.html)開啟

   預期會傳回404找不到頁面。 這是全新的AEM例項，尚未安裝任何內容。

## 安裝示例內容和GraphQL端點{#wknd-site-content-endpoints}

就像在「作者」例項中，「發佈」例項需要啟用GraphQL端點，並需要範例內容。 接著，在Publish實例上安裝WKND參考網站。

1. 下載最新編譯的WKND網站AEM套件：[aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 請確定下載與AEM相容的標準版為雲端服務，**不是**`classic`版本。

1. 直接導覽至：[http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html)，使用者名稱`admin`和密碼`admin`。
1. 接著，導航至[http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)的「包管理器」。
1. 按一下&#x200B;**Upload Package** ，然後選擇在上一步中下載的WKND包。 按一下&#x200B;**Install**&#x200B;安裝軟體包。
1. 安裝軟體包後，WKND參考站點現在可在[http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html)中找到。
1. 按一下功能表列中的「登出」按鈕，以`admin`使用者身分登出。

   ![WKND登出參考網站](assets/publish-deployment/sign-out-wknd-reference-site.png)

   與AEM Author例項不同，AEM Publish例項預設為匿名唯讀存取。 我們想要模擬匿名使用者在執行React應用程式時的體驗。

## 更新環境變數以指向Publish實例{#react-app-publish}

接著，更新React應用程式使用的環境變數，以指向Publish執行個體。 React App應&#x200B;**only**&#x200B;以生產模式連線至Publish實例。

接著，新增檔案`.env.production.local`以模擬生產體驗。

1. 在IDE中開啟WKND GraphQL React應用程式。

1. 在`aem-guides-wknd-graphql/react-app`下方，新增名為`.env.production.local`的檔案。
1. 將`.env.production.local`填入以下內容：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![添加新的環境變數檔案](assets/publish-deployment/env-production-local-file.png)

   使用環境變數可輕鬆在作者或發佈環境之間切換GraphQL端點，毋需在應用程式碼中新增額外的邏輯。 有關[ React的自定義環境變數的詳細資訊，請參閱此處](https://create-react-app.dev/docs/adding-custom-environment-variables)。

   >[!NOTE]
   >
   > 請注意，由於「發佈」環境預設會提供對內容的匿名存取，因此不會包含任何驗證資訊。

## 部署靜態節點伺服器{#static-server}

React應用程式可使用webpack伺服器來啟動，但僅供開發使用。 接著，使用[serve](https://github.com/vercel/serve)模擬生產部署，以代管使用Node.js的React應用程式的生產建置。

1. 開啟新的終端窗口並導航到`aem-guides-wknd-graphql/react-app`目錄

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 使用以下命令安裝[serve](https://github.com/vercel/serve):

   ```shell
   $ npm install serve --save-dev
   ```

1. 在`react-app/package.json`開啟檔案`package.json`。 添加名為`serve`的指令碼：

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   `serve`指令碼執行兩個操作。 首先，產生React App的製作組建。 其次，Node.js伺服器會啟動並使用生產建置。

1. 返回終端並輸入啟動靜態伺服器的命令：

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. 開啟新瀏覽器並導覽至[http://localhost:5000/](http://localhost:5000/)。 您應該會看到React App正在提供。

   ![React App Served](assets/publish-deployment/react-app-served-port5000.png)

   請注意，GraphQL查詢正在首頁上工作。 使用您的開發人員工具檢查&#x200B;**XHR**&#x200B;要求。 請注意，GraphQL POST是位於`http://localhost:4503/content/graphql/global/endpoint.json`的Publish實例。

   不過，首頁上的所有影像都會損壞！

1. 按一下其中一個「Adventure Detail」（冒險細節）頁面。

   ![Adventure Detail Error](assets/publish-deployment/adventure-detail-error.png)

   請注意，`adventureContributor`出現GraphQL錯誤。 在下個練習中，已修正損壞的影像和`adventureContributor`問題。

## 絕對影像參照{#absolute-image-references}

由於`<img src`屬性被設定為相對路徑，最終指向`http://localhost:5000/`處的Node靜態伺服器，因此映像會出現中斷。 這些影像應該會指向AEM Publish實例。 這有幾種可能的解決辦法。 使用webpack dev伺服器時，檔案`react-app/src/setupProxy.js`會在webpack伺服器和AEM作者例項之間設定Proxy，以取得對`/content`的任何要求。 代理配置可用於生產環境，但必須在Web伺服器級別配置。 例如，[Apache的代理模組](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)。

應用程式可以更新為包含使用`REACT_APP_HOST_URI`環境變數的絕對URL。 讓我們改用AEM的GraphQL API功能來請求影像的絕對URL。

1. 停止Node.js伺服器。
1. 返回IDE並開啟`Adventures.js`檔案，位於`react-app/src/components/Adventures.js`。
1. 將`_publishUrl`屬性新增至`allAdventuresQuery`內的`ImageRef`:

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` 以 `_authorUrl` 及內建至物件的 `ImageRef` 值，讓包含絕對URL變得更輕鬆。

1. 重複上述步驟，修改`filterQuery(activity)`函式中使用的查詢，以包含`_publishUrl`屬性。
1. 修改`function AdventureItem(props)`處的`AdventureItem`元件，以在構建`<img src=''>`標籤時參照`_publishUrl`，而非`_path`屬性：

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. 在`react-app/src/components/AdventureDetail.js`開啟檔案`AdventureDetail.js`。
1. 重複相同步驟以修改GraphQL查詢，並為Adventure添加`_publishUrl`屬性

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. 修改`AdventureDetail.js`中「Adventure Primary Image」（冒險主映像）和「Contributor Picture」（參與者圖片）參考的兩個`<img>`標籤：

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. 返回終端並啟動靜態伺服器：

   ```shell
   $ npm run serve
   ```

1. 導覽至[http://localhost:5000/](http://localhost:5000/)並觀察影像是否出現，以及`<img src''>`屬性指向`http://localhost:4503`。

   ![影像損壞已修正](assets/publish-deployment/broken-images-fixed.png)

## 模擬內容發佈{#content-publish}

請記得，當請求「冒險詳細資訊」頁面時，`adventureContributor`會拋出GraphQL錯誤。 **Contributor**&#x200B;內容片段模型尚不存在於Publish實例中。 對&#x200B;**Adventure**&#x200B;內容片段模型所做的更新也不適用於Publish實例。 這些變更是直接對「作者」例項所做，而且必須散布至「發佈」例項。

在推出依賴內容片段或內容片段模型更新之應用程式的新更新時，請考量這一點。

接下來，可模擬本機「作者」和「發佈」例項之間的內容發佈。

1. 啟動Author實例（如果尚未啟動），並導航至[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)的Package Manager
1. 下載軟體包[EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip)，然後使用軟體包管理器進行安裝。

   此套件會安裝組態，讓Author例項將內容發佈至Publish例項。 有關[此配置的手動步驟，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution)。

   >[!NOTE]
   >
   > 在AEM做為雲端服務環境中，作者層會自動設定，以將內容散發至發佈層。

1. 從&#x200B;**AEM Start**&#x200B;功能表，導覽至&#x200B;**Tools** > **Assets** > **Content Fragment Models**。

1. 按一下&#x200B;**WKND Site**&#x200B;資料夾。

1. 選擇所有三種型號，然後按一下&#x200B;**Publish**:

   ![發佈內容片段模型](assets/publish-deployment/publish-contentfragment-models.png)

   出現確認對話框，按一下&#x200B;**Publish**。

1. 導覽至位於[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的巴釐島衝浪訓練營內容片段。

1. 按一下頂端功能表列中的&#x200B;**Publish**&#x200B;按鈕。

   ![在內容片段編輯器中按一下「發佈」按鈕](assets/publish-deployment/publish-bali-content-fragment.png)

1. 「發佈」精靈會顯示任何應發佈的相依資產。 在這種情況下，列出所引用的片段&#x200B;**stacey-roswells**，並且還引用多個影像。 參考的資產會與片段一起發佈。

   ![要發佈的參考資產](assets/publish-deployment/referenced-assets.png)

   再按一下「**發佈**」按鈕，以發佈內容片段和相依資產。

1. 返回運行於[http://localhost:5000/](http://localhost:5000/)的React App。 您現在可以點選巴釐島衝浪夏令營，以檢視冒險的細節。

1. 切換回位於[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的「AEM作者」例項，並更新片段的&#x200B;**Title**。 **儲存並** 關閉片段。然後，**publish**&#x200B;片段。
1. 返回[http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)並觀察已發佈的更改。

   ![巴釐島衝浪訓練營發佈更新](assets/publish-deployment/bali-surf-camp-update.png)

## 更新COR設定

AEM預設為安全，不允許非AEM網頁屬性進行用戶端呼叫。 AEM的跨原始資源共用(CORS)設定可允許特定網域對AEM進行呼叫。

接下來，嘗試AEM Publish例項的CORS設定。

1. 返回到使用命令`npm run serve`運行React App的終端窗口：

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   請注意提供了兩個URL。 一個使用`localhost`，另一個使用本地網路IP地址。

1. 導覽至以[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)開始的位址。 每台本地電腦的地址都略有不同。 請注意，擷取資料時發生CORS錯誤。 這是因為目前的CORS組態僅允許來自`localhost`的要求。

   ![CORS錯誤](assets/publish-deployment/cors-error-not-fetched.png)

   接著，更新AEM Publish CORS設定，以允許來自網路IP位址的請求。

1. 導覽至[http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html)並使用使用者名稱`admin`和密碼`admin`登入。

1. 導覽至[http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr)並在`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`找到WKND GraphQL配置。

1. 更新&#x200B;**允許的來源**&#x200B;欄位以包含網路IP地址：

   ![更新CORS設定](assets/publish-deployment/cors-update.png)

   您也可以包含規則運算式，以允許來自特定子網域的所有請求。 儲存變更。

1. 搜尋&#x200B;**Apache Sling Referrer Filter**&#x200B;並檢視設定。 還需要&#x200B;**允許空**&#x200B;配置，才能啟用來自外部域的GraphQL請求。

   ![Sling 查閱者篩選](assets/publish-deployment/sling-referrer-filter.png)

   這些已配置為WKND參考站點的一部分。 您可以通過[ GitHub儲存庫](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)查看完整的OSGi配置集。

   >[!NOTE]
   >
   > OSGi組態是在AEM專案中管理，並提交至來源控制。 AEM專案可以使用Cloud Manager部署至AEM做為Cloud Service環境。 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)可協助產生特定實作的專案。

1. 從[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)開始返回React App，並觀察應用程式不會再引發CORS錯誤。

   ![CORS錯誤已修正](assets/publish-deployment/cors-error-corrected.png)

## 恭喜！{#congratulations}

恭喜！ 您現在已使用AEM Publish環境模擬完整的生產部署。 您也已學習如何在AEM中使用CORS設定。

## 其他資源

有關內容片段和GraphQL的詳細資訊，請參閱下列資源：

* [使用內容片段與GraphQL進行無頭內容傳送](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API，用於內容片段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [代號型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [將程式碼部署至AEM做為雲端服務](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
