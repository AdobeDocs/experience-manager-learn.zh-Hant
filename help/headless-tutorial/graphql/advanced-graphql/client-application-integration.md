---
title: 使用者端應用程式整合 — AEM Headless的進階概念 — GraphQL
description: 實作持續性查詢並將其整合到WKND應用程式中。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
duration: 241
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 1%

---

# 使用者端應用程式整合

在上一章中，您使用GraphiQL Explorer建立和更新持久查詢。

本章會逐步引導您完成步驟，在現有&#x200B;**React元件**&#x200B;中使用HTTP GET要求，將持續性查詢與WKND使用者端應用程式（亦稱為WKND應用程式）整合。 此外也提供選購挑戰功能，讓您運用AEM Headless學習經驗、編碼專業知識來增強WKND使用者端應用程式。

## 先決條件 {#prerequisites}

本檔案是多部分教學課程的一部分。 在繼續本章之前，請確定已完成前面的章節。 WKND使用者端應用程式連線至AEM發佈服務，因此您&#x200B;**發佈下列內容至AEM發佈服務**&#x200B;非常重要。

* 專案設定
* GraphQL 端點
* 內容片段模型
* 編寫的內容片段
* GraphQL持續查詢

本章中的&#x200B;_IDE熒幕擷取畫面來自[Visual Studio Code](https://code.visualstudio.com/)_

### 第1-4章解決方案套件（選填） {#solution-package}

可安裝的解決方案套件可完成第1-4章的AEM UI中的步驟。 如果前幾章已完成，則此封裝&#x200B;**不需要**。

1. 下載[Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip)。
1. 在AEM中，瀏覽至&#x200B;**工具** > **部署** > **套件**&#x200B;以存取&#x200B;**套件管理員**。
1. 上傳並安裝上一步驟中下載的套件（zip檔案）。
1. 將套件復寫至AEM Publish服務

## 目標 {#objectives}

在本教學課程中，您將瞭解如何使用適用於JavaScript的[AEM Headless Client ](https://github.com/adobe/aem-headless-client-js)，將持續性查詢的請求整合到範例WKND GraphQL React應用程式中。

## 複製並執行範例使用者端應用程式 {#clone-client-app}

為加速教學課程，我們提供入門版React JS應用程式。

1. 複製[adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql)存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯`aem-guides-wknd-graphql/advanced-tutorial/.env.development`檔案並將`REACT_APP_HOST_URI`設定為指向您的目標AEM發佈服務。

   如果連線到作者執行個體，請更新驗證方法。

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
   > 上述指示是將React應用程式連線至&#x200B;**AEM Publish服務**，但若要連線至&#x200B;**AEM Author服務**，請取得目標AEM as a Cloud Service環境的本機開發權杖。
   >
   > 也可以使用基本驗證，使用AEMaaCS SDK[&#128279;](/help/headless-tutorial/graphql/quick-setup/local-sdk.md)將應用程式連線到本機作者執行個體。


1. 開啟終端機並執行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器視窗應載入[http://localhost:3000](http://localhost:3000)


1. 點選&#x200B;**露營** > **Yosemite Backpacking**&#x200B;以檢視Yosemite Backpacking冒險詳細資料。

   ![Yosemite揹包熒幕](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 開啟瀏覽器的開發人員工具並檢查`XHR`請求

   ![發佈GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   您應該會看到對GraphQL端點的`GET`個要求，其中包含專案設定名稱(`wknd-shared`)、持續查詢名稱(`adventure-by-slug`)、變數名稱(`slug`)、值(`yosemite-backpacking`)和特殊字元編碼。

>[!IMPORTANT]
>
>    若您想知道為什麼GraphQL API要求是針對`http://localhost:3000`而非AEM Publish Service網域提出，請參閱基本教學課程的[&#128279;](../multi-step/graphql-and-react-app.md#under-the-hood)主題。


## 檢閱程式碼

在[基本教學課程 — 使用AEM的GraphQL API建置React應用程式](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=zh-Hant#review-the-aemheadless-object)步驟中，我們已檢閱並增強一些重要檔案，以獲得實作專業知識。 增強WKND應用程式之前，請先檢閱重要檔案。

* [檢閱AEMHeadless物件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=zh-Hant#review-the-aemheadless-object)

* [實作以執行AEM GraphQL持續查詢](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html?lang=zh-Hant#implement-to-run-aem-graphql-persisted-queries)

### 檢閱`Adventures` React元件

WKND React應用程式的主要檢視是所有冒險清單，您可以根據&#x200B;_露營、騎腳踏車_&#x200B;之類的活動型別篩選這些冒險。 此檢視由`Adventures`元件轉譯。 以下是主要實作詳細資料：

* `src/components/Adventures.js`呼叫`useAllAdventures(adventureActivity)`連結，而其中`adventureActivity`引數為活動型別。

* 已在`src/api/usePersistedQueries.js`檔案中定義`useAllAdventures(adventureActivity)`連結。 根據`adventureActivity`值，它決定要呼叫哪個持續查詢。 如果不是null值，它會呼叫`wknd-shared/adventures-by-activity`，否則會取得所有可用的冒險`wknd-shared/adventures-all`。

* 掛接使用透過`aemHeadlessClient.js`將查詢執行委派給`AEMHeadless`的主要`fetchPersistedQuery(..)`函式。

* 此掛接也只會從`response.data?.adventureList?.items`的AEM GraphQL回應傳回相關資料，允許`Adventures` React檢視元件與父JSON結構無關。

* 成功執行查詢後，`Adventures.js`的`AdventureListItem(..)`轉譯器函式會新增HTML元素，以顯示&#x200B;_影像、旅行長度、價格和標題_&#x200B;資訊。

### 檢閱`AdventureDetail` React元件

`AdventureDetail` React元件會呈現冒險的詳細資訊。 以下是主要實作詳細資料：

* `src/components/AdventureDetail.js`呼叫`useAdventureBySlug(slug)`連結，而這裡`slug`引數是查詢引數。

* 如上所述，`useAdventureBySlug(slug)`連結定義於`src/api/usePersistedQueries.js`檔案中。 它透過`aemHeadlessClient.js`委派給`AEMHeadless`來呼叫`wknd-shared/adventure-by-slug`持續查詢。

* 成功執行查詢後，`AdventureDetail.js`的`AdventureDetailRender(..)`轉譯器函式會新增HTML元素來顯示Adventure詳細資料。


## 增強程式碼

### 使用`adventure-details-by-slug`持久查詢

在上一章中，我們建立了`adventure-details-by-slug`持續查詢，它提供其他冒險資訊，例如&#x200B;_位置、instructorTeam和管理員_。 讓我們將`adventure-by-slug`取代為`adventure-details-by-slug`持續查詢，以呈現此額外資訊。

1. 開啟`src/api/usePersistedQueries.js`。

1. 尋找函式`useAdventureBySlug()`並將查詢更新為

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

1. 若要顯示其他冒險資訊，請開啟`src/components/AdventureDetail.js`

1. 找到函式`AdventureDetailRender(..)`並將傳回函式更新為

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

1. 同時定義對應的轉譯器函式：

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

   **講師團隊**

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

1. 開啟`src/components/AdventureDetail.scss`並新增下列類別定義

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
>更新的檔案可在&#x200B;**AEM Guides WKND - GraphQL**&#x200B;專案下取得，請參閱[進階教學課程](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial)區段。


完成上述增強功能後，WKND應用程式看起來如下所示，瀏覽器的開發人員工具顯示`adventure-details-by-slug`個持續的查詢呼叫。

![增強型WKND應用程式](assets/client-application-integration/Enhanced-WKND-APP.gif)

## 增強功能挑戰（選購）

WKND React應用程式的主要檢視可讓您根據活動型別（例如&#x200B;_露營、循環_）篩選這些冒險活動。 不過，WKND業務團隊想要額外的&#x200B;_位置_&#x200B;篩選功能。 需求為

* 在WKND應用程式的主檢視上，在左上角或右上角新增&#x200B;_位置_&#x200B;篩選圖示。
* 按一下&#x200B;_位置_&#x200B;篩選圖示應該會顯示位置清單。
* 從清單中按一下所需的位置選項時，應該只會顯示相符的「冒險」。
* 如果只有一個相符的Adventure，則會顯示Adventure詳細資料檢視。

## 恭喜

恭喜！您現在已完成整合，並將持續性查詢實作到範例WKND應用程式中。
