---
title: 將可編輯的固定元件添加到遠SPA程
description: 瞭解如何將可編輯的固定元件添加到遠SPA程。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 1%

---

# 可編輯的固定元件

可編輯的React元件可以是「固定」的，也可以硬編碼到視SPA圖中。 這允許開發人員將SPA與Editor相容的元件放SPA入視圖中，並允許用戶在編輯器中建立元件AEM的SPA內容。

![固定元件](./assets/spa-fixed-component/intro.png)

在本章中，我們將替換「首頁」視圖的標題「當前冒險」，該標題是 `Home.js` 具有固定但可編輯的標題元件。 固定元件可保證標題的放置，但也允許創作標題的文本，並在開發週期之外進行更改。

## 更新WKND應用

添加 __固定__ 「首頁」視圖的元件：

+ 建立自定義可編輯的標題元件，並將其註冊到項目的標題的資源類型
+ 將可編輯的「標題」元件置SPA於「首頁」視圖

### 建立可編輯的React Title元件

在「主SPA頁」視圖中，替換硬編碼文本 `<h2>Current Adventures</h2>` 具有自定義可編輯的標題元件。 在使用「標題」元件之前，我們必須：

1. 建立自定義標題反應元件
1. 使用來自的方法裝飾自定義標題元件 `@adobe/aem-react-editable-components` 使其可編輯。
1. 將可編輯的標題元件註冊到 `MapTo` 這樣它就可以 [容器元件](./spa-container-component.md)。

要執行此操作：

1. 開啟遠SPA程項目 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app` 在IDE中
1. 在以下位置建立React元件 `react-app/src/components/editable/core/Title.js`
1. 將以下代碼添加到 `Title.js`。

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   const TitleLink = (props) => {
   return (
       <RoutedLink className={props.baseCssClass + (props.nested ? '-' : '__') + 'link'} 
           isRouted={props.routed} 
           to={props.linkURL}>
       {props.text}
       </RoutedLink>
   );
   };
   
   const TitleV2Contents = (props) => {
       if (!props.linkDisabled) {
           return <TitleLink {...props} />
       }
   
       return <>{props.text}</>
   };
   
   export const Title = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-title'
       }
   
       const elementType = (!!props.type) ? props.type.toString() : 'h3';
       return (<div className={props.baseCssClass}>
           {
               React.createElement(elementType, {
                       className: props.baseCssClass + (props.nested ? '-' : '__') + 'text',
                   },
                   <TitleV2Contents {...props} />
               )
           }
   
           </div>)
   }
   
   export const titleIsEmpty = (props) => props.text == null || props.text.trim().length === 0
   ```

   請注意，此React元件尚不可使用編輯AEM器編SPA輯。 此基本元件將在下一步中進行編輯。

   閱讀代碼的注釋，瞭解實施詳細資訊。

1. 在以下位置建立React元件 `react-app/src/components/editable/EditableTitle.js`
1. 將以下代碼添加到 `EditableTitle.js`。

   ```javascript
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import React from 'react'
   
   // Import the AEM the Title component implementation and it's Empty Function
   import { Title, titleIsEmpty } from "./core/Title";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   // The sling:resourceType of the AEM component used to collected and serialize the data this React component displays
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Title",        // The component placeholder in AEM SPA Editor
       isEmpty: titleIsEmpty,      // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   export const WrappedTitle = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Title, "cmp-title"), titleIsEmpty, "TitleV2")
       return <Wrapped {...props} />
   }
   
   // EditableComponent makes the component editable by the AEM editor, either rendered statically or in a container
   const EditableTitle = (props) => <EditableComponent config={EditConfig} {...props}><WrappedTitle /></EditableComponent>
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(EditableTitle);
   
   export default EditableTitle;
   ```

   此 `EditableTitle` 反應元件包裝 `Title` 反應元件、包裝和裝飾，以在編輯器中AEM進行SPA編輯。

### 使用React EditableTitle元件

現在，EditableTitle React元件已註冊到React應用中並可供使用，請替換「首頁」視圖上的硬編碼標題文本。

1. 編輯 `react-app/src/components/Home.js`
1. 在 `Home()` 在底部，導入 `EditableTitle` 用新的 `AEMTitle` 元件：

   ```javascript
   ...
   import EditableTitle from './editable/EditableTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
           <EditableTitle
               pagePath='/content/wknd-app/us/en/home'
               itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

的 `Home.js` 檔案應如下所示：

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## 在中建立Title組AEM件

1. 登錄到AEM作者
1. 導航到 __站點> WKND應用__
1. 點擊 __首頁__ 選擇 __編輯__ 從頂部操作欄
1. 選擇 __編輯__ 從頁面編輯器右上角的編輯模式選擇器
1. 將滑鼠懸停在WKND徽標下方和冒險清單上方的預設標題文本上，直到藍色編輯大綱顯示
1. 點擊以顯示元件的操作欄，然後點擊 __扳__  編輯

   ![標題元件操作欄](./assets/spa-fixed-component/title-action-bar.png)

1. 編寫「標題」元件：
   + 標題： __WKND冒險__
   + 類型/大小： __H2__

      ![標題元件對話框](./assets/spa-fixed-component/title-dialog.png)

1. 點擊 __完成__ 保存
1. 在編輯器中預AEM覽更SPA改
1. 刷新本地運行的WKND應用 [http://localhost:3000](http://localhost:3000) 並立即查看所創作的標題更改。

   ![標題元件SPA](./assets/spa-fixed-component/title-final.png)

## 恭喜！

您已向WKND應用添加了一個固定的可編輯元件！ 您現在知道如何：

+ 已為
+ 在中建立固定組AEM件
+ 查看遠程中的創作內SPA容

## 後續步驟

下一步是 [添加AEMResponsedGrid容器元件](./spa-container-component.md) 允許SPA作者將和可編輯的元件添加到SPA!
