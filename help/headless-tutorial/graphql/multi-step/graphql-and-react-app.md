---
title: 使用GraphQL API構建查詢AEM的反應應用 — 無頭AEM入門 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 生成從GraphQL API提取內容/資料AEM的反應應用，另請參閱AEM如何使用無頭JS SDK。
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 294ad688b17a5fc9559fea39fc99ebf5e95cad39
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 1%

---


# 構建利用GraphQL API的AEMReact應用

本章介紹AEMGraphQL API如何在外部應用程式中推動體驗。

簡單的React應用用於查詢和顯示 **團隊** 和 **人員** 由GraphQL API公開AEM的內容。 React的使用在很大程度上並不重要，任何平台的任何框架都可以編寫耗用的外部應用程式。

## 必備條件

假定本多部分教程的前幾部分中概述的步驟已完成，或 [教程 — 解決方案 — content.zip](assets/explore-graphql-api/tutorial-solution-content.zip) 安裝在您的AEMas a Cloud Service作者和發佈服務上。

_本章中的IDE螢幕截圖來自 [Visual Studio代碼](https://code.visualstudio.com/)_

必須安裝以下軟體：

- [Node.js v12+](https://nodejs.org/en/)
- [npm 6.4+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- [Visual Studio代碼](https://code.visualstudio.com/)

## 目標

瞭解如何：

- 下載並啟動示例React應用
- 使AEM用的查詢GraphQL端點 [無AEM頭JS SDK](https://github.com/adobe/aem-headless-client-js)
- 查AEM詢團隊清單及其引用的成員
- 查AEM詢團隊成員的詳細資訊

## 獲取示例React應用

在本章中，將實現一個簡單的React應用程式，該應用程式具有與AEMGraphQL API交互所需的代碼，並顯示從這些代碼獲取的團隊和個人資料。

Github.com上提供的示例React應用程式原始碼：

- https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial

要獲取React應用：

1. 從克隆示例WKND GraphQL React應用 [吉圖布網](https://github.com/adobe/aem-guides-wknd-graphql)。

   ```shell
   $ cd ~/Code
   $ git clone --branch git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導航到 `basic-tutorial` 資料夾，然後在IDE中開啟它。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![在VSCode中反應應用](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新 `.env.development` 連接到AEMas a Cloud Service發佈服務。

   - 設定 `REACT_APP_HOST_URI`值：作為AEMas a Cloud Service的發佈URL(例如 `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) `REACT_APP_AUTH_METHOD`&#39;s值到 `none`
   >[!NOTE]
   >
   > 確保已發佈項目配置、內容片段模型、創作的內容片段、GraphQL終結點和前面步驟中的永續查詢。 或者，如果您在本地SDK上執AEM行了上述步驟，則可以指向 `http://localhost:4502` 和 `REACT_APP_AUTH_METHOD`&#39;s值到 `basic`。


1. 從命令行，轉到 `aem-guides-wknd-graphql/basic-tutorial` 資料夾
1. 啟動React應用

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React應用程式在開發模式下啟動， [http://localhost:3000/](http://localhost:3000/)。 在整個教程中對React應用所做的更改將自動反映。

## React應用的解剖

示例React應用包含三個主要部分：

1. `src/api` 包含用於對GraphQL查詢的文AEM件。
   - `src/api/aemHeadlessClient.js` 初始化並導AEM出用於與通信的無頭客AEM戶端
   - `src/api/usePersistedQueries.js` 實現 [自定義反應掛接](https://reactjs.org/docs/hooks-custom.html) 將數AEM據從GraphQL返回 `Teams.js` 和 `Person.js` 查看元件。

1. `src/components/Teams.js` 使用清單查詢顯示團隊及其成員的清單。
1. `src/components/Person.js` 使用參數化的單結果查詢顯示單個人員的詳細資訊。

## 建立AEMHeadless客戶端

審閱 `aemHeadlessClient.js` 如何初始化用AEM於與通信的無頭客AEM戶端。

1. 開啟 `src/api/aemHeadlessClient.js`.
1. 行1-40，導入 `AEMHeadless` 根據SDK中定義的變數進行配置授權 `.env.development`，並設定 `serviceUrl` 包括 [開發代理](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) 配置。
1. 第42-49行最重要，因為它們實例化了AEMHeadless客戶端並將其導出以在整個React應用程式中使用。

   ```javascript
   // Initialize the AEM Headless Client and export it for other files to use
   const aemHeadlessClient = new AEMHeadless({
     serviceURL: serviceURL,
     endpoint: REACT_APP_GRAPHQL_ENDPOINT,
     auth: setAuthorization(),
   });
   
   export default aemHeadlessClient;
   ```

## 連接到AEMGraphQL終結點

開啟 `usePersistedQueries.js` 實現通用 `fetchPersistedQuery(..)` 函式以連接到AEMGraphQL終結點。 `fetchPersistedQuery(..)` 調用 `aemHeadlessClient` 導出自 `aemHeadlessClient.js`。

稍後，自定義React useEffect掛接調用此函式以從中檢索特定AEM資料。

1. 在 `src/api/usePersistedQueries.js` 更新 `fetchPersistedQuery(..)` 代碼。

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## 實施團隊功能

接下來，構建功能，在React應用的主視圖中顯示團隊及其成員。 此功能需要：

- 新 [自定義React useEffect掛接](https://reactjs.org/docs/hooks-custom.html) 在 `src/api/usePersistedQueries.js` 調用 `my-project/all-teams` 永續查詢，返回中的團隊內容片段列AEM表。
- A React元件位於 `src/components/Teams.js` 調用新的自定義React useEffect掛接並呈現團隊資料。

完成後，應用的主視圖將填充來自的團隊數AEM據。

![團隊視圖](./assets/graphql-and-external-app/react-app__teams-view.png)

### 步驟

1. 開啟 `src/api/usePersistedQueries.js`.
1. 定位函式 `useAllTeams()`
1. 建立 `useEffect` 調用保留查詢的掛接 `my-project/all-teams` 通過 `fetchPersistedQuery(..)`，添加以下代碼。 此掛接僅返回GraphQL響應中AEM的相關資料 `data?.teamList?.items`，允許React視圖元件與父JSON結構不可知。

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. 開啟 `src/components/Teams.js`
1. 在 `Teams` 響應元件，使用 `useAllTeams()` 鈎子。

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```

1. 執行基於視圖的資料驗證，顯示錯誤消息或根據返回的資料載入指示符。

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. 最後，呈現團隊資料。 從GraphQL查詢返回的每個組都使用提供的 `Team` 反應子元件。

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## 實施人員功能

使用 [團隊功能](#implement-teams-functionality) 完成後，讓我們實施處理團隊成員或人員詳細資訊上顯示的功能。

此功能需要：

- 新 [自定義React useEffect掛接](https://reactjs.org/docs/hooks-custom.html) 在 `src/api/usePersistedQueries.js` 調用參數化的 `my-project/person-by-name` 永續查詢，並返回單個人員記錄。

- A React元件位於 `src/components/Person.js` 將人員的全名用作查詢參數，調用新的自定義React useEffect掛接，並呈現人員資料。

完成後，在「團隊」視圖中選擇人員姓名，將呈現人員視圖。

![人員](./assets/graphql-and-external-app/react-app__person-view.png)

1. 開啟 `src/api/usePersistedQueries.js`.
1. 定位函式 `usePersonByName(fullName)`
1. 建立 `useEffect` 調用保留查詢的掛接 `my-project/all-teams` 通過 `fetchPersistedQuery(..)`，添加以下代碼。 此掛接僅返回GraphQL響應中AEM的相關資料 `data?.teamList?.items`，允許React視圖元件與父JSON結構不可知。

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. 開啟 `src/components/Person.js`
1. 在 `Person` 反應元件，分析 `fullName` route參數，並使用 `usePersonByName(fullName)` 鈎子。

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. 執行基於視圖的資料驗證，顯示錯誤消息或根據返回的資料載入指示符。

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. 最後，呈現人員資料。

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```

## 嘗試應用

查看應用 [http://localhost:3000/](http://localhost:3000/) 按一下 _成員_ 連結。 此外，您可以將更多團隊和/或成員添加到團隊Alpha中。

## 《煙帽下》

如果開啟瀏覽器 **開發人員工具** > **網路** 和 *篩選* 為 `all-teams` 請求，您將注意到GraphQL API請求 `/graphql/execute.json/my-project/all-teams` 是 `http://localhost:3000` 和 **不** 與 `REACT_APP_HOST_URI`(例如https://publish-p123-e456.adobeaemcloud.com)，這是因為 [代理設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用 `http-proxy-middleware` 中。


![GraphQL API請求（通過代理）](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


查看主要 `../setupProxy.js` 檔案和 `../proxy/setupProxy.auth.**.js` 檔案注意如何 `/content` 和 `/graphql` 路徑被代理，並指示它不是靜態資產。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

但是，這不適合 **生產部署** 但在開發過程中效果良好，更多詳情請參閱： [_部署_](../deployment/spa.md) 的子菜單。

## 恭喜！{#congratulations}

恭喜！ 您已成功建立React應用程式，將GraphQL API中的資料AEM用作基本教程的一部分！
