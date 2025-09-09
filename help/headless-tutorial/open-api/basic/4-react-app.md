---
title: 使用AEM OpenAPI建置React應用程式 | Headless教學課程第4部分
description: 開始使用Adobe Experience Manager (AEM)和OpenAPI。 建置React應用程式，從AEM的OpenAPI型內容片段傳送API擷取內容/資料。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 900
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '874'
ht-degree: 1%

---


# 使用AEM的內容片段傳送OpenAPI建置React應用程式

在本章中，您將探索使用OpenAPI傳送AEM內容片段如何推動外部應用程式中的體驗。

一個簡單的React應用程式可用來請求和顯示透過OpenAPI的AEM內容片段傳遞公開的&#x200B;**團隊**&#x200B;和&#x200B;**人員**&#x200B;內容。 React的使用基本上不重要，只要可以向AEM as a Cloud Service發出HTTP請求，消費的外部應用程式便可以在任何平台的任何框架中撰寫。

## 先決條件

假設已完成此多部分教學課程前幾部分所概述的步驟。

必須安裝下列軟體：

* [Node.js v22+](https://nodejs.org/en)
* [Visual Studio程式碼](https://code.visualstudio.com/)

## 目標

瞭解如何：

* 下載並啟動範例React應用程式。
* 使用OpenAPI叫用AEM內容片段傳送，以取得團隊及其參考成員的清單。
* 使用OpenAPI叫用AEM內容片段傳送，以擷取團隊成員的詳細資訊。

## 在AEM as a Cloud Service上設定CORS

此範例React應用程式會在本機（在`http://localhost:3000`）執行，並使用OpenAPI API連線至AEM Publish服務的AEM內容片段傳送。 若要允許此連線，必須在AEM Publish （或Preview）服務上設定CORS （跨原始資源共用）。

依照設定在[上執行的SPA的`http://localhost:3000`指示，允許CORS要求傳送至AEM Publish服務](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#different-domains)。

### 本機CORS Proxy

或者，若要進行開發，請執行[本機CORS Proxy](https://www.npmjs.com/package/local-cors-proxy)，以提供AEM的CORS友好連線。

```bash
$ npm install --global lcp
$ lcp --proxyUrl https://publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com
```

將`--proxyUrl`值更新至您的AEM發佈（或預覽） URL。

在本機CORS Proxy執行下，存取位於`http://localhost:8010/proxy`的AEM內容片段傳送API以避免CORS問題。

## 複製範例React應用程式

實作一個現成的範例React應用程式，其中包含與OpenAPI的AEM內容片段傳送互動所需的程式碼，並顯示從這些API取得的團隊和人員資料。

範例React應用程式原始碼為[可在Github.com](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic)上取得。

若要取得React應用程式：

1. 從[Github.com](https://github.com/adobe/aem-tutorials)從[`headless_open-api_basic`標籤](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic)複製範例WKND OpenAPI React應用程式。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-tutorials.git
   $ cd aem-tutorials  
   $ git fetch --tags
   $ git tag
   $ git checkout tags/headless_open-api_basic
   ```

1. 導覽至`headless/open-api/basic`資料夾，並在IDE中開啟。

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ code .
   ```

1. 更新`.env`以連線至AEM as a Cloud Service Publish服務，因為這是發佈內容片段的位置。 如果您想要使用AEM預覽服務測試應用程式（且內容片段會發佈在那裡），這可以指向AEM預覽服務。

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   ```

   使用[本機CORS Proxy](#local-cors-proxy)時，將`REACT_APP_HOST_URI`設為`http://localhost:8010/proxy`。

   ```
   # AEM Publish (or Preview) service that provides Content Fragments
   REACT_APP_HOST_URI=http://localhost:8010/proxy
   ```

1. 啟動React應用程式

   ```shell
   $ cd ~/Code/aem-tutorials/headless/open-api/basic
   $ npm install
   $ npm start
   ```

1. React應用程式會在[http://localhost:3000/](http://localhost:3000/)上以開發模式啟動。 在教學課程中對React應用程式所做的變更，會立即反映在網頁瀏覽器中。

>[!IMPORTANT]
>
>   此React應用程式已部分實作。 請依照本教學課程中的步驟完成實作。 需要實施作業的JavaScript檔案有下列註解，請務必使用本教學課程中指定的程式碼，新增/更新這些檔案中的程式碼。
>
>
>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>  &#x200B;>  // TODO：依照AEM Headless教學課程中的步驟實作此專案
>  &#x200B;>  //**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;**&#x200B;***
>

## React應用程式剖析

範例React應用程式有三個主要部分需要更新。

1. `.env`檔案包含AEM發佈（或預覽）服務URL。
1. `src/components/Teams.js`會顯示團隊及其成員的清單。
1. `src/components/Person.js`會顯示單一團隊成員的詳細資料。

## 實作團隊功能

建置在React應用程式主要檢視上顯示團隊及其成員的功能。 此功能需要：

* 新的[自訂React useEffect勾點](https://react.dev/reference/react/useEffect#useeffect)會透過擷取要求叫用&#x200B;**列出所有內容片段API**，然後取得每個`fullName`的`teamMember`值以供顯示。

完成後，應用程式的主要檢視會填入AEM中的團隊資料。

![團隊檢視](./assets/4/teams.png)

1. 開啟 `src/components/Teams.js`。

1. 實作&#x200B;**Teams**&#x200B;元件以從[List all Content Fragments API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragments)擷取Teams清單，並轉譯Teams內容。 此程式分為下列步驟：

1. 建立叫用AEM的`useEffect`列出所有內容片段&#x200B;**API並將資料儲存到React元件狀態的**&#x200B;連結。
1. 針對傳回的每個&#x200B;**團隊**&#x200B;內容片段，叫用&#x200B;**取得內容片段** API以擷取團隊的完整水合詳細資訊，包括其成員及其`fullNames`。
1. 使用`Team`函式轉譯Teams資料。

   ```javascript
   import { useEffect, useState } from "react";
   import { Link } from "react-router-dom";
   import "./Teams.scss";
   
   function Teams() {
   
     // The teams folder is the only folder-tree that is allowed to contain Team Content Fragments.
     const TEAMS_FOLDER = '/content/dam/my-project/en/teams';
   
     // State to store the teams data
     const [teams, setTeams] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches all teams and their associated member details
       * This is a two-step process:
       * 1. First, get all team content fragments from the specified folder
       * 2. Then, for each team, fetch the full details including hydrated references to get the team member names
       */
       const fetchData = async () => {
         try {
           // Step 1: Fetch all teams from the teams folder
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments?path=${TEAMS_FOLDER}`
           );
           const allTeams = (await response.json()).items || [];
   
           // Step 2: Fetch detailed information for each team with hydrated references
           const hydratedTeams = [];
           for (const team of allTeams) {
             const hydratedTeamResponse = await fetch(
               `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${team.id}?references=direct-hydrated`
             );
             hydratedTeams.push(await hydratedTeamResponse.json());
           }
   
           setTeams(hydratedTeams);
         } catch (error) {
           console.error("Error fetching content fragments:", error);
         }
       };
   
       fetchData();
     }, [TEAMS_FOLDER]);
   
     // Show loading state while teams data is being fetched
     if (!teams) {
       return <div>Loading teams...</div>;
     }
   
     // Render the teams
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return (
             <Team 
               key={index} 
               {..team}
             />
           );
         })}
       </div>
     );
   }
   
   /**
   * Team component - renders a single team with its details and members
   * @param {string} fields - The authorable fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   */
   function Team({ fields, references, path }) {
     if (!fields.title || !fields.teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{fields.title}</h2>
         {/* Render description as HTML using dangerouslySetInnerHTML */}
         <p 
           className="team__description" 
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
         />
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render each team member as a link to their detail page */}
             {fields.teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                     {/* Display the full name from the hydrated reference */}
                     {references[teamMember].value.fields.fullName}
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

完成[團隊功能](#implement-teams-functionality)後，請實作該功能以處理團隊成員或個人詳細資訊上的顯示。

![人員檢視](./assets/4/person.png)

若要這麼做：

1. 開啟`src/components/Person.js`
1. 在`Person` React元件中，剖析`id`路由引數。 請注意，React應用程式的路由先前已設定為接受`id` URL引數（請參閱`/src/App.js`）。
1. 從AEM擷取人員資料[取得內容片段API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/#operation/fragments/getFragment)。

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
     // Get the person ID from the URL parameter
     const { id } = useParams();
   
     // State to store the person data
     const [person, setPerson] = useState(null);
   
     useEffect(() => {
       /**
       * Fetches person data from AEM Content Fragment Delivery API
       * Uses the ID from URL parameters to get the specific person's details
       */
       const fetchData = async () => {
         try {
           /* Hydrate references for access to profilePicture asset path */
           const response = await fetch(
             `${process.env.REACT_APP_HOST_URI}/adobe/contentFragments/${id}?references=direct-hydrated`
           );
           const json = await response.json();
           setPerson(json || null);
         } catch (error) {
           console.error("Error fetching person data:", error);
         }
       };
       fetchData();
     }, [id]); // Re-fetch when ID changes
   
     // Show loading state while person data is being fetched
     if (!person) {
       return <div>Loading person...</div>;
     }
   
     return (
       <div className="person">
         {/* Person profile image - Look up the profilePicture reference in the references object */}
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
           alt={person.fields.fullName}
         />
         {/* Display person's occupations */}
         <div className="person__occupations">
           {person.fields.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
   
         {/* Person's main content: name and biography */}
         <div className="person__content">
           <h1 className="person__full-name">{person.fields.fullName}</h1>
           {/* Render biography as HTML content */}
           <div
             className="person__biography"
             dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
           />
         </div>
       </div>
     );  
   }
   
   export default Person;
   ```

### 取得完成的程式碼

本章的完整原始碼為[，可在Github.com](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end)上取得。

```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_4-end
```

## 試用應用程式

檢閱應用程式[http://localhost:3000/](http://localhost:3000/)並按一下&#x200B;_團隊成員_&#x200B;連結。 您也可以新增更多團隊和/或成員至Alpha團隊，方法是在AEM Author服務中新增內容片段並加以發佈。

## 內部運作原理

當您與React應用程式互動時，開啟瀏覽器的&#x200B;**開發人員工具>網路**&#x200B;主控台和&#x200B;**篩選**&#x200B;以擷取`/adobe/contentFragments`請求。

## 恭喜！

恭喜！您已成功建立React應用程式，以便透過OpenAPI使用及顯示AEM內容片段傳送的內容片段。
