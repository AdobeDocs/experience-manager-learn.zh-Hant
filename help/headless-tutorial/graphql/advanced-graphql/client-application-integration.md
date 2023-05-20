---
title: 客戶端應用程式整合 — 無頭的AEM高級概念 — GraphQL
description: 實施永續查詢並將其整合到WKND應用中。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 2%

---

# 客戶端應用程式整合

在上一章中，您使用GraphiQL Explorer建立和更新了永續查詢。

本章將引導您完成使用現有HTTPGET請求將永續查詢與WKND客戶端應用程式（又稱WKND應用程式）整合的步驟 **反應元件**。 它還為您應用無頭學習、編AEM碼專業知識來增強WKND客戶端應用程式提供了可選的挑戰。

## 必備條件 {#prerequisites}

本文檔是多部分教程的一部分。 在繼續本章之前，請確保已完成前幾章。 WKND客戶端應用程式連AEM接到發佈服務，因此您 **將以下內容發佈AEM到發佈服務**。

* 項目配置
* GraphQL 端點
* 內容片段模型
* 創作的內容片段
* GraphQL永續查詢

的 _本章中的IDE螢幕截圖來自 [Visual Studio代碼](https://code.visualstudio.com/)_

### 第1-4章解決方案包（可選） {#solution-package}

可以安裝一個解決方案包，以完成UI中第1AEM-4章的步驟。 此包是 **不需要** 的下界。

1. 下載 [高級 — GraphQL — 教程 — 解決方案 — 軟體包–1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip)。
1. 在AEM中，導航到 **工具** > **部署** > **包** 訪問 **包管理器**。
1. 上載並安裝上一步中下載的包（zip檔案）。
1. 將包複製到AEM發佈服務

## 目標 {#objectives}

在本教程中，您將學習如何使用WKNDGraphQL回應應用將永續查詢請求整合到示例 [用AEM於JavaScript的無頭客戶端](https://github.com/adobe/aem-headless-client-js)。

## 克隆並運行示例客戶端應用程式 {#clone-client-app}

為加速本教程，提供了入門版React JS應用。

1. 克隆 [adobe/aem參考線 — wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) 儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/advanced-tutorial/.env.development` 檔案和設定 `REACT_APP_HOST_URI` 指向目標發佈AEM服務。

   如果連接到作者實例，請更新驗證方法。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![響應應用程式開發環境](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > 上述說明是將React應用程式連接到 **AEM發佈服務**，但要連接到 **AEM作者服務** 獲取目標as a Cloud Service環境的本地開發AEM令牌。
   >
   > 還可以將應用連接到 [使用AEMaaCS SDK的本地作者實例](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 使用基本身份驗證。


1. 開啟終端並運行以下命令：

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. 新瀏覽器窗口應載入 [http://localhost:3000](http://localhost:3000)


1. 點擊 **野營** > **約塞米蒂背包** 查看Yosemite Backpacking冒險詳細資訊。

   ![Yosemite背包螢幕](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 開啟瀏覽器的開發人員工具並檢查 `XHR` 請求

   ![POSTGraphQL](assets/client-application-integration/graphql-persisted-query.png)

   你應該看到 `GET` 請求到項目配置名稱為(`wknd-shared`)，永續查詢名稱(`adventure-by-slug`)，變數名稱(`slug`)，值(`yosemite-backpacking`)和特殊字元編碼。

>[!IMPORTANT]
>
>    如果您想知道為什麼針對 `http://localhost:3000` 和NOT針對AEM發佈服務域，請查看 [《煙帽下》](../multi-step/graphql-and-react-app.md#under-the-hood) 從基本教程。


## 查看代碼

在 [基本教程 — 構建使用GraphQLAPI的AEMReact應用](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) 我們已審查並增強了很少的關鍵檔案，以獲得實際操作的專業知識。 在增強WKND應用之前，請查看密鑰檔案。

* [查看AEMHeadles對象](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [實施以運行AEMGraphQL永續查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### 審閱 `Adventures` 反應元件

WKND React應用的主視圖是所有冒險的清單，您可以根據活動類型篩選這些冒險 _野營，騎腳踏車_。 此視圖由 `Adventures` 元件。 以下是主要實施細節：

* 的 `src/components/Adventures.js` 呼叫 `useAllAdventures(adventureActivity)` 鈎子 `adventureActivity` 參數是活動類型。

* 的 `useAllAdventures(adventureActivity)` 掛接在 `src/api/usePersistedQueries.js` 的子菜單。 基於 `adventureActivity` 值，它確定要調用的永續查詢。 如果不為null值，則調用 `wknd-shared/adventures-by-activity`，否則將獲得所有可用的冒險 `wknd-shared/adventures-all`。

* 鈎子使用主 `fetchPersistedQuery(..)` 將查詢執行委派到 `AEMHeadless` 通過 `aemHeadlessClient.js`。

* 此掛接僅返回GraphQL響應AEM的相關資料 `response.data?.adventureList?.items`，允許 `Adventures` 對父JSON結構不可知的視圖元件進行反應。

* 成功執行查詢後， `AdventureListItem(..)` render函式 `Adventures.js` 添加HTML元素以顯示 _影像、行程長度、價格和標題_ 的下界。

### 審閱 `AdventureDetail` 反應元件

的 `AdventureDetail` React元件呈現冒險的詳細資訊。 以下是主要實施細節：

* 的 `src/components/AdventureDetail.js` 呼叫 `useAdventureBySlug(slug)` 鈎子 `slug` 參數是查詢參數。

* 就像上面， `useAdventureBySlug(slug)` 掛接在 `src/api/usePersistedQueries.js` 的子菜單。 它叫 `wknd-shared/adventure-by-slug` 通過委託進行持久查詢 `AEMHeadless` 通過 `aemHeadlessClient.js`。

* 成功執行查詢後， `AdventureDetailRender(..)` render函式 `AdventureDetail.js` 添加HTML元素以顯示冒險詳細資訊。


## 增強代碼

### 使用 `adventure-details-by-slug` 永續查詢

在上一章中，我們建立了 `adventure-details-by-slug` 永續查詢，它提供其他冒險資訊，如 _位置、教師團隊和管理員_。 讓我們替換 `adventure-by-slug` 與 `adventure-details-by-slug` 永續查詢以呈現此附加資訊。

1. 開啟 `src/api/usePersistedQueries.js`.

1. 定位函式 `useAdventureBySlug()` 將查詢更新為

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### 顯示其他資訊

1. 要顯示其他冒險資訊，請開啟 `src/components/AdventureDetail.js`

1. 定位函式 `AdventureDetailRender(..)` 將返回函式更新為

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. 還定義相應的渲染函式：

   **位置資訊**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **位置**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **教師團隊**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **管理員**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### 定義新樣式

1. 開啟 `src/components/AdventureDetail.scss` 添加以下類定義

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>更新的檔案可在 **AEM嚮導 — GraphQL** 項目，請參閱 [高級教程](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) 的子菜單。


完成上述增強後，WKND App如下所示，瀏覽器的開發人員工具顯示 `adventure-details-by-slug` 永續查詢調用。

![增強的WKND應用](assets/client-application-integration/Enhanced-WKND-APP.gif)

## 增強挑戰（可選）

WKND React應用的主視圖允許您根據活動類型(如 _野營，騎腳踏車_。 但WKND業務團隊希望 _位置_ 基於過濾功能。 要求是

* 在WKND App的主視圖中，在左上角或右上角添加 _位置_ 「篩選」表徵圖。
* 按一下 _位置_ 篩選表徵圖應顯示位置清單。
* 從清單中按一下所需位置選項應僅顯示匹配的冒險。
* 如果只有一個匹配的「冒險」，將顯示「冒險詳細資訊」視圖。

## 恭喜

恭喜！您現在已完成將永續查詢整合併實施到示例WKND應用中。
