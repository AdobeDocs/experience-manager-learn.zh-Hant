---
title: 用戶端應用程式整合 — AEM無周邊的進階概念 — GraphQL
description: 實作持續的查詢並將其整合至WKND應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# 客戶端應用程式整合

在上一章中，您使用GraphiQL資源管理器建立並更新了持續查詢。

本章會逐步帶您了解使用現有內的HTTPGET要求，將保存的查詢與WKND用戶端應用程式（亦稱為WKND應用程式）整合的步驟 **React元件**. 它還為應用您的AEM無頭學習、編碼專業知識以增強WKND客戶端應用程式提供了一個可選挑戰。

## 必備條件 {#prerequisites}

本檔案是多部分教學課程的一部分。 在繼續處理本章之前，請確保已完成前幾章。 WKND用戶端應用程式會連線至AEM發佈服務，因此請務必 **將下列內容發佈至AEM發佈服務**.

* 專案組態
* GraphQL 端點
* 內容片段模型
* 製作內容片段
* GraphQL持續查詢

此 _本章中的IDE螢幕截圖來自 [Visual Studio代碼](https://code.visualstudio.com/)_

### 第1-4章解決方案套件（可選） {#solution-package}

可安裝解決方案套件，以完成AEM UI中章節1-4的步驟。 這個包是 **不需要** 如果先前章節已完成。

1. 下載 [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. 在AEM中，導覽至 **工具** > **部署** > **套件** 存取 **封裝管理員**.
1. 上傳並安裝上一步中下載的套件（zip檔案）。
1. 將套件復寫至AEM Publish Service

## 目標 {#objectives}

在本教學課程中，您將了解如何使用將持續查詢請求整合至範例WKND GraphQL React應用程式中， [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js).

## 克隆並運行示例客戶端應用程式 {#clone-client-app}

為加速本教學課程，我們提供入門React JS應用程式。

1. 複製 [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/advanced-tutorial/.env.development` 檔案和設定 `REACT_APP_HOST_URI` 來指向您的target AEM發佈服務。

   如果連線至製作例項，請更新驗證方法。

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

   ![React應用程式開發環境](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > 上述指示是將React應用程式連結至 **AEM發佈服務**，不過要連線至 **AEM作者服務** 取得目標AEMas a Cloud Service環境的本機開發代號。
   >
   > 您也可以將應用程式連結至 [使用AEMaaCS SDK的本機製作例項](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 使用基本驗證。


1. 開啟終端機並執行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. 新瀏覽器視窗應載入 [http://localhost:3000](http://localhost:3000)


1. 點選 **野營** > **約塞米蒂背包** 查看Yosemite Backpacking冒險詳細資訊。

   ![Yosemite回裝螢幕](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 開啟瀏覽器的開發人員工具，並檢查 `XHR` 請求

   ![POSTGraphQL](assets/client-application-integration/graphql-persisted-query.png)

   您應該會看到 `GET` 以專案設定名稱(`wknd-shared`)，持續查詢名稱(`adventure-by-slug`)，變數名稱(`slug`)，值(`yosemite-backpacking`)和特殊字元編碼。

>[!IMPORTANT]
>
>    如果您想知道為何對 `http://localhost:3000` 和NOT針對AEM Publish Service網域，請檢閱 [在兜帽下](../multi-step/graphql-and-react-app.md#under-the-hood) （從基本教學課程）。


## 檢閱程式碼

在 [基本教學課程 — 建立使用AEM GraphQL API的React應用程式](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) 我們審查並增強了幾個關鍵檔案，以獲得實際操作的專業知識。 增強WKND應用程式之前，請先檢閱密鑰檔案。

* [檢閱AEMHeadless物件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [實作以執行AEM GraphQL持續查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### 檢閱 `Adventures` React元件

WKND React應用程式的主要檢視是所有歷險的清單，您可以根據活動類型(如 _野營，騎腳踏車_. 此檢視由 `Adventures` 元件。 以下是主要實作詳細資料：

* 此 `src/components/Adventures.js` 呼叫 `useAllAdventures(adventureActivity)` 鈎子 `adventureActivity` 引數是活動類型。

* 此 `useAllAdventures(adventureActivity)` 鈎點定義於 `src/api/usePersistedQueries.js` 檔案。 根據 `adventureActivity` 值，它會決定要呼叫的持續查詢。 若非空值，則會呼叫 `wknd-shared/adventures-by-activity`，否則會取得所有可用的歷險 `wknd-shared/adventures-all`.

* 鈎子使用主 `fetchPersistedQuery(..)` 將查詢執行委派到的函式 `AEMHeadless` via `aemHeadlessClient.js`.

* 此掛接也只會從AEM GraphQL回應傳回相關資料(於 `response.data?.adventureList?.items`，允許 `Adventures` React檢視元件不受上層JSON結構限制。

* 成功執行查詢時， `AdventureListItem(..)` 從 `Adventures.js` 新增HTML元素以顯示 _影像、行程長度、價格和標題_ 資訊。

### 檢閱 `AdventureDetail` React元件

此 `AdventureDetail` React元件會呈現探險的詳細資訊。 以下是主要實作詳細資料：

* 此 `src/components/AdventureDetail.js` 呼叫 `useAdventureBySlug(slug)` 鈎子 `slug` 引數為查詢參數。

* 如上所示， `useAdventureBySlug(slug)` 鈎點定義於 `src/api/usePersistedQueries.js` 檔案。 它叫 `wknd-shared/adventure-by-slug` 將委派到 `AEMHeadless` via `aemHeadlessClient.js`.

* 成功執行查詢時， `AdventureDetailRender(..)` 從 `AdventureDetail.js` 新增HTML元素以顯示「冒險」詳細資訊。


## 增強程式碼

### 使用 `adventure-details-by-slug` 持續查詢

在上一章中，我們建立 `adventure-details-by-slug` 持續查詢，可提供額外的探險資訊，例如 _位置、instructorTeam和管理員_. 讓我們來替換 `adventure-by-slug` with `adventure-details-by-slug` 持續查詢來呈現此額外資訊。

1. 開啟 `src/api/usePersistedQueries.js`.

1. 找出函式 `useAdventureBySlug()` 將查詢更新為

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

1. 找出函式 `AdventureDetailRender(..)` 將傳回函式更新為

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

1. 也定義相應的渲染函式：

   **LocationInfo**

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

   **InstructorTeam**

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
>更新的檔案可在 **AEM指南WKND - GraphQL** 項目，請參閱 [進階教學課程](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) 區段。


完成上述增強功能後，WKND應用程式如下所示，瀏覽器的開發人員工具顯示 `adventure-details-by-slug` 持續查詢呼叫。

![增強的WKND應用程式](assets/client-application-integration/Enhanced-WKND-APP.gif)

## 增強功能挑戰（可選）

WKND React應用程式的主檢視可讓您根據活動類型(如 _野營，騎腳踏車_. 不過WKND業務團隊想要 _位置_ 篩選功能。 要求為

* 在WKND應用程式的主檢視中，在左上角或右上角新增 _位置_ 篩選圖示。
* 按一下 _位置_ 篩選圖示應會顯示位置清單。
* 從清單按一下所需的位置選項時，應該只會顯示相符的歷險。
* 如果只有一個相符的「冒險」，則會顯示「冒險詳細資訊」視圖。

## 恭喜

恭喜！您現在已完成整合，並將持續的查詢實作至範例WKND應用程式。
