---
title: 使用外部應用程式的GraphQL查詢AEM — 開始使用AEM無周邊功能 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 探索範例WKND GraphQL React應用程式的AEM GraphQL API。 了解此外部應用程式如何對AEM進行GraphQL呼叫，以強化其使用體驗。 了解如何執行基本錯誤處理。
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1382'
ht-degree: 0%

---

# 使用外部應用程式的GraphQL查詢AEM

在本章中，我們將探討如何使用AEM GraphQL API來促進外部應用程式中的體驗。

本教學課程使用簡單的React應用程式來查詢及顯示AEM GraphQL API公開的冒險內容。 React的使用在很大程度上不重要，任何平台的任何框架都可編寫耗用的外部應用程式。

## 必備條件

本教學課程分為多部分，假設已完成前幾部分所述的步驟。

_本章中的IDE螢幕截圖來自 [Visual Studio代碼](https://code.visualstudio.com/)_

（可選）安裝瀏覽器擴充功能，例如 [GraphQL網路檢查器](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) ，以便查看有關GraphQL查詢的更多詳細資訊。

## 目標

在本章中，我們將學習如何：

* 開始並了解範例React應用程式的功能
* 探索如何從外部應用程式呼叫AEM GraphQL端點
* 定義GraphQL查詢，以依活動篩選歷險內容片段清單
* 更新React應用程式，提供控制項以透過GraphQL篩選，該清單為依活動的歷險清單

## 啟動React應用程式

由於本章的重點是開發客戶端，以使用GraphQL上的內容片段，本示例 [必須下載並設定WKND GraphQL React應用程式原始碼](../quick-setup/local-sdk.md) 在本地電腦上。

如需啟動React應用程式的詳細說明，請參閱 [快速設定](../quick-setup/local-sdk.md) 章節，但可遵循節略指示：

1. 如果尚未複製，請從複製範例WKND GraphQL React應用程式 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在IDE中開啟WKND GraphQL React應用程式

   ![VSCode中的React應用程式](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 從命令列導覽至 `react-app` 資料夾
1. 從項目根( `react-app` 資料夾)

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. 查看應用程式的位置： [http://localhost:3000/](http://localhost:3000/). 範例React應用程式有兩個主要部分：

   * 家庭體驗可作為WKND Adventures的索引，方法是查詢 __冒險__ AEM中使用GraphQL的內容片段。 在本章中，我們將修改此檢視，以支援依活動篩選歷險。

      ![WKND GraphQL React應用程式 — 首頁體驗](./assets/graphql-and-external-app/react-home-view.png)

   * 探險詳細資訊體驗，使用GraphQL來查詢特定 __冒險__ 內容片段，並顯示更多資料點。

      ![WKND GraphQL React應用程式 — 詳細體驗](./assets/graphql-and-external-app/react-details-view.png)

1. 使用瀏覽器的開發工具和瀏覽器擴充功能，例如 [GraphQL網路檢查器](https://chrome.google.com/webstore/detail/graphql-network-inspector/ndlbedplllcgconngcnfmkadhokfaaln) 檢查傳送至AEM的GraphQL查詢及其JSON回應。 此方法可用來監控GraphQL請求和回應，以確保正確制定，並如預期般回應。

   ![AdventureList的原始查詢](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *從React應用程式傳送至AEM的GraphQL查詢*

   ![GraphQL JSON回應](assets/graphql-and-external-app/graphql-json-response.png)

   *從AEM回應至React應用程式的JSON回應*

   查詢和響應應與GraphiQL IDE中顯示的內容相匹配。

   >[!NOTE]
   >
   > 在開發期間， React應用程式會透過Webpack開發伺服器將HTTP要求代理傳送至AEM。 React應用程式向  `http://localhost:3000` 將其代理至執行中的AEM製作服務 `http://localhost:4502`. 檢閱檔案 `src/setupProxy.js` 和 `env.development` 以取得詳細資訊。
   >
   > 在非開發情況下， React應用程式會直接設定為向AEM提出要求。

## 探索應用程式的GraphQL程式碼

1. 在IDE中開啟檔案 `src/api/useGraphQL.js`.

   這是 [反應效果鈎](https://reactjs.org/docs/hooks-overview.html#effect-hook) 會監聽應用程式的變更 `query`，且變更時會向AEM GraphQL端點提出HTTPPOST要求，並傳回JSON回應至應用程式。

   每當React應用程式需要進行GraphQL查詢時，就會叫用此自訂 `useGraphQL(query)` 連結，傳入GraphQL以傳送至AEM。

   此鈎子使用 `fetch` 模組來發出HTTPPOSTGraphQL請求，但其他模組，例如 [Apollo GraphQL客戶端](https://www.apollographql.com/docs/react/) 可以類似地使用。

1. 開啟 `src/components/Adventures.js` 在IDE中，該IDE負責主視圖的歷險清單，並查看調用 `useGraphQL` 鈎。

   此程式碼會設定預設 `query` 成為 `allAdventuresQuery` 在此檔案中向下定義。

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   ...... `query` 變數變更， `useGraphQL` 會叫用連結，進而對AEM執行GraphQL查詢，將JSON傳回至 `data` 變數，此變數隨後用於轉譯歷險清單。

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   此 `allAdventuresQuery` 是檔案中定義的常數GraphQL查詢，可查詢所有探險內容片段（不含任何篩選），並只傳回需要呈現主檢視的資料點。

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

1. 開啟 `src/components/AdventureDetail.js`，此元件負責顯示冒險詳細資訊體驗。 此檢視會以特定內容片段的JCR路徑作為唯一ID，並轉譯提供的詳細資料。

   類似於 `Adventures.js`，自訂 `useGraphQL` React Hook可重新用於對AEM執行該GraphQL查詢。

   內容片段的路徑是從元件的 `props` 頂端用來指定要查詢的內容片段。

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   ...而GraphQL參數化查詢是使用 `adventureDetailQuery(..)` 函式，並傳遞至 `useGraphQL(query)` 會對AEM執行GraphQL查詢，並將結果傳回至 `data` 變數。

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   此 `adventureDetailQuery(..)` 函式僅包含篩選GraphQL查詢，該查詢使用AEM `<modelName>ByPath` 語法以查詢以其JCR路徑識別的單一內容片段，並傳回轉譯冒險的詳細資料所需的所有指定資料點。

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

1. 在IDE中，開啟檔案： `src/components/Adventures.js`. 此檔案代表首頁體驗的歷險元件，可查詢並顯示歷險元卡。
1. Inspect函式 `filterQuery(activity)`，此功能未使用，但已準備好制定GraphQL查詢，以篩選歷險 `activity`.

   請注意該參數 `activity` 會插入GraphQL查詢中，作為 `filter` 在 `adventureActivity` 欄位，要求該欄位的值與參數的值相符。

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

1. 更新React Adventures元件的 `return` 語句，添加調用新參數化參數的按鈕 `filterQuery(activity)` 提供登記的冒險。

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

1. 嘗試為活動添加更多篩選按鈕： `Rock Climbing`, `Cycling` 和 `Skiing`

## 處理GraphQL錯誤

GraphQL是強類型的，因此，如果查詢無效，可能會傳回有用的錯誤訊息。 接下來，我們模擬錯誤的查詢，以查看傳回的錯誤訊息。

1. 重新開啟檔案 `src/api/useGraphQL.js`. Inspect下列程式碼片段，查看錯誤處理：

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

   檢查該回應，以查看其是否包含 `errors` 物件。 此 `errors` 如果GraphQL查詢有問題（例如根據架構未定義的欄位）,AEM將會傳送物件。 如果沒有 `errors` 物件 `data` 已設定並傳回。

   此 `window.fetch` 包括 `.catch` 語句 *cat* 任何常見錯誤，例如無效的HTTP請求，或無法與伺服器連線。

1. 開啟檔案 `src/components/Adventures.js`.
1. 修改 `allAdventuresQuery` 包含無效屬性 `adventurePetPolicy`:

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

   我們知道 `adventurePetPolicy` 不屬於Adventure模型，因此應會觸發錯誤。

1. 儲存變更並返回瀏覽器。 您應會看到如下的錯誤訊息：

   ![屬性錯誤無效](assets/graphql-and-external-app/invalidProperty.png)

   GraphQL API會偵測 `adventurePetPolicy` 未定義 `AdventureModel` 並傳回適當的錯誤訊息。

1. Inspect AEM的回應，使用瀏覽器的開發人員工具來檢視 `errors` JSON物件：

   ![錯誤JSON物件](assets/graphql-and-external-app/error-json-response.png)

   此 `errors` 對象為詳細，包括有關錯誤查詢的位置和錯誤分類的資訊。

1. 返回 `Adventures.js` 並回複查詢變更，將應用程式還原為正確狀態。

## 恭喜！{#congratulations}

恭喜！ 您已成功探索範例WKND GraphQL React應用程式的程式碼，並將其更新為使用參數化、篩選GraphQL查詢，以依活動列出歷險！ 您也有機會探索一些基本的錯誤處理。

## 後續步驟 {#next-steps}

在下一章中， [使用片段參考的進階資料模型](./fragment-references.md) 您將了解如何使用片段參考功能來建立兩個不同內容片段之間的關係。 您也將學習如何修改GraphQL查詢以包含參考模型的欄位。
