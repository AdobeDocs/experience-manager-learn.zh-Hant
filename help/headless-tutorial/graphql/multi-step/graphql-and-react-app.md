---
title: 使用GraphQL API建置React應用程式以查詢AEM - AEM快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 建置從AEM GraphQL API擷取內容/資料的React應用程式，也瞭解AEM Headless JS SDK的使用方式。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 410
source-git-commit: e7f556737cdf6a92c0503d3b4a52eef1f71c8330
workflow-type: tm+mt
source-wordcount: '1479'
ht-degree: 1%

---


# 使用AEM的GraphQL API建置React應用程式

在本章中，您將探索AEM的GraphQL API如何在外部應用程式中推動體驗。

使用簡單的React應用程式來查詢及顯示AEM GraphQL API公開的&#x200B;**團隊**&#x200B;和&#x200B;**人員**&#x200B;內容。 React的使用在很大程度上不重要，而且消費的外部應用程式可以寫入任何平台的任何框架中。

## 先決條件

假設此多部分教學課程前幾部分所述的步驟已完成，或您的AEM作者和發佈服務上已安裝[basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip)。

本章中的&#x200B;_IDE熒幕擷取畫面來自[Visual Studio Code](https://code.visualstudio.com/)_

### AEM環境

若要完成本教學課程，建議您擁有AEM管理員存取權來存取下列其中一項：

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)進行本機設定
+ [AEM 6.5 LTS](https://experienceleague.adobe.com/docs/experience-manager-65/content/release-notes/release-notes.html?lang=zh-Hant)，已安裝[GraphQL Index Package 1.0.5+](https://experienceleague.adobe.com/docs/experience-manager-65/content/headless/graphql-api/graphql-endpoint.html)

### 軟體需求

必須安裝下列軟體：

+ [Node.js v18+](https://nodejs.org/en)
+ [Visual Studio程式碼](https://code.visualstudio.com/)
+ [Git](https://git-scm.com/)
+ [Java JDK](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-65/content/implementing/deploying/introduction/technical-requirements) (如果連線到本機AEM SDK或6.5執行個體)

## 目標

瞭解如何：

+ 下載並啟動範例React應用程式
+ 使用[AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)查詢AEM的GraphQL端點
+ 查詢AEM以取得團隊及其參照成員的清單
+ 查詢AEM以取得團隊成員的詳細資訊

## 取得範例React應用程式

在本章中，已使用與AEM的GraphQL API互動所需的程式碼實作簡略的範例React應用程式，並顯示從這些API取得的團隊和人員資料。

範例React應用程式原始碼可在Github.com上的<https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>取得

若要取得React應用程式：

1. 從[Github.com](https://github.com/adobe/aem-guides-wknd-graphql)複製範例WKND GraphQL React應用程式。 請記住，此Git存放庫包含多個專案，因此請務必依照下一個步驟所述導覽至`basic-tutorial`資料夾。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導覽至`basic-tutorial`資料夾，並在IDE中開啟。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   在VSCode中![React應用程式](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 更新`.env.development`以連線至您的AEM Publish服務。

   如需範例專案的環境變數及其設定方式的詳細資訊，[請檢閱README.md](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#update-environment-variables)。

   **AEM as a Cloud Service**

   將React應用程式連線至AEM as a Cloud Service Publish服務時，請將`REACT_APP_HOST_URI`的值設定為您的AEM as a Cloud Service發佈URL (例如 `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`)和`REACT_APP_AUTH_METHOD`的值至`none`

   **適用於AEM as a Cloud Service的AEM SDK （本機）**

   將React應用程式連線至本機AEM as a Cloud Service SDK環境時，請將`REACT_APP_HOST_URI`的值設為您的本機主機和發佈連線埠(例如 `REACT_APP_HOST_URI=http://localhost:4503`)和`REACT_APP_AUTH_METHOD`的值至`none`

   **AEM 6.5 LTS託管環境**

   將React應用程式連線至AEM as a Cloud Service Publish服務時，請將`REACT_APP_HOST_URI`的值設定為您的AEM 6.5發佈URL (例如 `REACT_APP_HOST_URI=https://dev.mysite.com`)和`REACT_APP_AUTH_METHOD`的值至`none`

   **AEM 6.5 LTS快速入門（本機）**

   將React應用程式連線至本機AEM 6.5快速入門時，請將`REACT_APP_HOST_URI`的值設為您的本機主機和發佈連線埠(例如 `REACT_APP_HOST_URI=http://localhost:4503`)和`REACT_APP_AUTH_METHOD`的值至`none`

   >[!NOTE]
   >
   > 請確定您已發佈專案設定、內容片段模型、編寫的內容片段、GraphQL端點，以及先前步驟中的持續查詢。
   >
   > 如果您在本機AEM Author SDK上執行上述步驟，您可以指向`http://localhost:4502`並將`REACT_APP_AUTH_METHOD`的值指向`basic`。


1. 從命令列，前往`aem-guides-wknd-graphql/basic-tutorial`資料夾

1. 啟動React應用程式

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React應用程式會在[http://localhost:3000/](http://localhost:3000/)上以開發模式啟動。 在教學課程中對React應用程式所做的變更會立即反映出來。

![部分實作的React應用程式](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   此React應用程式已部分實作。 依照本教學課程中的步驟完成實作。 需要實施作業的JavaScript檔案有下列註解，請務必使用本教學課程中指定的程式碼，新增/更新這些檔案中的程式碼。
>
>
> //*********************************
>
>  // TODO請依照AEM Headless教學課程中的步驟來實作此專案
>
>  //*********************************
>

## React應用程式剖析

範例React應用程式有三個主要部分：

1. `src/api`資料夾包含用來向AEM發出GraphQL查詢的檔案。
   + `src/api/aemHeadlessClient.js`會初始化並匯出用來與AEM通訊的AEM Headless使用者端
   + `src/api/usePersistedQueries.js`實作[自訂React鉤點](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components)從AEM GraphQL傳回資料到`Teams.js`和`Person.js`檢視元件。

1. `src/components/Teams.js`檔案使用清單查詢來顯示團隊及其成員的清單。
1. `src/components/Person.js`檔案使用引數化的單一結果查詢，顯示單一人員的詳細資料。

## 檢閱AEMHeadless物件

檢閱`aemHeadlessClient.js`檔案，瞭解如何建立用來與AEM通訊的`AEMHeadless`物件。

1. 開啟 `src/api/aemHeadlessClient.js`。

1. 複查明細行1-40：

   + 從JavaScript `AEMHeadless`的[AEM Headless使用者端匯入](https://github.com/adobe/aem-headless-client-js)宣告，第11行。

   + 根據`.env.development`、第14-22行中定義的變數以及箭號函式運算式`setAuthorization`、第31-40行所定義的授權組態。

   + 內含`serviceUrl`開發Proxy[組態的](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#proxy-api-requests)設定，第27行。

1. 第42到49行是最重要的，因為它們會將`AEMHeadless`使用者端例項化，並將其匯出以供在整個React應用程式中使用。

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

若要實作泛型`fetchPersistedQuery(..)`函式以執行AEM GraphQL持續查詢，請開啟`usePersistedQueries.js`檔案。 `fetchPersistedQuery(..)`函式使用`aemHeadlessClient`物件的`runPersistedQuery()`函式以非同步、承諾型行為執行查詢。

稍後，自訂React `useEffect`連結會呼叫此函式，以從AEM擷取特定資料。

1. 在`src/api/usePersistedQueries.js`中&#x200B;**更新** `fetchPersistedQuery(..)`，第35行，代碼如下。

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

接下來，建置可在React應用程式的主要檢視上顯示團隊及其成員的功能。 此功能需要：

+ [中的新](https://react.dev/reference/react/useEffect#useeffect)自訂React useEffect勾點`src/api/usePersistedQueries.js`，會叫用`my-project/all-teams`持續查詢，傳回AEM中的Team內容片段清單。
+ 位於`src/components/Teams.js`的React元件會叫用新的自訂React `useEffect`鉤點，並轉譯Teams資料。

完成後，應用程式的主要檢視會填入AEM中的團隊資料。

![團隊檢視](./assets/graphql-and-external-app/react-app__teams-view.png)

### 步驟

1. 開啟 `src/api/usePersistedQueries.js`。

1. 找到函式`useAllTeams()`

1. 若要建立透過`useEffect`叫用持續查詢`my-project/all-teams`的`fetchPersistedQuery(..)`連結，請新增下列程式碼。 此掛接也只會從`data?.teamList?.items`的AEM GraphQL回應傳回相關資料，讓React檢視元件與父JSON結構無關。

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

1. 開啟`src/components/Teams.js`

1. 在`Teams` React元件中，使用`useAllTeams()`勾點從AEM擷取團隊清單。

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. 執行以檢視為基礎的資料驗證，根據傳回的資料顯示錯誤訊息或載入指標。

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

1. 最後，轉譯團隊資料。 從GraphQL查詢傳回的每個團隊都會使用提供的`Team` React子元件轉譯。

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

完成[團隊功能](#implement-teams-functionality)後，讓我們實作這項功能以處理團隊成員或個人詳細資訊上的顯示。

此功能需要：

+ [中的新](https://react.dev/reference/react/useEffect#useeffect)自訂React useEffect勾點`src/api/usePersistedQueries.js`，它會叫用引數化`my-project/person-by-name`持續查詢，並傳回單一人員記錄。

+ 位於`src/components/Person.js`的React元件使用個人的全名作為查詢引數，叫用新的自訂React `useEffect`連結，並呈現個人資料。

完成後，在「團隊」檢視中選取人員名稱，即可呈現人員檢視。

![人員](./assets/graphql-and-external-app/react-app__person-view.png)

1. 開啟 `src/api/usePersistedQueries.js`。

1. 找到函式`usePersonByName(fullName)`

1. 若要建立透過`useEffect`叫用持續查詢`my-project/all-teams`的`fetchPersistedQuery(..)`連結，請新增下列程式碼。 此掛接也只會從`data?.teamList?.items`的AEM GraphQL回應傳回相關資料，讓React檢視元件與父JSON結構無關。

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

1. 開啟`src/components/Person.js`
1. 在`Person` React元件中，剖析`fullName`路由引數，並使用`usePersonByName(fullName)`勾點從AEM擷取人員資料。

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

1. 執行以檢視為基礎的資料驗證，根據傳回的資料顯示錯誤訊息或載入指標。

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


## CORS 設定

此範例React應用程式在開發期間不需要跨原始資源共用(CORS)，因為它使用本機Proxy連線到AEM。 本機Proxy僅用於促進快速開發，不適用於非開發使用案例。

不過，在生產案例中，通常最佳實務是使用者端應用程式從使用者的瀏覽器直接與AEM通訊。 若要啟用此功能，可能需要在AEM[中設定](../deployment/overview.md)CORS，以允許從您的網頁應用程式擷取請求。

## 試用應用程式

檢閱應用程式[http://localhost:3000/](http://localhost:3000/)並按一下&#x200B;_成員_&#x200B;連結。 您也可以在AEM中新增內容片段，以新增更多團隊和/或成員至Team Alpha。

>[!IMPORTANT]
>
>若要驗證您的實作變更，或是在上述變更後無法讓應用程式正常運作，請參閱[basic-tutorial](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) `solution`分支。

## 潛藏在引擎蓋下

開啟瀏覽器的&#x200B;**開發人員工具** > **網路**&#x200B;和&#x200B;_篩選器_，以處理`all-teams`請求。 請注意，GraphQL API要求`/graphql/execute.json/my-project/all-teams`是針對`http://localhost:3000`而提出的，且&#x200B;**NOT**&#x200B;是針對`REACT_APP_HOST_URI`的值，例如`<https://publish-pxxx-exxx.adobeaemcloud.com`。 這些要求是針對React應用程式的網域所發出，因為[Proxy設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)是使用`http-proxy-middleware`模組來啟用。


透過Proxy的![GraphQL API要求](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


檢閱主要`../setupProxy.js`檔案，並在`../proxy/setupProxy.auth.**.js`個檔案中注意到`/content`和`/graphql`路徑是如何進行代理的，並表明它不是靜態資產。

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

使用本機Proxy不適合用於生產部署，您可以在&#x200B;_生產部署_&#x200B;區段找到更多詳細資料。

## 恭喜！{#congratulations}

恭喜！您已成功建立React應用程式，以使用及顯示AEMGraphQL API的資料，作為基本教學課程的一部分！
