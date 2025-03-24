---
title: 網頁元件/JS - AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)無頭式功能的絕佳方式。 此網頁元件/JS應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 129
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---

# Web元件

範例應用程式是探索Adobe Experience Manager (AEM)無頭式功能的絕佳方式。 此Web元件應用程式示範了如何使用AEM的GraphQL API透過持續性查詢來查詢內容，以及如何轉譯UI的一部分，使用純JavaScript程式碼完成。

![含AEM Headless的Web元件](./assets/web-component/web-component.png)

在GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)上檢視[原始程式碼

## 先決條件 {#prerequisites}

下列工具應在本機安裝：

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM需求

網頁元件可搭配下列AEM部署選項使用。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)進行本機設定
   + 需要[JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14&amp;p.limit=144) (如果連線到本機AEM 6.5或AEM SDK)

此範例應用程式依賴安裝[basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip)，且已具備必要的[部署設定](../deployment/web-component.md)。


>[!IMPORTANT]
>
>網頁元件設計來連線至&#x200B;__AEM Publish__&#x200B;環境，不過，如果網頁元件的[`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11)檔案中有提供驗證，它就可以從AEM Author取得內容。

## 使用方式

1. 複製`adobe/aem-guides-wknd-graphql`存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導覽至`web-component`子目錄。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 編輯`.../src/person.js`檔案以包含AEM連線詳細資料：

   在`aemHeadlessService`物件中，更新`aemHost`以指向您的AEM發佈服務。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   如果連線到AEM作者服務，請在`aemCredentials`物件中提供本機AEM使用者認證。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 開啟終端機並從`aem-guides-wknd-graphql/web-component`執行命令：

   ```shell
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器視窗會開啟靜態的HTML頁面，其中在[http://localhost:8080](http://localhost:8080)嵌入網頁元件。
1. _個人資訊_&#x200B;網頁元件會顯示在網頁上。

## 程式碼

以下摘要說明如何建立Web元件、元件如何連線至AEM Headless以使用GraphQL持續查詢擷取內容，以及資料如何呈現。 您可以在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)上找到完整的程式碼。

### 網頁元件HTML標籤

已將可重複使用的Web元件（亦稱為自訂元素） `<person-info>`新增到`../src/assets/aem-headless.html` HTML頁面。 它支援`host`和`query-param-value`屬性來驅動元件的行為。 `host`屬性的值會覆寫`person.js`中`aemHeadlessService`物件的`aemHost`值，而`query-param-value`是用來選取要呈現的人員。

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web元件實施

`person.js`定義了Web元件功能，下面是其中的主要重點專案。

#### PersonInfo元素實作

`<person-info>`自訂專案的類別物件使用`connectedCallback()`生命週期方法來定義功能、附加陰影根、擷取GraphQL持續查詢，以及使用DOM操作來建立自訂專案的內部陰影DOM結構。

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src',
            host + (person.profilePicture._dynamicUrl || person.profilePicture._path));
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### 登入`<person-info>`專案

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### 跨原始資源共用(CORS)

此Web元件仰賴在目標AEM環境上執行的AEM型CORS設定，並假設主機頁面在開發模式及以下的`http://localhost:8080`上執行為本機AEM Author服務的CORS OSGi設定範例。

請檢閱各個AEM服務的[部署設定](../deployment/web-component.md)。
