---
title: 使用通用編輯器讓React應用程式可編輯 | Headless教學課程第5部分
description: 瞭解如何透過新增必要的檢測工具和設定，使您的React應用程式可在AEM Universal Editor中編輯。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 800
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 2%

---


# 使用通用編輯器讓React應用程式可編輯

在本章中，您將瞭解如何使用AEM Universal Editor讓[上一章](./4-react-app.md)中建立的React應用程式可編輯。 Universal Editor可讓內容作者直接在React應用程式體驗的環境中編輯內容，同時維持Headless應用程式的順暢體驗。

![通用編輯器](./assets/5/main.png)

Universal Editor提供的強大功能可讓您為任何Web應用程式啟用內容內嵌編輯，讓作者編輯內容時，不必在不同的撰寫介面之間切換。

## 先決條件

* 本教學課程的先前步驟已完成，尤其是[使用AEM的內容片段傳送OpenAPI建置React應用程式](./4-react-app.md)
* [如何使用及實作通用編輯器](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/introduction)的實用知識。

## 目標

瞭解如何：

* 將Universal Editor工具新增至React應用程式。
* 設定用於通用編輯器的React應用程式。
* 使用通用編輯器直接在React應用程式介面中啟用內容編輯。

## Universal Editor檢測

Universal Editor需要[HTML屬性和中繼標籤](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types)來識別可編輯的內容，並建立UI與AEM內容之間的連線。

### 新增通用編輯器標籤

首先，新增必要的中繼標籤，將React應用程式識別為相容於Universal Editor。

1. 在您的React應用程式中開啟`public/index.html`。
1. 在React應用程式的[區段中新增](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/getting-started)通用編輯器中繼標籤和CORS指令碼`<head>`：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="utf-8" />
       <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
       <meta name="viewport" content="width=device-width, initial-scale=1" />
       <meta name="theme-color" content="#000000" />
       <meta name="description" content="WKND Teams React App" />
   
       <!-- Universal Editor meta tags and CORS script -->
       <meta name="urn:adobe:aue:system:aemconnection" content="aem:%REACT_APP_AEM_AUTHOR_HOST_URI%" />
       <script src="https://universal-editor-service.adobe.io/cors.js"></script>
   
       <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
       <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
       <title>WKND Teams</title>
   </head>
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="root"></div>
   </body>
   </html>
   ```

1. 更新React應用程式的`.env`檔案，使其包含AEM Author服務主機，以支援通用編輯器中的回寫（用於`urn:adobe:aue:system:aemconnection` metat標籤的值）。

   ```bash
   # The AEM Publish (or Preview) service
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
   # The AEM Author service
   REACT_APP_AEM_AUTHOR_HOST_URI=https://author-p123-e456.adobeaemcloud.com
   ```

### 檢測teams元件

現在新增通用編輯器屬性，以使Teams元件可編輯。

1. 開啟 `src/components/Teams.js`。
1. 更新`Team`元件以包含[通用編輯器資料屬性](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types)：

   設定`data-aue-resource`屬性時，請確定使用OpenAPI的AEM內容片段傳送所傳回的內容片段的AEM路徑，會以內容片段變數的子路徑作為後置；在此例中為`/jcr:content/data/master`。

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
               {...team}
           />
           );
       })}
       </div>
   );
   }
   
   /**
   * Team - renders a single team with its details and members
   * @param {Object} fields - The authored Content Fragment fields
   * @param {Object} references - Hydrated references containing member details such as fullName
   * @param {string} path - Path of the team content fragment
   */
   function Team({ fields, references, path }) {
   if (!fields.title || !fields.teamMembers) {
       return null;
   }
   
   return (
       <>
       {/* Specify the correct Content Fragment variation path suffix in the data-aue-resource attribute */}
       <div className="team"
           data-aue-resource={`urn:aemconnection:${path}/jcr:content/data/master`}
           data-aue-type="component"
           data-aue-label={fields.title}>
   
           <h2 className="team__title"
           data-aue-prop="title"
           data-aue-type="text"
           data-aue-label="Team Title">{fields.title}</h2>
           <p className="team__description"
           data-aue-prop="description"
           data-aue-type="richtext"
           data-aue-label="Team Description"
           dangerouslySetInnerHTML={{ __html: fields.description.value }}
           />
           <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {fields.teamMembers.map((teamMember, index) => {
               return (
                   <li key={index} className="team__member">
                   <Link to={`/person/${teamMember}`}>
                       {references[teamMember].value.fields.fullName}
                   </Link>
                   </li>
               );
               })}
           </ul>
           </div>
       </div>
       </>
   );
   }
   
   export default Teams;
   ```

### 檢測人員元件

同樣地，將通用編輯器屬性新增至「人員」元件。

1. 開啟 `src/components/Person.js`。
1. 更新元件以包含[通用編輯器資料屬性](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types)：

   設定`data-aue-resource`屬性時，請確定使用OpenAPI的AEM內容片段傳送所傳回的內容片段的AEM路徑，會以內容片段變數的子路徑作為後置；在此例中為`/jcr:content/data/master`。

   ```javascript
   import "./Person.scss";
   import { useEffect, useState } from "react";
   import { useParams } from "react-router-dom";
   
   /**
   * Person component - displays detailed information about a single person
   * Fetches person data from AEM using the ID from the URL parameters
   */
   function Person() {
       const { id } = useParams();
       const [person, setPerson] = useState(null);
   
       useEffect(() => {
           const fetchData = async () => {
           try {
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
       }, [id]);
   
       if (!person) {
           return <div>Loading person...</div>;
       }
   
       /* Add the Universal Editor data-aue-* attirbutes to the rendered HTML */
       return (
           <div className="person"
               data-aue-resource={`urn:aemconnection:${person.path}/jcr:content/data/master`}
               data-aue-type="component"
               data-aue-label={person.fields.fullName}>
               <img className="person__image"
                   src={process.env.REACT_APP_HOST_URI + person.references[person.fields.profilePicture].value.path}
                   alt={person.fields.fullName}
                   data-aue-prop="profilePicture"
                   data-aue-type="media"
                   data-aue-label="Profile Picture"
               />
               <div className="person__occupations">
                   {person.fields.occupation.map((occupation, index) => {
                   return (
                       <span key={index} className="person__occupation">
                           {occupation}
                       </span>
                   );
                   })}
               </div>
   
               <div className="person__content">
                   <h1 className="person__full-name"
                       data-aue-prop="fullName"
                       data-aue-type="text"
                       data-aue-label="Full Name">
                       {person.fields.fullName}
                   </h1>
                   <div className="person__biography"
                       data-aue-prop="biographyText"
                       data-aue-type="richtext"
                       data-aue-label="Biography"
                       dangerouslySetInnerHTML={{ __html: person.fields.biographyText.value }}
                   />
               </div>
           </div>
       );
   }
   ```

### 取得完成的程式碼

本章的完整原始碼為[，可在Github.com](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end)上取得。


```bash
$ git fetch --tags
$ git tag
$ git checkout tags/headless_open-api_basic_5-end
```

## 測試通用編輯器整合

現在透過在Universal Editor中開啟React應用程式來測試通用編輯器相容性更新。

### 啟動React應用程式

1. 確認您的React應用程式執行中：

   ```bash
   $ cd ~/Code/aem-guides-wknd-openapi/basic-tutorial
   $ npm install
   $ npm start
   ```

1. 驗證應用程式會在`http://localhost:3000`載入並顯示團隊和人員內容。

### 執行本機SSL Proxy

通用編輯器要求透過HTTPS載入可編輯的應用程式。

1. 若要透過HTTPS執行本機React應用程式，請從命令列使用[local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy) npm模組。

   ```bash
   $ npm install -g local-ssl-proxy
   $ local-ssl-proxy --source 3001 --target 3000
   ```

1. 在網頁瀏覽器中開啟`https://localhost:3001`
1. 接受自我簽署憑證。
1. 驗證React應用程式是否已載入。

### 在通用編輯器中開啟

![在通用編輯器中開啟應用程式](./assets/5/open-app-in-universal-editor.png)

1. 瀏覽至[通用編輯器](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/)。
1. 在&#x200B;**網站URL**&#x200B;欄位中，輸入HTTPS React應用程式URL： `https://localhost:3001`。
1. 選取「按一下&#x200B;**開啟**」。

通用編輯器應載入已啟用編輯功能的React應用程式。

### 測試編輯功能

![在通用編輯器中編輯](./assets/5/edit-in-universal-editor.png)

1. 在Universal Editor中，將游標暫留在React應用程式中可編輯的元素上。

1. 若要在React應用程式中導覽，請開啟&#x200B;**預覽**&#x200B;模式，然後再次關閉以進行編輯。 請記住，**預覽**&#x200B;與AEM預覽服務無關，而是在通用編輯器中開啟和關閉編輯Chrome。

1. 您應該會看到正在編輯的指標，並且可以按一下React應用程式的各種可編輯元素。

1. 嘗試編輯團隊標題：
   * 按一下團隊標題
   * 編輯屬性面板中的文字
   * 儲存變更

1. 嘗試編輯個人的個人資料圖片：
   * 按一下個人的個人資料圖片
   * 從資產選擇器中選取新影像
   * 儲存變更

1. 按下Universal Editor右上角的&#x200B;**Publish**，將編輯內容發佈至AEM Publish （或Preview）服務，以便在Universal Editor的React應用程式中反映出來。

## 通用編輯器資料屬性

如需為通用編輯器檢測應用程式的完整檔案，請參閱[通用編輯器檔案](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/)。

## 恭喜！

恭喜！您已成功將Universal Editor與React應用程式整合。 內容作者現在可以直接在React應用程式介面中編輯內容片段，提供順暢的撰寫體驗，同時保留Headless架構的優點。

請記住，您一律可以從`main`GitHub.com存放庫[的](https://github.com/adobe/aem-tutorials/tree/main)分支取得此教學課程的最終原始程式碼。
