---
title: 無AEM頭API和反應 — 無AEM頭第一教程
description: 瞭解如何從GraphQLAPI中檢索內容片AEM段資料並在React應用中顯示它。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---


# 無AEM頭API和反應

歡迎參加本教程章，我們將探討如何配置React應用程式以使用Headless SDK與Adobe Experience Manager(AEM)Headless APIAEM連接。 我們將介紹從GraphQLAPI中檢索內AEM容片段資料並在React應用中顯示。

無AEM頭API允許從任AEM何客戶端應用訪問內容。 我們將指導您配置您的React應用程式，使AEM用Headless SDK連AEM接到Headless API。 此安裝程式在您的React應用和之間建立可重用的通AEM信通道。

接下來，我們將使用無AEM頭SDK從GraphQLAPI中檢索內容AEM片段資料。 中的內容片AEM段提供結構化內容管理。 利用無AEM頭SDK，您可以使用GraphQL輕鬆查詢和獲取內容片段資料。

一旦我們獲得了內容片段資料，我們將將其整合到您的React應用中。 您將學習如何以吸引人的方式格式化和顯示資料。 我們將介紹處理和呈現Reacte元件中內容片段資料的最佳做法，確保與應用的UI無縫整合。

在整個教程中，我們將提供說明、代碼示例和實用提示。 最後，您將能夠配置您的React應用程式以連接到AEMHeadless API，使用Headless SDK檢索內容碎片資料AEM，並在您的React應用程式中無縫顯示資料。 開始吧！


## 克隆React應用

1. 從克隆應用 [吉圖布](https://github.com/lamontacrook/headless-first/tree/main) 命令行上執行以下命令。

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. 更改為 `headless-first` 目錄，並安裝依賴項。

   ```
   $ cd headless-first
   $ npm ci
   ```

## 配置React應用

1. 建立名為 `.env` 在項目的根部。 在 `.env` 設定以下值：

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. 可以在雲管理器中檢索開發人員令牌。 登錄到 [Adobe雲管理器](https://experience.adobe.com/)。 按一下 __Experience Manager>雲管理器__。 選擇相應的「程式」(Program)，然後按一下「環境」(Environment)旁邊的省略號。

   ![開AEM發者控制台](./assets/2/developer-console.png)

   1. 按一下 __整合__ 頁籤
   1. 按一下 __「本地令牌」頁籤和獲取本地開發令牌__ 按鈕
   1. 複製訪問令牌，從開啟報價後開始，直到關閉報價前為止。
   1. 將複製的標籤貼上為 `REACT_APP_TOKEN` 的 `.env` 的子菜單。
   1. 現在，我們通過執行 `npm ci` 命令行上。
   1. 現在啟動React應用並執行 `npm run start` 命令行上。
   1. 在 [./src/utis](https://github.com/lamontacrook/headless-first/tree/main/src/utils) 名為 `context.js`  包括用於設定 `.env` 檔案到應用的上下文。

## 運行React應用

1. 通過執行React應用程式啟動 `npm run start` 命令行上。

   ```
   $ npm run start
   ```

   React應用將啟動並開啟瀏覽器窗口以 `http://localhost:3000`。 將在瀏覽器中自動重新載入對React應用的更改。

## 連接到無AEM頭API

1. 要將React應用程式連AEM接到as a Cloud Service，讓我們添加一些內容 `App.js`。 在 `React` 導入，添加 `useContext`。

   ```javascript
   import React, {useContext} from 'react';
   ```

   導入 `AppContext` 從 `context.js` 的子菜單。

   ```javascript
   import { AppContext } from './utils/context';
   ```

   現在，在應用程式碼中定義上下文變數。

   ```javascript
   const context = useContext(AppContext);
   ```

   最後，將返回代碼 `<AppContext.Provider> ... </AppContext.Provider>`。

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   為參考， `App.js` 應該是這樣。

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. 導入 `AEMHeadless` SDK。 此SDK是應用程式用於與無頭API交互的AEM幫助程式庫。

   將此導入語句添加到 `home.js`。

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   添加以下內容 `{ useContext, useEffect, useState }` 到` React` 導入語句。

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   導入 `AppContext`。

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   在 `Home` 元件，獲取 `context` 變數 `AppContext`。

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. 初始化AEM無頭SDK  `useEffect()`，因AEM為當  `context` 變數更改。

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > 有 `context.js` 檔案 `/utils` 就是從 `.env` 的子菜單。 為參考， `context.url` 是as a Cloud Service環境的AEMURL。 的 `context.endpoint` 是上一課中建立的端點的完整路徑。 最後， `context.token` 是開發者令牌。


1. 建立React狀態，以顯示來自無頭SDKAEM的內容。

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. 將應用連接到AEM。 使用上一課中建立的永續查詢。 讓我們在 `useEffect` 初始AEM化無頭SDK後。 使 `useEffect` 取決於  `context` 變數，如下所示。


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. 開啟開發人員工具的「網路」視圖以查看GraphQL請求。

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Chrome開發工具](./assets/2/dev-tools.png)

   Headless AEM SDK對GraphQL的請求進行編碼並添加所提供的參數。 您可以在瀏覽器中開啟請求。

   >[!NOTE]
   >
   > 由於請求將發往作者環境，因此您必須登錄到同一瀏覽器的另一個頁籤中的環境。


## 呈現內容片段內容

1. 顯示應用中的內容片段。 返回 `<div>` 劇名。

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   您應看到螢幕上顯示的預告的標題欄位。

1. 最後一步是將預告添加到頁面。 在封裝中包括反應預激器元件。 首先，我們包括導入。 在 `home.js` 檔案，添加行：

   `import Teaser from '../../components/teaser/teaser';`

   更新返回語句：

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   現在，您應看到片段中包含內容的預告。


## 後續步驟

恭喜！您已成功更新React應用程式，使AEM用Headless SDK與Headless APIAEM整合！

接下來，我們建立一個更複雜的「影像清單」元件，該元件從中動態呈現引用的內容AEM片段。

[下一章：生成映像清單元件](./3-complex-components.md)