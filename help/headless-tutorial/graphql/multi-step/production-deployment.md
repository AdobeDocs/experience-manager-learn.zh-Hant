---
title: 使用AEM發佈服務進行生產部署 — 無頭AEM入門 — GraphQL
description: 瞭解AEM作者和發佈服務以及建議的無頭應用程式的部署模式。 在本教程中，學習如何使用環境變數根據目標環境動態更改GraphQL終結點。 學習正確配AEM置跨源資源共用(CORS)。
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
ht-degree: 7%

---

# 使用AEM發佈服務進行生產部署

在本教程中，您將設定一個本地環境，以模擬正在從「作者」實例分發到「發佈」實例的內容。 您還將生成React App的生產版本，該React App配置為使用GraphQLAPI從AEM發佈環境中使用內容。 在此過程中，您將學習如何有效地使用環境變數以及如何更新AEMCORS配置。

## 必備條件

本教程是多部分教程的一部分。 假定已完成前面部分中概述的步驟。

## 目標

瞭解如何：

* 瞭解AEM作者和發佈體系結構。
* 瞭解管理環境變數的最佳做法。
* 瞭解如何正確AEM配置跨源資源共用(CORS)。

## 作者發佈部署模式 {#deployment-pattern}

完整的 AEM 環境由作者、發佈和 Dispatcher 組成。作者服務是內部使用者建立、管理和預覽內容的地方。「發佈」服務被視為「即時」環境，通常是最終用戶與之交互的內容。 內容在作者服務上經編輯和核准後，將傳遞到發佈服務。

AEM 無周邊應用程式最常見的部署模式是讓應用程式的生產版本連接到 AEM Publish 服務。

![高級部署模式](assets/publish-deployment/high-level-deployment.png)

上圖描述了這種常見的部署模式。

1. A **內容作者** 使用作者AEM服務建立、編輯和管理內容。
2. **內容作者**&#x200B;和其他內部使用者可以直在作者服務預覽內容。可以設定連接到作者服務的應用程式預覽版本。
3. 一旦內容獲得批准， **出版** AEM發佈服務。
4. **一般使用者是與應用程式生產版本互動。**&#x200B;生產應用程式連接到發佈服務並使用GraphQLAPI請求和使用內容。

本教程通過將AEM發佈實例添加到當前設定來模擬上述部署。 在前幾章中，React App通過直接連接到Author實例而充當預覽。 將React App的生產版本部署到連接到新發佈實例的靜態Node.js伺服器。

最後，運行了三台本地伺服器：

* http://localhost:4502 — 作者實例
* http://localhost:4503 — 發佈實例
* http://localhost:5000 — 在生產模式下響應應用，連接到發佈實例。

## 安裝AEMSDK — 發佈模式 {#aem-sdk-publish}

當前，我們在 **作者** 的子菜單。 SDK也可以在 **發佈** 模式來模擬AEM發佈環境。

設定本地開發環境的更詳細指南 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)。

1. 在本地檔案系統上，建立一個專用資料夾以安裝發佈實例，即 `~/aem-sdk/publish`。
1. 複製前幾章中用於Author實例的Quickstart jar檔案，並將其貼上到 `publish` 的子菜單。 或者導航到 [軟體分發門戶](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 下載最新的SDK並解壓Quickstart jar檔案。
1. 將jar檔案更名為 `aem-publish-p4503.jar`。

   的 `publish` string指定快速啟動jar在「發佈」模式下啟動。 的 `p4503` 指定Quickstart伺服器在埠4503上運行。

1. 開啟新的終端窗口並導航到包含jar檔案的資料夾。 安裝並啟AEM動實例：

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 提供管理員密碼 `admin`。 任何管理員密碼都可接受，但建議使用本地開發的預設密碼以避免額外的配置。
1. 實例完AEM成安裝後，將在 [http://localhost:4503/content.html](http://localhost:4503/content.html)

   應返回「404未找到」頁。 這是一個全新實AEM例，尚未安裝任何內容。

## 安裝示例內容和GraphQL終端節點 {#wknd-site-content-endpoints}

與「作者」實例一樣，「發佈」實例需要啟用GraphQL端點，並需要示例內容。 接下來，在發佈實例上安裝WKND參考站點。

1. 下載WKND站AEM點的最新編譯包： [aem輔助線 — wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 確保下載與AEMas a Cloud Service相容的 **不** 這樣 `classic` 。

1. 通過直接導航至： [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) 用戶名 `admin` 和密碼 `admin`。
1. 接下來，導航至「包管理器」(Package Manager) [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)。
1. 按一下 **上載包** 選擇上一步下載的WKND包。 按一下 **安裝** 安裝軟體包。
1. 安裝軟體包後，WKND參考站點現在可在 [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html)。
1. 註銷為 `admin` 按鈕。

   ![WKND註銷參考站點](assets/publish-deployment/sign-out-wknd-reference-site.png)

   與AEM Author實例不同，AEM Publish實例預設為匿名只讀訪問。 我們希望模擬匿名用戶在運行React應用程式時的體驗。

## 更新環境變數以指向發佈實例 {#react-app-publish}

接下來，更新React應用程式使用的環境變數以指向Publish實例。 應用應 **僅** 在生產模式下連接到「發佈」實例。

接下來，添加新檔案 `.env.production.local` 來模擬生產體驗。

1. 在IDE中開啟WKNDGraphQLReact應用。

1. 在下面 `aem-guides-wknd-graphql/react-app`，添加名為 `.env.production.local`。
1. 填充 `.env.production.local` 下面列出：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![添加新的環境變數檔案](assets/publish-deployment/env-production-local-file.png)

   使用環境變數可以輕鬆地在「作者」或「發佈」環境之間切換GraphQL端點，而無需在應用程式碼中添加額外的邏輯。 有關 [此處可找到用於React的自定義環境變數](https://create-react-app.dev/docs/adding-custom-environment-variables)。

   >[!NOTE]
   >
   > 請注意，由於預設發佈環境提供對內容的匿名訪問，因此未包括任何身份驗證資訊。

## 部署靜態節點伺服器 {#static-server}

React應用程式可以使用webpack伺服器啟動，但僅用於開發。 接下來，使用 [服務](https://github.com/vercel/serve) 使用Node.js承載React應用的生產版本。

1. 開啟新的終端窗口並導航到 `aem-guides-wknd-graphql/react-app` 目錄

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 安裝 [服務](https://github.com/vercel/serve) 命令：

   ```shell
   $ npm install serve --save-dev
   ```

1. 開啟檔案 `package.json` 在 `react-app/package.json`。 添加名為 `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   的 `serve` 指令碼執行兩個操作。 首先，生成React App的生產版本。 其次，Node.js伺服器啟動並使用生產生成。

1. 返回到終端並輸入啟動靜態伺服器的命令：

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

1. 開啟新瀏覽器並導航到 [http://localhost:5000/](http://localhost:5000/)。 您應看到正在提供的React App。

   ![React App服務](assets/publish-deployment/react-app-served-port5000.png)

   請注意，GraphQL查詢正在首頁上工作。 Inspect **XHR** 請求。 請注意，GraphQLPOST將位於 `http://localhost:4503/content/graphql/global/endpoint.json`。

   但是，所有影像都在首頁上被損壞！

1. 按一下「Adventure Detail（冒險詳細資訊）」頁面之一。

   ![冒險詳細資訊錯誤](assets/publish-deployment/adventure-detail-error.png)

   觀察GraphQL錯誤 `adventureContributor`。 在接下來的練習中，斷裂的影像和 `adventureContributor` 問題已解決。

## 絕對影像引用 {#absolute-image-references}

影像顯示為損壞，因為 `<img src` 屬性設定為相對路徑，最後指向位於 `http://localhost:5000/`。 相反，這些影像應指向AEM發佈實例。 這有幾種可能的解決辦法。 使用webpack dev伺服器時 `react-app/src/setupProxy.js` 在webpack伺服器和作者實例之AEM間設定代理，以便 `/content`。 代理配置可以在生產環境中使用，但必須在Web伺服器級別進行配置。 比如說， [Apache的代理模組](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)。

可以更新應用，以使用 `REACT_APP_HOST_URI` 環境變數。 相反，讓我們使用GraphQLAPI的AEM功能來請求影像的絕對URL。

1. 停止Node.js伺服器。
1. 返回到IDE並開啟檔案 `Adventures.js` 在 `react-app/src/components/Adventures.js`。
1. 添加 `_publishUrl` 屬性 `ImageRef` 在 `allAdventuresQuery`:

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

   `_publishUrl` 和 `_authorUrl` 是內置於 `ImageRef` 使包含絕對url更容易。

1. 重複上述步驟以修改中使用的查詢 `filterQuery(activity)` 函式以包括 `_publishUrl` 屬性。
1. 修改 `AdventureItem` 元件 `function AdventureItem(props)` 引用 `_publishUrl` 而不是 `_path` 構建時的屬性 `<img src=''>` 標籤：

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. 開啟檔案 `AdventureDetail.js` 在 `react-app/src/components/AdventureDetail.js`。
1. 重複相同步驟以修改GraphQL查詢並添加 `_publishUrl` 探險公寓

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

1. 修改兩個 `<img>` 中的「冒險主映像」和「參與者圖片」引用的標籤 `AdventureDetail.js`:

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

1. 返回到終端並啟動靜態伺服器：

   ```shell
   $ npm run serve
   ```

1. 導航到 [http://localhost:5000/](http://localhost:5000/) 觀察影像的出現和 `<img src''>` 屬性點到 `http://localhost:4503`。

   ![已修復損壞的影像](assets/publish-deployment/broken-images-fixed.png)

## 模擬內容發佈 {#content-publish}

記得GraphQL錯誤 `adventureContributor` 請求「冒險詳細資訊」頁面時。 的 **貢獻者** 發佈實例上尚不存在內容片段模型。 對 **冒險** 內容片段模型在發佈實例上也不可用。 這些更改是直接對「作者」實例進行的，需要分發到「發佈」實例。

在向依賴內容片段或內容片段模型更新的應用程式推出新更新時，需要考慮這一點。

接下來，允許模擬本地作者和發佈實例之間的內容發佈。

1. 啟動「作者」實例（如果尚未啟動）並導航到「包管理器」(Package Manager) [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 下載包 [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) 並使用包管理器安裝。

   此包安裝使作者實例能夠將內容發佈到發佈實例的配置。 手動步驟 [此配置可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution)。

   >[!NOTE]
   >
   > 在AEMas a Cloud Service環境中，作者層將自動設定為將內容分發到發佈層。

1. 從 **開AEM始** 菜單，導航至 **工具** > **資產** > **內容片段模型**。

1. 按一下 **WKND站點** 的子菜單。

1. 選取所有三個模型，然後按一下 **發佈**:

   ![發佈內容片段模型](assets/publish-deployment/publish-contentfragment-models.png)

   出現確認對話框，按一下 **發佈**。

1. 導航至位於的Bali Surf Camp內容片段 [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)。

1. 按一下 **發佈** 按鈕。

   ![按一下內容片段編輯器中的「發佈」按鈕](assets/publish-deployment/publish-bali-content-fragment.png)

1. 「發佈」嚮導顯示應發佈的任何從屬資產。 在本例中，引用的片段 **斯泰西·羅斯威爾斯** 列出並引用多個影像。 引用的資產與片段一起發佈。

   ![要發佈的引用資產](assets/publish-deployment/referenced-assets.png)

   按一下 **發佈** 按鈕以發佈內容片段和從屬資產。

1. 返回到運行於的React App [http://localhost:5000/](http://localhost:5000/)。 您現在可以點擊巴釐島衝浪營地，查看探險細節。

1. 切換回AEM Author實例，位於 [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) 並更新 **標題** 碎片。 **保存並關閉** 碎片。 然後 **發佈** 碎片。
1. 返回到 [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) 並觀察已發佈的更改。

   ![巴釐島衝浪訓練營發佈更新](assets/publish-deployment/bali-surf-camp-update.png)

## 更新COR配置

默AEM認情況下是安全的，不允AEM許非web屬性進行客戶端調用。 AEM跨源資源共用(CORS)配置允許特定域調用AEM。

接下來，對AEM發佈實例的CORS配置進行實驗。

1. 返回到使用命令運行React App的終端窗口 `npm run serve`:

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

   請注意提供了兩個URL。 一個使用 `localhost` 另一個使用本地網路IP地址。

1. 導航到以下地址開始的地址 [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)。 每個本地電腦的地址稍有不同。 請注意在獲取資料時存在CORS錯誤。 這是因為當前的CORS配置僅允許 `localhost`。

   ![CORS錯誤](assets/publish-deployment/cors-error-not-fetched.png)

   接下來，更新AEM發佈CORS配置以允許來自網路IP地址的請求。

1. 導航到 [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) 用用戶名登錄 `admin` 和密碼 `admin`。

1. 導航到 [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) 並找到WKNDGraphQL配置 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`。

1. 更新 **允許的源** 欄位以包括網路IP地址：

   ![更新CORS配置](assets/publish-deployment/cors-update.png)

   也可以包含規則運算式以允許來自特定子域的所有請求。 儲存變更。

1. 搜索 **Apache Sling引用篩選器** 並查看配置。 的 **允許空** 還需要配置才能從外部域啟用GraphQL請求。

   ![Sling 查閱者篩選器](assets/publish-deployment/sling-referrer-filter.png)

   這些已配置為WKND參考站點的一部分。 您可以通過以下方式查看OSGi配置的完整集 [GitHub儲存庫](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)。

   >[!NOTE]
   >
   > OSGi配置在提交AEM到原始碼管理的項目中管理。 使用AEM雲管理器可將項AEM目部署為Cloud Service環境。 的 [項AEM目原型](https://github.com/adobe/aem-project-archetype) 有助於生成特定實施的項目。

1. 返回到React App，從 [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) 並觀察應用程式不再引發CORS錯誤。

   ![CORS錯誤已更正](assets/publish-deployment/cors-error-corrected.png)

## 恭喜！ {#congratulations}

恭喜！您現在已使用AEM發佈環境模擬完整的生產部署。 您還在中學習了如何使用CORS配AEM置。

## 其他資源

有關內容片段和GraphQL的詳細資訊，請參閱以下資源：

* [使用內容片段與GraphQL進行無頭內容傳遞](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [與內容片段搭配使用的 AEM GraphQL API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [基於令牌的身份驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [將代碼部署到AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
