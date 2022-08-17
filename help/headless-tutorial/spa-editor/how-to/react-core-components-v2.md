---
title: 如何使用AEMReact Editable Components v2
description: 瞭解如何使AEM用React Editable Components v2為React應用程式提供電源。
version: Cloud Service
feature: SPA Editor
topic: Headless
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
source-git-commit: 18a414b847a7353eebcfad4bcc125920258948b3
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 1%

---


# 如何使用AEMReact Editable Components v2

AEM [反AEM應可編輯元件v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components)，一個基於Node.js的SDK，允許建立React元件，支援使用編輯器進行上下文元件編AEMSPA輯。

+ [npm模組](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [吉圖布項目](https://github.com/adobe/aem-react-editable-components)
+ [Adobe文檔](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


有關React Editable Components v2的詳細信AEM息和代碼示例，請查看技術文檔：

+ [與文檔集AEM成](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [可編輯的元件文檔](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [幫助程式文檔](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM頁

React Editable ComponentsAEM可與Editor或Remote SPAReact Apps一起使用。 填充可編輯的React元件的內容必須通過擴展AEM了 [頁SPA面元件](https://experienceleague.adobe.com/docs/experience-manager-64/developing/headless/spas/spa-page-component.html)。 映AEM射到可編輯React元件的元件必須實AEM施 [元件導出器框架](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html)  — 例如 [核AEM心WCM元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)。


## 相依性

確保React應用在Node.js 14+上運行。

React應用使用React Editable Components v2AEM的最小依賴關係集為： `@adobe/aem-react-editable-components`。 `@adobe/aem-spa-component-mapping`,  `@adobe/aem-spa-page-model-manager`。


+ `package.json`

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
> [反AEM應核心WCM元件基](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [反AEM應核心WCM組SPA件](https://github.com/adobe/aem-react-core-wcm-components-spa) 與React Editable Components v2AEM不相容。

## 編SPA輯器

將React Editable Components與AEM基於編輯器的React SPAApp一起使用時，AEM `ModelManager` SDK，作為SDK:

1. 從中檢索內AEM容
1. 使用內容填充反應可食AEM用元件

將React應用程式套件裝為已初始化的ModelManager，並呈現React應用程式。 React應用應包含 `<Page>` 從導出的元件 `@adobe/aem-react-editable-components`。 的 `<Page>` 元件具有邏輯，可基於 `.model.json` 由提供AEM。

+ `src/index.js`

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

的 `<Page>` 作為頁面AEM的表示形式傳遞為JSON，通過 `pageModel` 由 `ModelManager`。 的 `<Page>` 元件動態建立對象的反應元件 `pageModel` 通過匹配 `resourceType` 與通過註冊到資源類型的Reacte元件 `MapTo(..)`。

## 可編輯元件

的 `<Page>` 將頁AEM的表示形式作為JSON傳遞 `ModelManager`。 的 `<Page>` 然後，通過匹配JS對象的 `resourceType` 值與通過元件的 `MapTo(..)` 調用。 例如，以下內容將用於實例化實例

+ `HTTP GET /content/.../home.model.json`

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

提供的上述JSON可AEM用於動態實例化和填充可編輯的React元件。

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
 * If this component will be embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## 嵌入元件

可編輯的元件可以重複使用並相互嵌入。 將一個可編輯元件嵌入另一個元件時，需要考慮以下兩個關鍵因素：

1. 嵌入元件的AEMJSON內容必須包含滿足嵌入元件的內容。 為此，請為收集所需資料AEM的元件建立對話框。
1. 必須嵌入React元件的「不可編輯」實例，而不是包含「可編輯」實例的 `<EditableComponent>`。 原因是，如果嵌入的元件 `<EditableComponent>` 包裝器，SPA編輯器嘗試用編輯chrome（藍色懸停框）而不是外部嵌入元件來為內部元件著裝。

+ `HTTP GET /content/.../home.model.json`

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

提供的上述JSON可AEM用於動態實例化和填充可編輯的React元件，該元件嵌入另一個React元件。


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



