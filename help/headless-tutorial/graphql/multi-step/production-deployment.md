---
title: 使用AEM Publish服務的生產部署 — AEM Headless快速入門 — GraphQL
description: 瞭解AEM作者和Publish服務，以及Headless應用程式的建議部署模式。 在本教學課程中，瞭解如何使用環境變數，根據目標環境動態變更GraphQL端點。 瞭解如何正確設定AEM以進行跨原始資源共用(CORS)。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
duration: 486
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2137'
ht-degree: 4%

---

# 搭配AEM Publish服務的生產部署

在本教學課程中，您將設定本機環境以模擬從Author例項散發到Publish例項的內容。 您也會產生React應用程式的生產組建，該應用程式已設定為透過GraphQL API使用AEM Publish環境中的內容。 在此過程中，您將瞭解如何有效地使用環境變數以及如何更新AEM CORS設定。

## 先決條件

本教學課程是多部分教學課程的一部分。 假設前幾個部分中概述的步驟已經完成。

## 目標

瞭解如何：

* 瞭解AEM作者和Publish架構。
* 瞭解管理環境變數的最佳實務。
* 瞭解如何正確設定AEM以進行跨原始資源共用(CORS)。

## 編寫Publish部署模式 {#deployment-pattern}

完整的 AEM 環境由編寫、發佈和 Dispatcher 組成。Author服務是內部使用者建立、管理和預覽內容的地方。 Publish服務被視為「即時」環境，通常是使用者與之互動的對象。 在Author服務上編輯及核准後的內容，會發佈至Publish服務。

AEM Headless 應用程式最常見的部署模式是讓應用程式的生產版本連接到 AEM Publish 服務。

![高階部署模式](assets/publish-deployment/high-level-deployment.png)

上圖描述了這種常見的部署模式。

1. **內容作者**&#x200B;使用AEM作者服務來建立、編輯及管理內容。
2. **內容作者**&#x200B;和其他內部使用者可以直在作者服務預覽內容。可以設定連接到作者服務的應用程式預覽版本。
3. 內容獲得核准後，就可以&#x200B;**發佈**&#x200B;到AEM Publish服務。
4. **一般使用者**&#x200B;與應用程式的生產版本互動。 生產應用程式會連線至Publish服務，並使用GraphQL API來請求和使用內容。

本教學課程會將AEM Publish例項新增至目前的設定，以模擬上述部署。 在之前的章節中，React應用程式會直接連線至Author例項，以作為預覽。 React應用程式的生產組建會部署至連線到新Publish執行個體的靜態Node.js伺服器。

最後，有三部本機伺服器在執行中：

* http://localhost:4502 — 作者例項
* http://localhost:4503 - Publish例項
* http://localhost:5000 — 生產模式中的React應用程式，連線至Publish執行個體。

## 安裝AEM SDK - Publish模式 {#aem-sdk-publish}

目前有一個SDK執行個體以&#x200B;**作者**&#x200B;模式執行。 SDK也可以在&#x200B;**Publish**&#x200B;模式下啟動，以模擬AEM Publish環境。

如需設定本機開發環境[的詳細指南，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up)。

1. 在您的本機檔案系統上，建立專用資料夾以安裝Publish執行個體，亦即名為`~/aem-sdk/publish`。
1. 複製前幾章中用於Author執行個體的Quickstart jar檔案，並將其貼到`publish`目錄中。 或者，導覽至[軟體發佈入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)，下載最新的SDK並解壓縮Quickstart jar檔案。
1. 將jar檔案重新命名為`aem-publish-p4503.jar`。

   `publish`字串指定Quickstart jar以Publish模式啟動。 `p4503`指定Quickstart伺服器在連線埠4503上執行。

1. 開啟新的終端機視窗，並瀏覽到包含jar檔案的資料夾。 安裝並啟動AEM執行個體：

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 提供管理員密碼做為`admin`。 可接受任何管理員密碼，但建議使用預設的本機開發以避免額外的設定。
1. 當AEM執行個體完成安裝時，將在[http://localhost:4503/content.html](http://localhost:4503/content.html)開啟新的瀏覽器視窗

   應該會傳回404 「找不到」頁面。 這是全新的AEM例項，尚未安裝任何內容。

## 安裝範例內容和GraphQL端點 {#wknd-site-content-endpoints}

就像在製作執行個體上一樣，Publish執行個體需要啟用GraphQL端點，並需要範例內容。 接下來，在Publish執行個體上安裝WKND參考網站。

1. 下載適用於WKND網站的最新編譯的AEM套件： [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest)。

   >[!NOTE]
   >
   > 請確定下載與AEM as a Cloud Service相容的標準版本，**不** `classic`版本。

1. 透過直接導覽至[http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html)登入Publish執行個體，使用者名稱為`admin`，密碼為`admin`。
1. 接著，瀏覽至[http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp)的封裝管理員。
1. 按一下&#x200B;**上傳封裝**，然後選擇在先前步驟中下載的WKND封裝。 按一下&#x200B;**安裝**&#x200B;以安裝封裝。
1. 安裝套件後，現在可在[http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html)取得WKND參考網站。
1. 按一下功能表列中的[登出]按鈕，登出`admin`使用者。

   ![WKND登出參考網站](assets/publish-deployment/sign-out-wknd-reference-site.png)

   與AEM Author例項不同，AEM Publish例項預設為匿名唯讀存取。 我們希望在執行React應用程式時模擬匿名使用者的體驗。

## 更新環境變數以指向Publish執行個體 {#react-app-publish}

接下來，更新React應用程式使用的環境變數，以指向Publish執行個體。 React應用程式應&#x200B;**僅**&#x200B;連線到生產模式的Publish執行個體。

接下來，新增檔案`.env.production.local`以模擬生產體驗。

1. 在IDE中開啟WKND GraphQL React應用程式。

1. 在`aem-guides-wknd-graphql/react-app`下方，新增名為`.env.production.local`的檔案。
1. 以下列專案填入`.env.production.local`：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![新增環境變數檔案](assets/publish-deployment/env-production-local-file.png)

   使用環境變數可讓您在Author或Publish環境之間切換GraphQL端點，而不需在應用程式程式碼內新增額外邏輯。 有關React的[自訂環境變數的詳細資訊，請參閱此處](https://create-react-app.dev/docs/adding-custom-environment-variables)。

   >[!NOTE]
   >
   > 請注意，由於Publish環境預設會提供匿名內容存取權，因此不會包含驗證資訊。

## 部署靜態節點伺服器 {#static-server}

React應用程式可使用webpack伺服器啟動，但這僅適用於開發。 接下來，使用[伺服器](https://github.com/vercel/serve)來模擬生產部署，以使用Node.js主控React應用程式的生產組建。

1. 開啟新的終端機視窗並瀏覽至`aem-guides-wknd-graphql/react-app`目錄

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 使用以下命令安裝[伺服器](https://github.com/vercel/serve)：

   ```shell
   $ npm install serve --save-dev
   ```

1. 在`react-app/package.json`開啟檔案`package.json`。 新增名為`serve`的指令碼：

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   `serve`指令碼執行兩個動作。 首先，產生React應用程式的生產組建。 接著，Node.js伺服器啟動並使用生產組建。

1. 返回終端機並輸入啟動靜態伺服器的命令：

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

1. 開啟新的瀏覽器，並導覽至[http://localhost:5000/](http://localhost:5000/)。 您應該會看到系統提供React應用程式。

   ![已提供React應用程式](assets/publish-deployment/react-app-served-port5000.png)

   請注意，首頁上正在進行GraphQL查詢。 使用您的開發人員工具Inspect **XHR**&#x200B;要求。 請注意，GraphQLPOST位於`http://localhost:4503/content/graphql/global/endpoint.json`的Publish執行個體。

   不過，首頁上的所有影像都會損毀！

1. 按一下進入其中一個Adventure Detail頁面。

   ![冒險詳細資料錯誤](assets/publish-deployment/adventure-detail-error.png)

   觀察到`adventureContributor`擲回GraphQL錯誤。 在下個練習中，已修正損壞的影像和`adventureContributor`問題。

## 絕對影像參照 {#absolute-image-references}

影像似乎已損毀，因為`<img src`屬性已設定為相對路徑，且結尾指向`http://localhost:5000/`的Node靜態伺服器。 這些影像應該指向AEM Publish例項。 對此有幾種可能的解決方案。 使用webpack開發伺服器時，檔案`react-app/src/setupProxy.js`會在webpack伺服器與AEM作者執行個體之間設定Proxy，以處理對`/content`的任何要求。 Proxy設定可用於生產環境，但必須在Web伺服器層級進行設定。 例如，[Apache的Proxy模組](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html)。

應用程式可以更新為包含使用`REACT_APP_HOST_URI`環境變數的絕對URL。 我們改用AEM GraphQL API的功能來要求影像的絕對URL。

1. 停止Node.js伺服器。
1. 返回IDE並在`react-app/src/components/Adventures.js`開啟檔案`Adventures.js`。
1. 在`allAdventuresQuery`內將`_publishUrl`屬性新增至`ImageRef`：

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

   `_publishUrl`和`_authorUrl`是內建在`ImageRef`物件中的值，以便更輕鬆地包含絕對url。

1. 重複上述步驟，修改`filterQuery(activity)`函式中使用的查詢，以包含`_publishUrl`屬性。
1. 在建構`<img src=''>`標籤時，修改位於`function AdventureItem(props)`的`AdventureItem`元件以參照`_publishUrl`而非`_path`屬性：

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. 在`react-app/src/components/AdventureDetail.js`開啟檔案`AdventureDetail.js`。
1. 重複相同的步驟來修改GraphQL查詢，並為冒險新增`_publishUrl`屬性

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

1. 修改`AdventureDetail.js`中Adventure主要影像和投稿人圖片參考的兩個`<img>`標籤：

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

1. 返回終端機並啟動靜態伺服器：

   ```shell
   $ npm run serve
   ```

1. 導覽至[http://localhost:5000/](http://localhost:5000/)，並觀察影像是否出現，以及`<img src''>`屬性是否指向`http://localhost:4503`。

   ![已修復損毀的影像](assets/publish-deployment/broken-images-fixed.png)

## 模擬內容發佈 {#content-publish}

請記得，請求「冒險詳細資料」頁面時，`adventureContributor`擲回GraphQL錯誤。 Publish執行個體上尚未存在&#x200B;**貢獻者**&#x200B;內容片段模型。 對&#x200B;**Adventure**&#x200B;內容片段模型所做的更新也無法在Publish執行個體上使用。 這些變更是直接針對作者執行個體所進行，需要散發至Publish執行個體。

向依賴內容片段更新或內容片段模式的應用程式推出新更新時，需要考量這一點。

接下來，可模擬本機作者與Publish執行個體之間的內容發佈。

1. 啟動作者執行個體（如果尚未啟動），並瀏覽至[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)的封裝管理員
1. 下載套件[EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip)，並使用封裝管理員進行安裝。

   此套件會安裝可讓作者執行個體將內容發佈至Publish執行個體的設定。 您可在此找到[此設定的手動步驟](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution)。

   >[!NOTE]
   >
   > 在AEM as a Cloud Service環境中，作者層級會自動設定為將內容發佈至Publish層級。

1. 從&#x200B;**AEM Start**&#x200B;功能表，導覽至&#x200B;**工具** > **Assets** > **內容片段模式**。

1. 按一下&#x200B;**WKND網站**&#x200B;資料夾。

1. 選取所有三個模型，然後按一下&#x200B;**Publish**：

   ![Publish內容片段模型](assets/publish-deployment/publish-contentfragment-models.png)

   出現確認對話方塊，按一下&#x200B;**Publish**。

1. 瀏覽至[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的「巴厘島衝浪營」內容片段。

1. 按一下頂端功能表列中的&#x200B;**Publish**&#x200B;按鈕。

   ![按一下內容片段編輯器中的Publish按鈕](assets/publish-deployment/publish-bali-content-fragment.png)

1. Publish精靈會顯示任何應發佈的相依資產。 在此情況下，會列出參照的片段&#x200B;**stacey-roswells**，也會參照數個影像。 引用的資產會與片段一起發佈。

   ![參考要發佈的Assets](assets/publish-deployment/referenced-assets.png)

   再次按一下&#x200B;**Publish**&#x200B;按鈕以發佈內容片段和相依資產。

1. 返回在[http://localhost:5000/](http://localhost:5000/)執行的React應用程式。 您現在可以按一下Bali Surf Camp檢視冒險細節。

1. 切換回[http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)的AEM Author執行個體並更新片段的&#x200B;**Title**。 **儲存並關閉**&#x200B;片段。 然後&#x200B;**發佈**&#x200B;片段。
1. 返回[http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp)並觀察發佈的變更。

   ![巴厘島衝浪營Publish更新](assets/publish-deployment/bali-surf-camp-update.png)

## 更新COR設定

AEM預設為安全，不允許非AEM Web屬性進行使用者端呼叫。 AEM的跨原始資源共用(CORS)設定可允許特定網域呼叫AEM。

接下來，實驗AEM Publish例項的CORS設定。

1. 返回終端機視窗，其中使用命令`npm run serve`執行React應用程式：

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

   請注意，系統已提供兩個URL。 一個使用`localhost`，另一個使用本機網路IP位址。

1. 導覽至開頭為[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)的地址。 每個本機電腦的位址稍有不同。 請注意，擷取資料時發生CORS錯誤。 這是因為目前的CORS設定僅允許來自`localhost`的請求。

   ![CORS錯誤](assets/publish-deployment/cors-error-not-fetched.png)

   接下來，更新AEM Publish CORS設定，以允許來自網路IP位址的請求。

1. 導覽至[http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html)，並使用使用者名稱`admin`和密碼`admin`登入。

1. 導覽至[http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr)，並在`com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`找到WKND GraphQL設定。

1. 更新&#x200B;**允許的來源**&#x200B;欄位以包含網路IP位址：

   ![更新CORS組態](assets/publish-deployment/cors-update.png)

   也可以包含規則運算式，以允許來自特定子網域的所有請求。 儲存變更。

1. 搜尋&#x200B;**Apache Sling反向連結篩選器**&#x200B;並檢閱設定。 還需要有&#x200B;**允許空白**&#x200B;設定，才能啟用來自外部網域的GraphQL要求。

   ![Sling 查閱者篩選器](assets/publish-deployment/sling-referrer-filter.png)

   這些內容已設定為WKND參考網站的一部分。 您可以透過[GitHub存放庫](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig)檢視完整的OSGi設定集。

   >[!NOTE]
   >
   > OSGi設定是在認可給原始檔控制的AEM專案中進行管理。 AEM專案可以使用Cloud Manager部署到AEM作為Cloud Service環境。 [AEM專案原型](https://github.com/adobe/aem-project-archetype)可協助產生特定實作的專案。

1. 返回以[http://192.168.86.XXX:5000](http://192.168.86.XXX:5000)開頭的React應用程式，並觀察應用程式不再擲回CORS錯誤。

   ![CORS錯誤已更正](assets/publish-deployment/cors-error-corrected.png)

## 恭喜！ {#congratulations}

恭喜！您現在已使用AEM Publish環境模擬完整生產部署。 您也學習了如何在AEM中使用CORS設定。

## 其他資源

如需內容片段和GraphQL的詳細資訊，請參閱下列資源：

* 使用內容片段搭配GraphQL的[Headless內容傳遞](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [與內容片段搭配使用的 AEM GraphQL API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [權杖型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [將程式碼部署至AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
