---
title: 將可編輯的固定元件新增至遠端SPA
description: 瞭解如何將可編輯的固定元件新增至遠端SPA。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7634
thumbnail: kt-7634.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
duration: 164
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# 可編輯的固定元件

可編輯的React元件可以「固定」，或硬式編碼至SPA檢視中。 如此一來，開發人員就能將與SPA編輯器相容的元件置於SPA檢視中，且使用者能在AEM SPA編輯器中編寫元件的內容。

![固定元件](./assets/spa-fixed-component/intro.png)

在本章中，我們會以固定但可編輯的Title元件取代Home檢視的標題「Current Adventures」（在`Home.js`中為硬式編碼文字）。 固定元件可保證標題的位置，但也允許編寫標題的文字，並在開發週期之外進行變更。

## 更新WKND應用程式

若要將&#x200B;__Fixed__&#x200B;元件新增至Home檢視：

+ 建立自訂可編輯的標題元件並將其註冊到專案的標題資源型別
+ 將可編輯的標題元件放在SPA首頁檢視上

### 建立可編輯的React Title元件

在SPA首頁檢視中，以自訂的可編輯標題元件取代硬式編碼文字`<h2>Current Adventures</h2>`。 在使用標題元件之前，我們必須：

1. 建立自訂Title React元件
1. 使用來自`@adobe/aem-react-editable-components`的方法裝飾自訂Title元件，使其可編輯。
1. 透過`MapTo`註冊可編輯的Title元件，以便稍後在[容器元件](./spa-container-component.md)中使用。

若要這麼做：

1. 在IDE中的`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app`開啟遠端SPA專案
1. 在`react-app/src/components/editable/core/Title.js`建立React元件
1. 將下列程式碼新增至`Title.js`。

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

   請注意，此React元件目前無法使用AEM SPA編輯器進行編輯。 此基本元件將在下一個步驟中變為可編輯。

   閱讀程式碼的註釋，以瞭解實作詳細資訊。

1. 在`react-app/src/components/editable/EditableTitle.js`建立React元件
1. 將下列程式碼新增至`EditableTitle.js`。

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

   此`EditableTitle` React元件會包裝`Title` React元件，並將其包裝及裝飾為可在AEM SPA編輯器中編輯。

### 使用React EditableTitle元件

現在EditableTitle React元件已在中註冊並可在React應用程式中使用，請取代「首頁」檢視上的硬式編碼標題文字。

1. 編輯`react-app/src/components/Home.js`
1. 在底部的`Home()`中，匯入`EditableTitle`並以新的`AEMTitle`元件取代硬式編碼的標題：

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

`Home.js`檔案應該如下所示：

![Home.js](./assets/spa-fixed-component/home-js-update.png)

## 在AEM中編寫標題元件

1. 登入AEM Author
1. 導覽至&#x200B;__網站> WKND應用程式__
1. 點選&#x200B;__首頁__，然後從最上方的動作列選取&#x200B;__編輯__
1. 從「頁面編輯器」右上角的編輯模式選取器中選取&#x200B;__編輯__
1. 將游標暫留在WKND標誌下方和冒險清單上方的預設標題文字上，直到顯示藍色編輯外框為止
1. 點選以公開元件的動作列，然後點選&#x200B;__扳手__&#x200B;以進行編輯

   ![標題元件動作列](./assets/spa-fixed-component/title-action-bar.png)

1. 編寫標題元件：
   + 標題： __WKND冒險__
   + 型別/大小： __H2__

     ![標題元件對話方塊](./assets/spa-fixed-component/title-dialog.png)

1. 點選&#x200B;__完成__&#x200B;以儲存
1. 在AEM SPA Editor中預覽變更
1. 重新整理在[http://localhost:3000](http://localhost:3000)本機執行的WKND應用程式，並立即看到編寫的標題變更。

   SPA中的![標題元件](./assets/spa-fixed-component/title-final.png)

## 恭喜！

您已將固定、可編輯的元件新增至WKND應用程式！ 您現在知道如何：

+ 建立固定但可編輯的SPA元件
+ 在AEM中編寫固定元件
+ 檢視遠端SPA中的撰寫內容

## 後續步驟

接下來的步驟是[將AEM ResponsiveGrid容器元件](./spa-container-component.md)新增至SPA，讓作者將可編輯的元件新增至SPA！
