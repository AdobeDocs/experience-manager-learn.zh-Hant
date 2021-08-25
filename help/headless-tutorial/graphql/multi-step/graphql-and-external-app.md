---
title: 使用外部應用程式的GraphQL查詢AEM — 開始使用AEM無周邊功能 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 探索範例WKND GraphQL React應用程式的AEM GraphQL API。 了解此外部應用程式如何對AEM進行GraphQL呼叫，以強化其使用體驗。 了解如何執行基本錯誤處理。
version: cloud-service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1398'
ht-degree: 0%

---


# 使用外部應用程式的GraphQL查詢AEM

在本章中，我們將探討如何使用AEM GraphQL API來促進外部應用程式中的體驗。

本教學課程使用簡單的React應用程式來查詢及顯示AEM GraphQL API公開的冒險內容。 React的使用在很大程度上不重要，任何平台的任何框架都可編寫耗用的外部應用程式。

## 必備條件

本教學課程分為多部分，假設已完成前幾部分所述的步驟。

_本章中的IDE螢幕截圖來自 [Visual Studio Code](https://code.visualstudio.com/)_

（可選）安裝瀏覽器擴展（如[GraphQL網路檢查器](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln)），以便查看有關GraphQL查詢的更多詳細資訊。

## 目標

在本章中，我們將學習如何：

* 開始並了解範例React應用程式的功能
* 探索如何從外部應用程式呼叫AEM GraphQL端點
* 定義GraphQL查詢，以依活動篩選歷險內容片段清單
* 更新React應用程式，提供控制項以透過GraphQL篩選，該清單為依活動的歷險清單

## 啟動React應用程式

由於本章著重於開發用戶端以透過GraphQL使用內容片段，因此必須下載範例[WKND GraphQL React應用程式原始碼，並在本機電腦上設定](./setup.md#react-app)，且[AEM SDK以作者服務](./setup.md#aem-sdk)的形式執行，並安裝[範例WKND網站](./setup.md#wknd-site)。

在[快速設定](./setup.md)章節中會詳細說明啟動React應用程式，但可遵循節略指示：

1. 如果尚未複製，請從[Github.com](https://github.com/adobe/aem-guides-wknd-graphql)複製範例WKND GraphQL React應用程式

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在IDE中開啟WKND GraphQL React應用程式

   ![VSCode中的React應用程式](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 從命令列導覽至`react-app`資料夾
1. 從項目根目錄（`react-app`資料夾）中執行以下命令，啟動WKND GraphQL React應用

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. 請前往[http://localhost:3000/](http://localhost:3000/)檢閱應用程式。 範例React應用程式有兩個主要部分：

   * 使用GraphQL在AEM中查詢&#x200B;__Adventure__&#x200B;內容片段，可讓首頁體驗充當WKND歷險的索引。 在本章中，我們將修改此檢視，以支援依活動篩選歷險。

      ![WKND GraphQL React應用程式 — 首頁體驗](./assets/graphql-and-external-app/react-home-view.png)

   * 探險詳細資訊體驗，使用GraphQL來查詢特定&#x200B;__探險__&#x200B;內容片段，並顯示更多資料點。

      ![WKND GraphQL React應用程式 — 詳細體驗](./assets/graphql-and-external-app/react-details-view.png)

1. 使用瀏覽器的開發工具和瀏覽器擴展（如[GraphQL網路檢查器](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln)）來檢查發送到AEM的GraphQL查詢及其JSON響應。 此方法可用來監控GraphQL請求和回應，以確保正確制定，並如預期般回應。

   ![AdventureList的原始查詢](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *從React應用程式傳送至AEM的GraphQL查詢*

   ![GraphQL JSON回應](assets/graphql-and-external-app/graphql-json-response.png)

   *從AEM回應至React應用程式的JSON回應*

   查詢和響應應與GraphiQL IDE中顯示的內容相匹配。

   >[!NOTE]
   >
   > 在開發期間， React應用程式會透過Webpack開發伺服器將HTTP要求代理傳送至AEM。 React應用程式會向`http://localhost:3000`提出請求，將其代理至`http://localhost:4502`上執行的AEM製作服務。 查看檔案`src/setupProxy.js`和`env.development`以了解詳細資訊。
   >
   > 在非開發情況下， React應用程式會直接設定為向AEM提出要求。

## 探索應用程式的GraphQL程式碼

1. 在IDE中，開啟檔案`src/api/useGraphQL.js`。

   這是[React Effect Hook](https://reactjs.org/docs/hooks-overview.html#effect-hook)，會監聽應用程式`query`的變更，且在變更時會向AEM GraphQL端點發出HTTPPOST請求，並將JSON回應傳回應用程式。

   每當React應用程式需要進行GraphQL查詢時，就會叫用此自訂`useGraphQL(query)`連結，並傳入GraphQL以傳送至AEM。

   此掛接使用簡單的`fetch`模組來發出HTTPPOSTGraphQL請求，但其他模組（如[Apollo GraphQL客戶端](https://www.apollographql.com/docs/react/)）也可以同樣使用。

1. 在IDE中開啟`src/components/Adventures.js`，該IDE負責主視圖的歷險清單，並查看`useGraphQL`掛接的調用。

   此代碼將預設`query`設定為`allAdventuresQuery`，如此檔案中向下定義。

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   .......每當`query`變數變更，就會叫用`useGraphQL`鈎點，進而對AEM執行GraphQL查詢，將JSON傳回`data`變數，然後用於轉譯歷險清單。

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   `allAdventuresQuery`是檔案中定義的常數GraphQL查詢，可查詢所有探險內容片段（不含任何篩選），並只傳回需要呈現首頁檢視的資料點。

   ```javascript
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

1. 開啟`src/components/AdventureDetail.js`, React元件負責顯示冒險詳細資訊體驗。 此檢視會以特定內容片段的JCR路徑作為唯一ID，並轉譯提供的詳細資料。

   與`Adventures.js`類似，自訂`useGraphQL` React Hook會重新用於對AEM執行該GraphQL查詢。

   內容片段的路徑是從元件的`props`頂端所收集，用以指定要查詢的內容片段。

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   ...且使用`adventureDetailQuery(..)`函式構造GraphQL參數化查詢，並傳遞至`useGraphQL(query)`，該會對AEM執行GraphQL查詢，並將結果傳回至`data`變數。

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   `adventureDetailQuery(..)`函式只會包住篩選GraphQL查詢，該查詢使用AEM `<modelName>ByPath`語法來查詢以其JCR路徑識別的單一內容片段，並傳回轉譯探險程式詳細資料所需的所有指定資料點。

   ```javascript
   function adventureDetailQuery(_path) {
   return `{
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
         }
       }
   }
   `;
   }
   ```

## 建立參數化的GraphQL查詢

接下來，我們將修改React應用程式，以執行參數化的篩選GraphQL查詢，這些查詢會依歷險活動來限制首頁檢視。

1. 在IDE中，開啟檔案：`src/components/Adventures.js`。 此檔案代表首頁體驗的歷險元件，可查詢並顯示歷險元卡。
1. Inspect函式`filterQuery(activity)`（未使用），但已準備好制定GraphQL查詢，依`activity`篩選歷險。

   請注意，參數`activity`會作為`adventureActivity`欄位上`filter`的一部分插入到GraphQL查詢中，要求該欄位的值與參數的值相符。

   ```javascript
   function filterQuery(activity) {
       return `
           {
           adventures (filter: {
               adventureActivity: {
               _expressions: [
                   {
                   value: "${activity}"
                   }
                 ]
               }
           }){
               items {
               _path
               adventureTitle
               adventurePrice
               adventureTripLength
               adventurePrimaryImage {
               ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
               }
               }
             }
         }
       }
       `;
   }
   ```

1. 更新React Adventures元件的`return`陳述式，以新增叫用新參數化`filterQuery(activity)`的按鈕，以提供要列出的歷險。

   ```javascript
   function Adventures() {
       ...
       return (
           <div className="adventures">
   
           {/* Add these three new buttons that set the GraphQL query accordingly */}
   
           {/* The first button uses the default `allAdventuresQuery` */}
           <button onClick={() => setQuery(allAdventuresQuery)}>All</button>
   
           {/* The 2nd and 3rd button use the `filterQuery(..)` to filter by activity */}
           <button onClick={() => setQuery(filterQuery('Camping'))}>Camping</button>
           <button onClick={() => setQuery(filterQuery('Surfing'))}>Surfing</button>
   
           <ul className="adventure-items">
           ...
       )
   }
   ```

1. 儲存變更並在網頁瀏覽器中重新載入React應用程式。 三個新按鈕會出現在頂端，然後按一下它們就會自動透過相符的活動重新查詢AEM的冒險內容片段。

   ![依活動篩選歷險記](./assets/graphql-and-external-app/filter-by-activity.png)

1. 嘗試為活動添加更多篩選按鈕：`Rock Climbing`、`Cycling`和`Skiing`

## 處理GraphQL錯誤

GraphQL是強類型的，因此，如果查詢無效，可能會傳回有用的錯誤訊息。 接下來，我們模擬錯誤的查詢，以查看傳回的錯誤訊息。

1. 重新開啟檔案`src/api/useGraphQL.js`。 Inspect下列程式碼片段，查看錯誤處理：

   ```javascript
   //useGraphQL.js
   .then(({data, errors}) => {
           //If there are errors in the response set the error message
           if(errors) {
               setErrors(mapErrors(errors));
           }
           //Otherwise if data in the response set the data as the results
           if(data) {
               setData(data);
           }
       })
       .catch((error) => {
           setErrors(error);
       });
   ```

   檢查響應以查看其是否包含`errors`對象。 如果GraphQL查詢有問題，例如根據架構的未定義欄位，則AEM會傳送`errors`物件。 如果沒有`errors`物件，則會設定並傳回`data`。

   `window.fetch`包含`.catch`語句，用於&#x200B;*catch*&#x200B;任何常見錯誤，如無效的HTTP請求，或無法與伺服器進行連接。

1. 開啟檔案`src/components/Adventures.js`。
1. 修改`allAdventuresQuery`以包含無效屬性`adventurePetPolicy`:

   ```javascript
   /**
    * Query for all Adventures
    * adventurePetPolicy has been added beneath items
   */
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           adventurePetPolicy
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
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

   我們知道`adventurePetPolicy`不屬於Adventure模型，因此應該會觸發錯誤。

1. 儲存變更並返回瀏覽器。 您應會看到如下的錯誤訊息：

   ![屬性錯誤無效](assets/graphql-and-external-app/invalidProperty.png)

   GraphQL API會偵測`AdventureModel`中未定義`adventurePetPolicy`，並傳回適當的錯誤訊息。

1. Inspect來自AEM的回應，使用瀏覽器的開發人員工具來查看`errors` JSON物件：

   ![錯誤JSON物件](assets/graphql-and-external-app/error-json-response.png)

   `errors`對象是詳細的，包括有關錯誤查詢的位置和錯誤分類的資訊。

1. 返回`Adventures.js`並還原查詢變更，將應用程式傳回至其正確狀態。

## 恭喜！{#congratulations}

恭喜！ 您已成功探索範例WKND GraphQL React應用程式的程式碼，並將其更新為使用參數化、篩選GraphQL查詢，以依活動列出歷險！ 您也有機會探索一些基本的錯誤處理。

## 後續步驟 {#next-steps}

在下一章中， [使用片段參考的進階資料模型](./fragment-references.md)您將了解如何使用片段參考功能來建立兩個不同內容片段之間的關係。 您也將學習如何修改GraphQL查詢以包含參考模型的欄位。
