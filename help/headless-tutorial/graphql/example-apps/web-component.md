---
title: Web元件/JS — 無AEM頭示例
description: 示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此Web元件/JS應用程式演示了如何使用永續查AEM詢使用GraphQL API查詢內容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 2%

---


# Web元件

示例應用程式是探索Adobe Experience Manager()無頭功能的極AEM好方法。 此Web元件應用程式演示了如何使用永續查詢AEM使用GraphQL API查詢內容，並呈現一部分UI（使用純JavaScript代碼完成）。

![帶無頭的Web組AEM件](./assets/web-component/web-component.png)

查看 [GitHub上的原始碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 必備條件 {#prerequisites}

應在本地安裝以下工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;order=%40jcr%3內容%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (如果連接到AEM本地6.5或AEMSDK)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [蠢貨](https://git-scm.com/)

## AEM要求

Web元件可以使用以下部AEM署選項。

+ [AEM as a Cloud Service ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本地設定使用 [AEM Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM6.5 SP13+快速啟動](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

所有部署都需要 `tutorial-solution-content.zip` 從 [解決方案檔案](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) 必須安裝 [部署配置](../deployment/web-component.md) 。


>[!IMPORTANT]
>
>Web元件設計為連接到 __AEM發佈__ 但是，如果Web元件中提供了身份驗證，則它可以從AEM Author中源出內容 [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 的子菜單。

## 如何使用

1. 克隆 `adobe/aem-guides-wknd-graphql` 儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 導航到 `web-component` 的子目錄。

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 編輯 `.../src/person.js` 要包括連接詳細AEM資訊的檔案：

   在 `aemHeadlessService` 對象，更新 `aemHost` 指向AEM發佈服務。

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   如果連接到AEM作者服務，請在 `aemCredentials` 對象，提供本地AEM用戶憑據。

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 開啟終端並運行命令 `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. 新的瀏覽器窗口將開啟靜態HTML頁，該頁在 [http://localhost:8080](http://localhost:8080)。
1. 的 _人員資訊_ Web元件顯示在網頁上。

## 代碼

以下是Web元件的構建方式、它如何連接到AEMHeadless以使用GraphQL永續查詢檢索內容，以及如何顯示該資料的摘要。 可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)。

### Web元件HTML標籤

可重用的Web元件（又稱自定義元素） `<person-info>` 添加到 `../src/assets/aem-headless.html` HTML。 它支援 `host` 和 `query-param-value` 屬性，以驅動元件的行為。 的 `host` 屬性的值覆蓋 `aemHost` 值 `aemHeadlessService` 對象 `person.js`, `query-param-value` 用於選擇要呈現的人員。

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### Web元件實現

的 `person.js` 定義Web元件功能，下面是其中的關鍵亮點。

#### PersonInfo元素實現

的 `<person-info>` custom element的類對象使用 `connectedCallback()` 生命週期方法、附加陰影根、獲取GraphQL永續查詢和DOM操作以建立自定義元素的內部陰影DOM結構。

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

### 跨源資源共用(CORS)

此Web元件依賴於在AEM目標環境上運行的基於CORS的AEM配置，並假定主機頁在 `http://localhost:8080` 在開發模式中，下面是本地AEM Author服務的CORS OSGi配置示例。

請查看 [部署配置](../deployment/web-component.md) 為各自的服AEM務。

![CORS配置](assets/react-app/cross-origin-resource-sharing-configuration.png)
