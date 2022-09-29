---
title: 使用AEM發佈服務進行生產部署 — 開始使用AEM無周邊功能 — GraphQL
description: 了解AEM製作和發佈服務，以及無頭式應用程式的建議部署模式。 在本教學課程中，了解如何使用環境變數根據目標環境動態變更GraphQL端點。 了解如何正確設定AEM以進行跨原始資源共用(CORS)。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2357'
ht-degree: 1%

---

# 使用AEM Publish服務進行生產部署

在本教學課程中，您將設定本機環境，以模擬從製作例項發佈到發佈例項的內容。 您也會使用GraphQL API從AEM發佈環境中產生React應用程式的生產組建，並設定為使用內容。 在過程中，您將學習如何有效使用環境變數，以及如何更新AEM CORS設定。

## 必備條件

本教學課程是多部分教學課程的一部分。 假定已完成前幾部分中概述的步驟。

## 目標

了解如何：

* 了解AEM製作和發佈架構。
* 了解管理環境變數的最佳實務。
* 了解如何正確設定AEM以進行跨原始資源共用(CORS)。

## 製作發佈部署模式 {#deployment-pattern}

完整的AEM環境由製作、發佈和Dispatcher組成。 內部使用者可在「作者」服務建立、管理和預覽內容。 發佈服務會視為「即時」環境，且通常是使用者與之互動的環境。 內容在Author服務上經過編輯和核准後，會分發至Publish服務。

AEM無頭式應用程式最常見的部署模式是讓生產版本的應用程式連線至AEM發佈服務。

![高級部署模式](assets/publish-deployment/high-level-deployment.png)

上圖描述了此通用部署模式。

1. A **內容作者** 使用AEM製作服務來建立、編輯及管理內容。
2. 此 **內容作者** 而其他內部使用者可以直接在Author服務上預覽內容。 可設定應用程式的預覽版本，以連線至製作服務。
3. 內容一經核准，即可 **已發佈** AEM發佈服務。
4. **一般使用者** 與應用程式的生產版本互動。 生產應用程式會連線至發佈服務，並使用GraphQL API來請求和使用內容。

本教學課程會將AEM Publish例項新增至目前的設定，以模擬上述部署。 在舊版章節中， React應用程式會直接連線至Author例項，以作為預覽。 React應用程式的生產組建會部署至靜態Node.js伺服器，而此伺服器會連線至新的Publish執行個體。

最後，運行了三台本地伺服器：

* http://localhost:4502 — 製作例項
* http://localhost:4503 — 發佈例項
* http://localhost:5000 — 在生產模式中連線至發佈執行個體的React App 。

## 安裝AEM SDK — 發佈模式 {#aem-sdk-publish}

目前，我們有執行中的SDK例項，位於 **作者** 模式。 SDK也可在 **發佈** 模式來模擬AEM發佈環境。

設定本機開發環境的更詳細指南 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. 在您的本機檔案系統上，建立專用資料夾以安裝發佈執行個體，即 `~/aem-sdk/publish`.
1. 複製前幾章中用於製作例項的Quickstart jar檔案，並貼到 `publish` 目錄。 或者，導覽至 [Software Distribution入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 並下載最新的SDK並解壓縮Quickstart jar檔案。
1. 將jar檔案重新命名為 `aem-publish-p4503.jar`.

   此 `publish` 字串指定Quickstart jar在「發佈」模式下啟動。 此 `p4503` 指定Quickstart伺服器在埠4503上運行。

1. 開啟新的終端機視窗，並導覽至包含jar檔案的資料夾。 安裝並啟動AEM執行個體：

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 提供管理員密碼作為 `admin`. 任何管理員密碼都是可接受的，但建議將預設密碼用於本機開發，以避免額外的設定。
1. 當AEM執行個體完成安裝時，會在中開啟新的瀏覽器視窗 [http://localhost:4503/content.html](http://localhost:4503/content.html)

   預計會傳回404找不到頁面。 這是全新的AEM例項，尚未安裝任何內容。

## 安裝範例內容和GraphQL端點 {#wknd-site-content-endpoints}

就像在製作執行個體上一樣，發佈執行個體需要啟用GraphQL端點，且需要範例內容。 接下來，在發佈執行個體上安裝WKND參考網站。

1. 下載WKND站點的最新編譯AEM包： [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > 請務必下載與AEMas a Cloud Service相容的標準版本，以及 **not** the `classic` 版本。

1. 直接導覽至： [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) 使用者名稱 `admin` 和密碼 `admin`.
1. 接下來，導覽至封裝管理器，網址為 [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. 按一下 **上傳套件** 並選擇在前一步中下載的WKND包。 按一下 **安裝** 安裝套件。
1. 安裝套件後，WKND參考網站現在可於 [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. 登出為 `admin` 使用者，方法是按一下功能表列的「登出」按鈕。

   ![WKND登出參考網站](assets/publish-deployment/sign-out-wknd-reference-site.png)

   與AEM Author例項不同，AEM Publish例項預設為匿名唯讀存取。 我們希望模擬匿名用戶在運行React應用程式時的體驗。

## 更新環境變數以指向發佈例項 {#react-app-publish}

接下來，更新React應用程式使用的環境變數，以指向Publish例項。 React應用程式應 **僅限** 以生產模式連線至發佈執行個體。

接下來，添加新檔案 `.env.production.local` 來模擬生產體驗。

1. 在IDE中開啟WKND GraphQL React應用程式。

1. 下方 `aem-guides-wknd-graphql/react-app`，新增檔案名為 `.env.production.local`.
1. 填入 `.env.production.local` 並搭配下列項目：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![新增環境變數檔案](assets/publish-deployment/env-production-local-file.png)

   使用環境變數可讓您輕鬆在製作或發佈環境之間切換GraphQL端點，而無須在應用程式程式碼中新增額外的邏輯。 有關 [可在此處找到React的自訂環境變數](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > 請注意，由於「發佈」環境預設會提供匿名內容存取權，因此未包含驗證資訊。

## 部署靜態節點伺服器 {#static-server}

React應用程式可透過Webpack伺服器啟動，但僅供開發使用。 接下來，使用 [serve](https://github.com/vercel/serve) 使用Node.js托管React應用程式的生產組建。

1. 開啟新的終端機視窗，並導覽至 `aem-guides-wknd-graphql/react-app` 目錄

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 安裝 [serve](https://github.com/vercel/serve) 使用下列命令：

   ```shell
   $ npm install serve --save-dev
   ```

1. 開啟檔案 `package.json` at `react-app/package.json`. 新增指令碼，命名為 `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   此 `serve` 指令碼執行兩個動作。 首先，產生React應用程式的生產組建。 接著， Node.js伺服器會啟動並使用生產組建。

1. 返回到終端，然後輸入命令以啟動靜態伺服器：

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

1. 開啟新瀏覽器並導覽至 [http://localhost:5000/](http://localhost:5000/). 您應會看到React應用程式正在提供。

   ![已提供React應用程式](assets/publish-deployment/react-app-served-port5000.png)

   請注意，GraphQL查詢在首頁上運作。 Inspect **XHR** 使用您的開發人員工具提出要求。 請注意，GraphQLPOST是發佈執行個體() `http://localhost:4503/content/graphql/global/endpoint.json`.

   不過，首頁上的所有影像都損毀！

1. 按一下其中一個「冒險詳細資訊」頁面。

   ![冒險詳細資訊錯誤](assets/publish-deployment/adventure-detail-error.png)

   請注意， `adventureContributor`. 在下一個練習中，損壞的影像和 `adventureContributor` 問題已修正。

## 絕對影像參照 {#absolute-image-references}

影像似乎損毀，因為 `<img src` 屬性被設定為相對路徑，最終指向位於的Node靜態伺服器 `http://localhost:5000/`. 這些影像應該會指向AEM Publish例項。 有幾種可能的解決方案。 使用Webpack開發伺服器時，檔案 `react-app/src/setupProxy.js` 在webpack伺服器和AEM製作執行個體之間設定代理，以取得 `/content`. 代理配置可用於生產環境，但必須在Web伺服器級別進行配置。 例如， [Apache的代理模組](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

應用程式可更新為包含絕對URL，使用 `REACT_APP_HOST_URI` 環境變數。 請改為使用AEM GraphQL API的功能，為影像要求絕對URL。

1. 停止Node.js伺服器。
1. 返回到IDE並開啟檔案 `Adventures.js` at `react-app/src/components/Adventures.js`.
1. 新增 `_publishUrl` 屬性 `ImageRef` 在 `allAdventuresQuery`:

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

   `_publishUrl` 和 `_authorUrl` 是內建在 `ImageRef` 物件，讓包含絕對url更容易。

1. 重複上述步驟，修改 `filterQuery(activity)` 函式以包含 `_publishUrl` 屬性。
1. 修改 `AdventureItem` 元件於 `function AdventureItem(props)` 以參考 `_publishUrl` 而非 `_path` 建構屬性時的屬性 `<img src=''>` 標籤：

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. 開啟檔案 `AdventureDetail.js` at `react-app/src/components/AdventureDetail.js`.
1. 重複相同步驟以修改GraphQL查詢並新增 `_publishUrl` 《冒險家》

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

1. 修改兩個 `<img>` 中的冒險主影像和貢獻者圖片參考的標籤 `AdventureDetail.js`:

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

1. 導覽至 [http://localhost:5000/](http://localhost:5000/) 並觀察影像的出現 `<img src''>` 屬性點 `http://localhost:4503`.

   ![影像損壞已修正](assets/publish-deployment/broken-images-fixed.png)

## 模擬內容發佈 {#content-publish}

回想一下， `adventureContributor` 請求「冒險詳細資訊」頁時。 此 **貢獻者** 發佈例項上尚未存在內容片段模型。 更新 **冒險** 內容片段模型也無法在發佈執行個體上使用。 這些變更會直接對製作例項進行，且需分發至發佈例項。

對仰賴內容片段或內容片段模型更新的應用程式推出新更新時，請考量這點。

接下來，可模擬本機「製作」和「發佈」例項之間的內容發佈。

1. 啟動製作例項（如果尚未啟動），並導覽至封裝管理器()，網址為 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 下載套件 [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) 並使用軟體包管理器進行安裝。

   此套件會安裝組態，讓製作執行個體可將內容發佈至發佈執行個體。 手動步驟 [可在此處找到此設定](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > 在AEMas a Cloud Service環境中，製作層級會自動設定，以將內容分發至發佈層級。

1. 從 **AEM開始** ，導航 **工具** > **資產** > **內容片段模型**.

1. 按一下 **WKND站點** 檔案夾。

1. 選取全部三個模型，然後按一下 **發佈**:

   ![發佈內容片段模型](assets/publish-deployment/publish-contentfragment-models.png)

   出現確認對話方塊，按一下 **發佈**.

1. 導覽至下列位置的巴釐衝浪訓練營內容片段： [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. 按一下 **發佈** 按鈕。

   ![在內容片段編輯器中按一下「發佈」按鈕](assets/publish-deployment/publish-bali-content-fragment.png)

1. 「發佈」精靈會顯示應發佈的任何相依資產。 在此案例中，參考的片段 **斯塔西·羅斯韋爾斯** 會列出，並參考數個影像。 參照的資產會與片段一併發佈。

   ![要發佈的參考資產](assets/publish-deployment/referenced-assets.png)

   按一下 **發佈** 按鈕，以發佈內容片段和相依資產。

1. 返回React應用程式，在 [http://localhost:5000/](http://localhost:5000/). 您現在可以按一下巴釐島衝浪營，查看冒險活動的詳細資訊。

1. 切換回AEM Author例項，位於 [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) 並更新 **標題** 片段。 **儲存並關閉** 片段。 然後 **發佈** 片段。
1. 返回 [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) 並觀察所發佈的變更。

   ![巴釐島衝浪營發佈更新](assets/publish-deployment/bali-surf-camp-update.png)

## 更新COR配置

AEM預設為安全，不允許非AEM Web屬性進行用戶端呼叫。 AEM跨原始資源共用(CORS)設定可允許特定網域對AEM進行呼叫。

接下來，試用AEM Publish例項的CORS設定。

1. 返回使用命令運行React應用程式的終端窗口 `npm run serve`:

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

   請注意，已提供兩個URL。 使用 `localhost` 另一個使用本地網路IP地址。

1. 導覽至以開頭的地址 [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). 每個本地電腦的地址稍有不同。 請注意，擷取資料時發生CORS錯誤。 這是因為目前的CORS設定僅允許來自 `localhost`.

   ![CORS錯誤](assets/publish-deployment/cors-error-not-fetched.png)

   接下來，更新AEM發佈CORS設定，以允許來自網路IP位址的請求。

1. 導覽至 [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) 並使用用戶名登錄 `admin` 和密碼 `admin`.

1. 導覽至 [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) 並在 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. 更新 **允許的原始項** 欄位以包含網路IP位址：

   ![更新CORS設定](assets/publish-deployment/cors-update.png)

   您也可以包含規則運算式，以允許來自特定子網域的所有請求。 儲存變更。

1. 搜尋 **Apache Sling反向連結篩選器** 並查看配置。 此 **允許空** 還需要配置，才能從外部域啟用GraphQL請求。

   ![Sling 查閱者篩選器](assets/publish-deployment/sling-referrer-filter.png)

   這些檔案已配置為WKND參考站點的一部分。 您可以透過 [GitHub存放庫](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > OSGi設定在提交至原始碼控制的AEM專案中進行管理。 AEM專案可使用Cloud Manager部署至AEM作為Cloud Service環境。 此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 可協助產生特定實作的專案。

1. 從開始返回React應用程式 [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) 並觀察應用程式不再擲回CORS錯誤。

   ![已更正CORS錯誤](assets/publish-deployment/cors-error-corrected.png)

## 恭喜！ {#congratulations}

恭喜！ 您現在已使用AEM發佈環境模擬完整的生產部署。 您也學會了如何在AEM中使用CORS設定。

## 其他資源

如需內容片段和GraphQL的詳細資訊，請參閱下列資源：

* [使用內容片段搭配GraphQL的無周邊內容傳送](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM GraphQL API以搭配內容片段使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [權杖式驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [將程式碼部署至AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
