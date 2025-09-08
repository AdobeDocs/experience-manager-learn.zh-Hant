---
title: 檢測 React 應用程式以使用通用編輯器編輯內容
description: 了解如何檢測 React 應用程式以使用通用編輯器編輯內容。
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 421
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 2a25cd44-cbd1-465e-ae3f-d3876e915114
source-git-commit: 252d7045ba43c0998e9bb98fa86c399812ce92e9
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 100%

---

# 檢測 React 應用程式以使用通用編輯器編輯內容

了解如何檢測 React 應用程式以使用通用編輯器編輯內容。

## 先決條件

您已依照上一個[本機開發設定](./local-development-setup.md)步驟中的說明，設定本機開發環境。

## 包括通用編輯器核心程式庫

讓我們先在 WKND Teams React 應用程式中包含通用編輯器核心程式庫。這是在所編輯的應用程式和通用編輯器之間提供通訊層的 JavaScript 程式庫。

有兩種方法可以在 React 應用程式中包含通用編輯器核心程式庫：

1. 來自 npm 登錄的節點模組相依性，請參閱 [@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors)。
1. HTML 檔案中的指令碼標記 (`<script>`)。

對於本教學課程，我們使用指令碼標記方法。

1. 安裝 `react-helmet-async` 封裝來管理 React 應用程式中的 `<script>` 標記。

   ```bash
   $ npm install react-helmet-async
   ```

1. 更新 WKND Teams React 應用程式的 `src/App.js` 檔案，以便包含通用編輯器核心程式庫。

   ```javascript
   ...
   import { Helmet, HelmetProvider } from "react-helmet-async";
   
   function App() {
   return (
       <HelmetProvider>
           <div className="App">
               <Helmet>
                   {/* AEM Universal Editor :: CORE Library
                     Loads the LATEST Universal Editor library
                   */}
                   <script
                       src="https://universal-editor-service.adobe.io/cors.js"
                       async
                   />
               </Helmet>
               <Router>
                   <header>
                       <Link to={"/"}>
                       <img src={logo} className="logo" alt="WKND Logo" />
                       </Link>
                       <hr />
                   </header>
                   <Routes>
                       <Route path="/" element={<Home />} />
                       <Route path="/person/:fullName" element={<Person />} />
                   </Routes>
               </Router>
           </div>
       </HelmetProvider>
   );
   }
   
   export default App;
   ```

## 新增後設資料 - 內容來源

若要將 WKND Teams React 應用程式&#x200B;_與內容來源_&#x200B;連接以便進行編輯，您需要提供連線後設資料。通用編輯器服務使用此後設資料與內容來源建立連線。

連線後設資料以 `<meta>` 標記儲存在 HTML 檔案中。連線後設資料的語法如下：

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

我們要將連線後設資料新增到 `<Helmet>` 元件內的 WKND Teams React 應用程式。使用以下 `src/App.js` 標記更新 `<meta>` 檔案。在此範例中，內容來源是在 `https://localhost:8443`上執行的本機 AEM 實例。

```javascript
...
function App() {
return (
    <HelmetProvider>
        <div className="App">
            <Helmet>
                {/* AEM Universal Editor :: CORE Library
                    Loads the LATEST Universal Editor library
                */}
                <script
                    src="https://universal-editor-service.adobe.io/cors.js"
                    async
                />
                {/* AEM Universal Editor :: Connection metadata 
                    Connects to local AEM instance
                */}
                <meta
                    name="urn:adobe:aue:system:aemconnection"
                    content={`aem:https://localhost:8443`}
                />
            </Helmet>
            ...
    </HelmetProvider>
);
}

export default App;
```

 `aemconnection` 提供內容來源的簡稱。後續的檢測會使用簡稱來指示內容來源。

## 新增後設資料 - 本機通用編輯器服務設定

我們不採用 Adobe 託管的通用編輯器服務，而是使用通用編輯器服務的本機副本進行本機開發。本機服務將通用編輯器和 AEM SDK 連結起來，所以我們要把本機通用編輯器服務後設資料新增至 WKND Teams React 應用程式。

這些設定會以 `<meta>` 標記的形式儲存在 HTML 檔案中。本機通用編輯器服務後設資料的語法如下：

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

我們要將連線後設資料新增到 `<Helmet>` 元件內的 WKND Teams React 應用程式。使用以下 `src/App.js` 標記更新 `<meta>` 檔案。在此範例中，本機通用編輯器服務在 `https://localhost:8001` 上執行。

```javascript
...

function App() {
  return (
    <HelmetProvider>
      <div className="App">
        <Helmet>
          {/* AEM Universal Editor :: CORE Library
              Loads the LATEST Universal Editor library
          */}
          <script
            src="https://universal-editor-service.adobe.io/cors.js"
            async
          />
          {/* AEM Universal Editor :: Connection metadata 
              Connects to local AEM instance
          */}
          <meta
            name="urn:adobe:aue:system:aemconnection"
            content={`aem:https://localhost:8443`}
          />
          {/* AEM Universal Editor :: Configuration for Service
              Using locally running Universal Editor service
          */}
          <meta
            name="urn:adobe:aue:config:service"
            content={`https://localhost:8001`}
          />
        </Helmet>
        ...
    </HelmetProvider>
);
}
export default App;
```

## 檢測 React 元件

若要編輯 WKND Teams React 應用程式的內容 (例如 _團隊標題和團隊描述_)，您需要檢測 React 元件。檢測指在您想要透過通用編輯器編輯的 HTML 元素中，新增相關的資料屬性 (`data-aue-*`)。如需有關資料屬性的更多資訊，請參閱[屬性和類型](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types)。

### 定義可編輯元素

我們從定義想要透過通用編輯器編輯的元素開始。在 WKND Teams React 應用程式中，團隊標題和描述儲存在 AEM 的團隊內容片段中，因此是最適合編輯的候選內容。

讓我們檢測 `Teams` React 元件，讓團隊標題和描述變成可以編輯。

1. 開啟 WKND Teams React 應用程式的 `src/components/Teams.js` 檔案。
1. 新增 `data-aue-prop`、`data-aue-type` 和 `data-aue-label` 屬性到團隊標題和描述元素。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       return (
           <div className="team">
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <h2 className="team__title" data-aue-prop="title" data-aue-type="text" data-aue-label="title">{title}</h2>
               <p className="team__description" data-aue-prop="description" data-aue-type="richtext" data-aue-label="description">{description.plaintext}</p>
               ...
           </div>
       );
   }
   
   export default Teams;
   ```

1. 重新整理瀏覽器中載入 WKND Teams React 應用程式的通用編輯器頁面。現在您可以看到團隊標題和描述元素皆是可編輯的。

   ![通用編輯器 - WKND Teams 標題和描述可編輯](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. 如果您使用內嵌編輯或屬性邊欄來編輯團隊標題或描述，會顯示載入中轉圈圖示，但不允許您編輯內容。因為通用編輯器不知道載入和儲存內容的 AEM 資源詳細資料。

   ![通用編輯器 - WKND Teams 標題和描述載入中](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

總而言之，上述變更在通用編輯器中將團隊標題和描述元素標記為可編輯。然而，**您目前還無法編輯 (透過內嵌或屬性邊欄) 及儲存變更**，為此您需要使用 `data-aue-resource` 屬性新增 AEM 資源詳細資料。我們會在下一步完成這件事。

### 定義 AEM 資源詳細資料

要將編輯的內容儲存到 AEM 以及將內容載入到屬性邊欄中，您必須把 AEM 資源詳細資料提供給通用編輯器。

在本案例中，AEM 資源是團隊內容片段的路徑，因此我們要將資源詳細資料新增至位在頂層 `<div>` 元素的 `Teams` React 元件。

1. 更新 `src/components/Teams.js` 檔案，將 `data-aue-resource`、`data-aue-type` 和 `data-aue-label` 屬性新增到頂層的 `<div>` 元素。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       // Render single Team
       function Team({ _path, title, shortName, description, teamMembers }) {
           // Must have title, shortName and at least 1 team member
           if (!_path || !title || !shortName || !teamMembers) {
               return null;
           }
   
         return (
           // AEM Universal Editor :: Instrumentation using data-aue-* attributes
           <div className="team" data-aue-resource={`urn:aemconnection:${_path}/jcr:content/data/master`} data-aue-type="reference" data-aue-label={title}>
           ...
           </div>
       );
       }
   }
   export default Teams;
   ```

   `data-aue-resource` 屬性的值是團隊內容片段的 AEM 資源路徑。`urn:aemconnection:` 前置詞使用在連線後設資料中定義的內容來源簡稱。

1. 重新整理瀏覽器中載入 WKND Teams React 應用程式的通用編輯器頁面。現在您可以看到頂層的團隊元素是可編輯的，但屬性邊欄仍然沒有載入內容。在瀏覽器的網路標籤中，對於載入內容的 `details` 請求，您可以看到「401 未授權錯誤」。這是想要使用 IMS 權杖進行驗證，但是本機 AEM SDK 不支援 IMS 驗證。

   ![通用編輯器 - WKND Teams 團隊可編輯](./assets/universal-editor-wknd-teams-team-editable.png)

1. 若要修正 401 未授權錯誤，您必須使用通用編輯器中的&#x200B;**驗證標頭**&#x200B;選項，把本機 AEM SDK 驗證詳細資料提供給通用編輯器。作為其本機 AEM SDK，請將值設為 `Basic YWRtaW46YWRtaW4=` 以便使用 `admin:admin` 認證。

   ![通用編輯器 - 新增驗證標頭](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. 重新整理瀏覽器中載入 WKND Teams React 應用程式的通用編輯器頁面。現在您可以看到屬性邊欄正在載入內容，而您可以用內嵌功能或屬性邊欄來編輯團隊標題和描述。

   ![通用編輯器 - WKND Teams 團隊可編輯](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### 內部運作原理

屬性邊欄使用本機通用編輯器服務從 AEM 資源載入內容。使用瀏覽器的網路標籤，您可以看到對本機通用編輯器服務發出的 POST 要求 (`https://localhost:8001/details`)，要求載入內容。

當您使用內嵌編輯或屬性邊欄編輯內容時，相關變更會使用本機通用編輯器服務儲存到 AEM 資源中。使用瀏覽器的網路標籤，您可以看到對本機通用編輯器服務發出的 POST 要求 (`https://localhost:8001/update` 或 `https://localhost:8001/patch`)，要求儲存內容。

![通用編輯器 - WKND Teams 團隊可編輯](./assets/universal-editor-under-the-hood-request.png)

請求承載 JSON 物件包含必要的詳細資料，例如內容伺服器 (`connections`)、資源路徑 (`target`)，以及更新後的內容 (`patch`)。

![通用編輯器 - WKND Teams 團隊可編輯](./assets/universal-editor-under-the-hood-payload.png)

### 展開可編輯內容

我們要展開可編輯內容並將檢測套用到&#x200B;**團隊成員**，以便您可以使用屬性邊欄編輯團隊成員。

如同上述，我們要在 `Teams` React 元件中對團隊成員新增相關的 `data-aue-*` 屬性。

1. 更新 `src/components/Teams.js` 檔案，以便將資料屬性新增到 `<li key={index} className="team__member">` 元素。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {/* Render the referenced Person models associated with the team */}
               {teamMembers.map((teamMember, index) => {
                   return (
                       // AEM Universal Editor :: Instrumentation using data-aue-* attributes
                       <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                       <Link to={`/person/${teamMember.fullName}`}>
                           {teamMember.fullName}
                       </Link>
                       </li>
                   );
               })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

   因為團隊成員是作為 `Person` 內容片段儲存到 AEM 中，所以 `data-aue-type` 屬性的值是 `component`，並協助指示內容中可移動/可刪除的部分。

1. 重新整理瀏覽器中載入 WKND Teams React 應用程式的通用編輯器頁面。現在您可以看到團隊成員可以使用屬性邊欄進行編輯。

   ![通用編輯器 - WKND Teams 團隊成員可編輯](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### 內部運作原理

如同上述，內容檢索和儲存皆是由本機通用編輯器務完成。向本機通用編輯器發出 `/details`、`/update` 或 `/patch` 請求，要求載入和儲存內容。

### 定義新增和刪除內容

到目前為止，您已經使現有內容可編輯，但若要增加新內容，該怎麼做？我們要讓通用編輯器擁有可以新增或刪除 WKND 團隊成員的功能。這樣，內容作者便不需要到 AEM 去新增或刪除團隊成員。

無論如何，我們簡單回顧重點，WKND 團隊成員作為 `Person` 內容片段儲存在 AEM 中，並使用 `teamMembers` 屬性與團隊內容片段相關聯。若要檢閱 AEM 中的模型定義，請造訪 [my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project)。

1. 首先，建立元件定義檔 `/public/static/component-definition.json`。這個檔案包含 `Person` 內容片段的元件定義。 `aem/cf` 外掛程式允許根據模型及範本來插入內容片段，而範本提供要套用的預設值。

   ```json
   {
       "groups": [
           {
           "title": "Content Fragments",
           "id": "content-fragments",
           "components": [
               {
               "title": "Person",
               "id": "person",
               "plugins": {
                   "aem": {
                       "cf": {
                           "name": "person",
                           "cfModel": "/conf/my-project/settings/dam/cfm/models/person",
                           "cfFolder": "/content/dam/my-project/en",
                           "title": "person",
                           "template": {
                               "fullName": "New Person",
                               "biographyText": "This is biography of new person"
                               }
                           }
                       }
                   }
               }
           ]
           }
       ]
   }
   ```

1. 接下來，參照 WKND Team React 應用程式 `index.html` 中的上述元件定義檔。更新 `public/index.html` 檔案當中的 `<head>` 區段以便包含元件定義檔。

   ```html
   ...
   <script
       type="application/vnd.adobe.aue.component+json"
       src="/static/component-definition.json"
   ></script>
   <title>WKND App - Basic GraphQL Tutorial</title>
   </head>
   ...
   ```

1. 最後，更新 `src/components/Teams.js` 檔案以便新增資料屬性。**MEMBERS** 區段將用作團隊成員的容器，讓我們將 `data-aue-prop`、`data-aue-type`和 `data-aue-label` 屬性新增到 `<div>` 元素。

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       {/* AEM Universal Editor :: Team Members as container */}
       <div data-aue-prop="teamMembers" data-aue-type="container" data-aue-label="members">
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
           {/* Render the referenced Person models associated with the team */}
           {teamMembers.map((teamMember, index) => {
               return (
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                   <Link to={`/person/${teamMember.fullName}`}>
                   {teamMember.fullName}
                   </Link>
               </li>
               );
           })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

1. 重新整理瀏覽器中載入 WKND Teams React 應用程式的通用編輯器頁面。現在您可以看到 **MEMBERS** 區段用作容器。您可以使用屬性邊欄和 **+** 圖示插入新團隊成員。

   ![通用編輯器 - WKND Teams 團隊成員插入](./assets/universal-editor-wknd-teams-add-team-members.png)

1. 若要刪除團隊成員，請選取該團隊成員，然後按一下「**刪除**」圖示。

   ![通用編輯器 - WKND Teams 團隊成員刪除](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### 內部運作原理

新增和刪除內容的操作由本機通用編輯器服務完成。向本機通用編輯器服務發出包含詳細承載的 POST 要求至 `/add` 或 `/remove`，要求對 AEM 新增內容或刪除其中內容。

## 解決方案檔案

若要驗證您的實施變更，或如果您無法讓 WKND Teams React 應用程式與通用編輯器搭配運作，請參考 [basic-tutorial-instrumented-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) 解決方案分支。

您可以在[這裡](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1)針對運作中的 **basic-tutorial** 分支進行檔案逐一比對。

## 恭喜

您已成功檢測 WKND Teams React 應用程式，以使用通用編輯器新增、編輯和刪除內容。您已經了解如何包含核心程式庫、新增連線和本機通用編輯器服務後設資料，以及如何使用各種資料 (`data-aue-*`) 屬性來檢測 React 元件。
