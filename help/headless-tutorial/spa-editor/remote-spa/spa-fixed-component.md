---
title: 將可編輯的固定元件添加到遠程SPA設備
description: 瞭解如何將可編輯的固定元件新增至遠端SPA。
topic: 無頭、SPA開發
feature: 編SPA輯器、核心元件、API、開發
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 1%

---


# 可編輯的固定元件

可編輯的React元件可以是「固定」的，或硬式編碼到檢SPA視中。 這可讓開發人員將SPAEditor相容的元件放入檢視SPA中，並讓使用者在Editor中製作元件AEM的SPA內容。

![固定元件](./assets/spa-fixed-component/intro.png)

在本章中，我們將「首頁」檢視的標題「目前的冒險」（`Home.js`中的硬式編碼文字）取代為固定但可編輯的「標題」元件。 固定元件可確保標題的放置，但也允許編寫標題的文字，並在開發週期之外進行變更。

## 更新WKND應用程式

要將「已修復」元件添加到「首頁」視圖，請執行以下操作：

+ 匯入AEMReact Core Component Title元件，並將其註冊到項目的Title資源類型
+ 將可編輯的Title元件置於「首頁」SPA檢視

### 在React CoreAEM Component的Title元件中導入

在「SPAHome」（首頁）視圖中，將硬式編碼文本`<h2>Current Adventures</h2>`替換為AEM React Core Components的Title（標題）元件。 在使用Title元件之前，我們必須：

1. 從`@adobe/aem-core-components-react-base`匯入Title元件
1. 使用`withMappable`進行註冊，讓開發人員將它置於
1. 此外，請註冊`MapTo`，以便在[容器元件後](./spa-container-component.md)中使用。

要執行此操作：

1. 在IDESPA的`~/Code/wknd-app/aem-guides-wknd-graphql/react-app`處開啟遠程項目
1. 在`react-app/src/components/aem/AEMTitle.js`建立React元件
1. 將下列程式碼新增至`AEMTitle.js`。

   ```
   // Import the withMappable API provided by the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the AEM React Core Components' Title component implementation and it's Empty Function 
   import { TitleV2, TitleV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wknd-app/components/title";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {    
       emptyLabel: "Title",  // The component placeholder in AEM SPA Editor
       isEmpty: TitleV2IsEmptyFn, // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE // The sling:resourceType this component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(TitleV2, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMTitle .../>
   const AEMTitle = withMappable(TitleV2, EditConfig);
   
   export default AEMTitle;
   ```

閱讀程式碼的注釋，以取得實作詳細資訊。

`AEMTitle.js`檔案應該如下所示：

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### 使用React AEMTitle元件

現在，AEM React Core Component的Title元件已註冊並可在React應用程式中使用，請取代「首頁」檢視上的硬式編碼標題文字。

1. 編輯 `react-app/src/App.js`
1. 在底部的`Home()`中，將硬式編碼標題取代為新的`AEMTitle`元件：

   ```
   <h2>Current Adventures</h2>
   ```

   替換為

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   使用下列程式碼更新`Apps.js`:

   ```
   ...
   import { AEMTitle } from './components/aem/AEMTitle';
   ...
   function Home() {
       return (
           <div className="Home">
   
               <AEMTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

`Apps.js`檔案應該如下所示：

![App.js](./assets/spa-fixed-component/app-js.png)

## 在中編寫Title元AEM件

1. 登入AEM作者
1. 導覽至「__網站> WKND應用程式__」
1. 點選&#x200B;__首頁__&#x200B;並從頂端動作列選擇&#x200B;__編輯__
1. 從頁面編輯器右上角的編輯模式選擇器中選擇&#x200B;__編輯__
1. 將滑鼠指標暫留在WKND標誌下方和探險清單上方的預設標題文字上，直到藍色編輯外框顯示
1. 點選以顯示元件的動作列，然後點選&#x200B;__扳手__&#x200B;以進行編輯

   ![標題元件動作列](./assets/spa-fixed-component/title-action-bar.png)

1. 編寫標題元件：
   + 標題：__WKND冒險__
   + 類型／大小：__H2__

      ![標題元件對話方塊](./assets/spa-fixed-component/title-dialog.png)

1. 點選&#x200B;__Done__&#x200B;以儲存
1. 在編輯器中預AEM覽變SPA更
1. 重新整理在[http://localhost:3000](http://localhost:3000)本機執行的WKND應用程式，並立即看到撰寫的標題變更。

   ![Title component in SPA](./assets/spa-fixed-component/title-final.png)

## 恭喜！

您已將固定、可編輯的元件新增至WKND應用程式！ 您現在知道如何：

+ 從中導入並重AEM復使用React Core Component SPA
+ 新增固定但可編輯的元件至
+ 在中編寫固定元AEM件
+ 請參閱遠端

## 後續步驟

接下來的步驟是將[新增AEMResponsiveGrid容器元件](./spa-container-component.md)至SPA，讓作者將可編輯的元件新增至SPA!
