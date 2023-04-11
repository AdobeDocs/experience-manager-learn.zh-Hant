---
title: 建立React應用程式，可使用GraphQL API查詢AEM -AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 建立從AEM GraphQL API擷取內容/資料的React應用程式，並了解如何使用AEM Headless JS SDK。
version: Cloud Service
mini-toc-levels: 1
kt: 6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1182'
ht-degree: 2%

---


# 建立使用AEM GraphQL API的React應用程式

在本章中，您將探索AEM GraphQL API如何在外部應用程式中促進體驗。

簡單的React應用程式可用於查詢和顯示 **團隊** 和 **人員** AEM GraphQL API公開的內容。 React的使用在很大程度上不重要，任何平台的任何框架都可編寫耗用的外部應用程式。

## 必備條件

假設已完成本多部分教學課程前幾部分概述的步驟，或 [tutorial-solution-content-zip](assets/explore-graphql-api/tutorial-solution-content.zip) 安裝在AEMas a Cloud Service製作和發佈服務上。

_本章中的IDE螢幕截圖來自 [Visual Studio代碼](https://code.visualstudio.com/)_

必須安裝以下軟體：

- [Node.js v18](https://nodejs.org/)
- [Visual Studio代碼](https://code.visualstudio.com/)

## 目標

了解如何：

- 下載並啟動範例React應用程式
- 使用查詢AEM GraphQL端點 [AEM無頭JS SDK](https://github.com/adobe/aem-headless-client-js)
- 查詢AEM以獲取團隊及其引用的成員的清單
- 查詢AEM成員的詳細資訊

## 取得範例React應用程式

在本章中，已實作簡化的範例React應用程式，其中包含與AEM GraphQL API互動所需的程式碼，並顯示從這些程式碼取得的團隊和人員資料。

Github.com上提供範例React應用程式原始碼，網址為 <https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>

若要取得React應用程式：

1. 從複製範例WKND GraphQL React應用程式 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導覽至 `basic-tutorial` 資料夾，並在IDE中開啟它。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![VSCode中的React應用程式](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新 `.env.development` 連線至AEMas a Cloud Service發佈服務。

   - 設定 `REACT_APP_HOST_URI`的值是您的AEMas a Cloud Service的發佈URL(例如 `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`)和 `REACT_APP_AUTH_METHOD`&#39;s值為 `none`
   >[!NOTE]
   >
   > 請確定您已發佈專案設定、內容片段模型、製作內容片段、GraphQL端點，以及先前步驟的持續查詢。
   >
   > 如果您在本機AEM Author SDK上執行上述步驟，可以指向 `http://localhost:4502` 和 `REACT_APP_AUTH_METHOD`&#39;s值為 `basic`.


1. 從命令列，前往 `aem-guides-wknd-graphql/basic-tutorial` 資料夾

1. 啟動React應用程式

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React應用程式以開發模式啟動，於 [http://localhost:3000/](http://localhost:3000/). 教學課程中對React應用程式所做的變更會立即反映。

![部分實作的React應用程式](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   此React應用程式已部分實作。 請依照本教學課程中的步驟完成實作。 需要實作的JavaScript檔案有下列註解，請務必使用本教學課程中指定的程式碼，在這些檔案中新增/更新程式碼。
>
>
> /*********************************
>
>  // TODO :請依照AEM Headless教學課程中的步驟來實作
>
>  //*********************************

## React應用程式解剖

範例React應用程式包含三個主要部分：

1. 此 `src/api` 資料夾包含用於對AEM進行GraphQL查詢的檔案。
   - `src/api/aemHeadlessClient.js` 初始化並導出用於與AEM通信的AEM Headless客戶端
   - `src/api/usePersistedQueries.js` 實作 [自訂React鈎點](https://react.dev/docs/hooks-custom.html) 將資料從AEM GraphQL傳回至 `Teams.js` 和 `Person.js` 檢視元件。

1. 此 `src/components/Teams.js` 檔案使用清單查詢顯示團隊及其成員的清單。
1. 此 `src/components/Person.js` 檔案使用參數化的單一結果查詢，顯示單個人員的詳細資訊。

## 檢閱AEMHeadless物件

檢閱 `aemHeadlessClient.js` 檔案，以便建立 `AEMHeadless` 用來與AEM通訊的物件。

1. 開啟 `src/api/aemHeadlessClient.js`.

1. 查看行1-40:

   - 匯入 `AEMHeadless` 來自 [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js)，第11行。

   - 根據 `.env.development`、行14-22和，箭頭函式表達式 `setAuthorization`,31-40號線。

   - 此 `serviceUrl` 包含的設定 [開發代理](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app#proxy-api-requests) 配置，第27行。

1. 第42-49行是最重要的行，因為它們可將 `AEMHeadless` 用戶端，並匯出以在React應用程式中使用。

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## 實作以執行AEM GraphQL持續查詢

實作一般 `fetchPersistedQuery(..)` 函式以執行AEM GraphQL持續查詢會開啟 `usePersistedQueries.js` 檔案。 此 `fetchPersistedQuery(..)` 函式使用 `aemHeadlessClient` 物件 `runPersistedQuery()` 函式，以非同步方式執行查詢、以Promise為基礎的行為。

稍後，自訂React `useEffect` 連結呼叫此函式以從AEM擷取特定資料。

1. 在 `src/api/usePersistedQueries.js` **更新** `fetchPersistedQuery(..)`，第35行，下面有代碼。

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

## 實作團隊功能

接下來，建置功能，在React應用程式的主檢視上顯示團隊及其成員。 此功能需要：

- 新 [自訂React useEffect鈎點](https://react.dev/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` 調用 `my-project/all-teams` 持續查詢，傳回AEM中的團隊內容片段清單。
- React元件位於 `src/components/Teams.js` 調用新的自定義React `useEffect` 連結，並轉譯團隊資料。

完成後，應用程式的主要檢視會填入AEM的團隊資料。

![團隊檢視](./assets/graphql-and-external-app/react-app__teams-view.png)

### 步驟

1. 開啟 `src/api/usePersistedQueries.js`.

1. 找出函式 `useAllTeams()`

1. 建立 `useEffect` 調用持續查詢的掛接 `my-project/all-teams` via `fetchPersistedQuery(..)`，新增下列程式碼。 此連結也只會傳回AEM GraphQL回應中的相關資料，位置為 `data?.teamList?.items`，可讓React檢視元件不受父JSON結構限制。

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

1. 在 `Teams` React元件，使用從AEM擷取團隊清單 `useAllTeams()` 鈎。

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

1. 最後，呈現團隊資料。 從GraphQL查詢傳回的每個團隊，都會使用提供的 `Team` React子元件。

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


## 實作人員功能

使用 [團隊功能](#implement-teams-functionality) 完成後，我們實施功能來處理團隊成員或人員詳細資訊的顯示。

此功能需要：

- 新 [自訂React useEffect鈎點](https://react.dev/docs/hooks-custom.html) in `src/api/usePersistedQueries.js` 調用參數化 `my-project/person-by-name` 持續查詢，並傳回單一人員記錄。

- React元件位於 `src/components/Person.js` 將人員的全名用作查詢參數，調用新的自定義React `useEffect` 掛接，並轉譯人員資料。

完成後，在「團隊」視圖中選擇人員姓名，將呈現人員視圖。

![人員](./assets/graphql-and-external-app/react-app__person-view.png)

1. 開啟 `src/api/usePersistedQueries.js`.

1. 找出函式 `usePersonByName(fullName)`

1. 建立 `useEffect` 調用持續查詢的掛接 `my-project/all-teams` via `fetchPersistedQuery(..)`，新增下列程式碼。 此連結也只會傳回AEM GraphQL回應中的相關資料，位置為 `data?.teamList?.items`，可讓React檢視元件不受父JSON結構限制。

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
1. 在 `Person` React元件，解析 `fullName` 路由參數，並使用 `usePersonByName(fullName)` 鈎。

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

檢閱應用程式 [http://localhost:3000/](http://localhost:3000/) 按一下 _成員_ 連結。 您也可以在AEM中新增內容片段，將更多團隊和/或成員新增至Team Alpha。

## 在兜帽下

開啟瀏覽器的 **開發人員工具** > **網路** 和 _篩選_ for `all-teams` 請求。 注意GraphQL API請求 `/graphql/execute.json/my-project/all-teams` 針對 `http://localhost:3000` 和 **NOT** 值 `REACT_APP_HOST_URI` (例如， <https://publish-p123-e456.adobeaemcloud.com>)。 會根據React應用程式的網域提出請求，因為 [代理設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 啟用 `http-proxy-middleware` 模組。


![GraphQL API透過Proxy要求](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


檢閱主要 `../setupProxy.js` 檔案和內 `../proxy/setupProxy.auth.**.js` 檔案注意如何 `/content` 和 `/graphql` 路徑會經過代理，並指出這不是靜態資產。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

使用本機代理不是生產部署的合適選項，有關詳細資訊，請訪問 _生產部署_ 區段。

## 恭喜！{#congratulations}

恭喜！您已成功建立React應用程式，以使用和顯示AEM GraphQL API中的資料，做為基本教學課程的一部分！
