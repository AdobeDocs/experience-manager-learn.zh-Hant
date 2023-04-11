---
title: Web元件/JS - AEM無頭範例
description: 範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此Web元件/JS應用程式示範如何使用持續查詢，使用AEM GraphQL API來查詢內容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 5%

---


# Web元件

範例應用程式是探索Adobe Experience Manager(AEM)無頭功能的絕佳方式。 此Web元件應用程式示範如何使用持續查詢使用AEM GraphQL API來查詢內容，並轉譯部分UI，這是使用純JavaScript程式碼完成的。

![具有AEM無頭的Web元件](./assets/web-component/web-component.png)

檢視 [GitHub原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 必備條件 {#prerequisites}

應在本機安裝下列工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14) (若連線至本機AEM 6.5或AEM SDK)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM需求

Web元件可搭配下列AEM部署選項使用。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)
+ [AEM 6.5 SP13+快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

所有部署都需 `tutorial-solution-content.zip` 從 [解決方案檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) 必須安裝 [部署配置](../deployment/web-component.md) 的URL。


>[!IMPORTANT]
>
>Web元件設計為連接至 __AEM發佈__ 環境，但如果Web元件中提供驗證，則可從AEM作者來源內容 [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 檔案。

## 如何使用

1. 複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導覽至 `web-component` 子目錄中。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 編輯 `.../src/person.js` 包含AEM連線詳細資訊的檔案：

   在 `aemHeadlessService` 對象，更新 `aemHost` 來指向您的AEM Publish服務。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   如果連線至AEM製作服務，請在 `aemCredentials` 物件，提供本機AEM使用者憑證。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 開啟終端機，然後從 `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器視窗會開啟靜態HTML頁面，其中內嵌Web元件於 [http://localhost:8080](http://localhost:8080).
1. 此 _人員資訊_ Web元件顯示在網頁上。

## 程式碼

以下是如何建置Web元件、如何連線至AEM Headless以使用GraphQL持續查詢擷取內容的摘要，以及如何呈現該資料。 您可以在上找到完整的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### Web元件HTML標籤

可重複使用的Web元件（也稱為自訂元素） `<person-info>` 已新增至 `../src/assets/aem-headless.html` HTML頁面。 支援 `host` 和 `query-param-value` 屬性，以驅動元件的行為。 此 `host` 屬性的值覆蓋 `aemHost` 值自 `aemHeadlessService` 物件 `person.js`，和 `query-param-value` 用於選擇要呈現的人員。

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web元件實作

此 `person.js` 定義Web元件功能，其關鍵重點如下。

#### PersonInfo元素實作

此 `<person-info>` 自訂元素的類別物件會使用 `connectedCallback()` 生命週期方法、附加陰影根、擷取GraphQL持續查詢，以及DOM操作以建立自訂元素的內部陰影DOM結構。

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
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### 註冊 `<person-info>` 元素

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### 跨原始資源共用(CORS)

此Web元件需仰賴目標AEM環境上執行的AEM型CORS設定，並假設主機頁面執行於 `http://localhost:8080` 在開發模式中，以下是本機AEM製作服務的範例CORS OSGi設定。

請查閱 [部署配置](../deployment/web-component.md) 以取得AEM服務。

![CORS設定](assets/react-app/cross-origin-resource-sharing-configuration.png)
