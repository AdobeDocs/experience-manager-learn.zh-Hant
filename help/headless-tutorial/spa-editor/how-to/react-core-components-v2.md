---
title: How to use AEM React Editable Components v2
description: Learn how to use AEM React Editable Components v2 to power a React app.
version: Experience Manager as a Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 190
hide: true
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '634'
ht-degree: 4%

---

# How to use AEM React Editable Components v2

{{spa-editor-deprecation}}

AEM provides [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), an Node.js-based SDK that allows the creation of React components, that support in-context component editing using AEM SPA Editor.

* [npm module](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
* [Github project](https://github.com/adobe/aem-react-editable-components)
* [Adobe documentation](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html?lang=zh-Hant)


For more details and code samples for AEM React Editable Components v2 review the technical documentation:

* [Integration with AEM documentation](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
* [Editable component documentation](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
* [Helpers documentation](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM pages

AEM React Editable Components work with both SPA Editor or Remote SPA React apps. Content populating the editable React components, must be exposed via AEM pages that extend the [SPA Page component](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html?lang=zh-Hant). AEM components, that maps to editable React components, must implement AEM&#39;s [Component Exporter framework](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html?lang=zh-Hant) - such as [AEM Core WCM Components](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant).


## 相依性

Ensure that the React app is running on Node.js 14+.

使用AEM React Editable Components v2的React應用程式的最小相依性集合為： `@adobe/aem-react-editable-components`、`@adobe/aem-spa-component-mapping`和`@adobe/aem-spa-page-model-manager`。

* `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [AEM React Core WCM Components Base](https://github.com/adobe/aem-react-core-wcm-components-base)和[AEM React Core WCM Components SPA](https://github.com/adobe/aem-react-core-wcm-components-spa)與AEM React Editable Components v2不相容。

## SPA 編輯器

將AEM React Editable Components與SPA Editor型React應用程式搭配使用時，AEM `ModelManager` SDK會當作SDK：

1. 從AEM擷取內容
1. 以AEM的內容填入React可食用元件

使用初始化的ModelManager包裝React應用程式，並轉譯React應用程式。 React應用程式應包含一個從`@adobe/aem-react-editable-components`匯出的`<Page>`元件執行個體。 `<Page>`元件具有根據AEM提供的`.model.json`動態建立React元件的邏輯。

* `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

透過`ModelManager`提供的`pageModel`，將`<Page>`傳遞為AEM頁面的JSON表示法。 `<Page>`元件會將`resourceType`與透過`MapTo(..)`向資源型別註冊的React元件比對，以動態方式建立`pageModel`中物件的React元件。

## 可編輯的元件

`<Page>`是透過`ModelManager`以JSON形式傳遞AEM頁面的表示法。 接著，`<Page>`元件會將JS物件的`resourceType`值與React元件比對，以動態建立JSON中每個物件的React元件，而該元件會透過元件的`MapTo(..)`引動過程將其本身註冊為資源型別。 例如，以下將用於例項化執行個體

* `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

AEM提供的上述JSON可用來動態例項化及填入可編輯的React元件。

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## 內嵌元件

可編輯的元件可重複使用並相互嵌入。 將一個可編輯元件內嵌於另一個元件時，有兩個主要考量事項：

1. AEM內嵌元件的JSON內容必須包含滿足內嵌元件要求的內容。 可透過為AEM元件建立對話方塊以收集必要資料來完成此操作。
1. React元件的「不可編輯」執行個體必須內嵌，而非以`<EditableComponent>`包住的「可編輯」執行個體。 原因在於，如果內嵌元件具有`<EditableComponent>`包裝函式，SPA編輯器會嘗試使用編輯鉻黃（藍色暫留方塊）來裝飾內部元件，而不是使用外部內嵌元件。

* `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

AEM提供的上述JSON可用來動態例項化和填入可編輯的React元件，該元件內嵌其他React元件。


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```
