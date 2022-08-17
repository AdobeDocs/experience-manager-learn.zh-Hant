---
title: 將可編輯的固定元件添加到遠SPA程
description: 瞭解如何將可編輯的固定元件添加到遠SPA程。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
exl-id: edd18f2f-6f24-4299-a31a-54ccc4f6d86e
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---

# 可編輯的固定元件

可編輯的React元件可以是「固定」的，也可以硬編碼到視SPA圖中。 這允許開發人員將SPA與Editor相容的元件放SPA入視圖中，並允許用戶在編輯器中建立元件AEM的SPA內容。

![固定元件](./assets/spa-fixed-component/intro.png)

在本章中，我們將替換「首頁」視圖的標題「當前冒險」，該標題是 `Home.js` 具有固定但可編輯的標題元件。 固定元件可保證標題的放置，但也允許創作標題的文本，並在開發週期之外進行更改。

## 更新WKND應用

添加 __固定__ 「首頁」視圖的元件：

+ 導入AEMReact Core Component Title元件並將其註冊到項目Title的資源類型
+ 將可編輯的「標題」元件置SPA於「首頁」視圖

### 在反應核AEM心元件的標題元件中導入

在「主SPA頁」視圖中，替換硬編碼文本 `<h2>Current Adventures</h2>` 與「AEMReact Core Components」的「Title」（標題）元件一起使用。 在使用「標題」元件之前，我們必須：

1. 從導入標題元件 `@adobe/aem-core-components-react-base`
1. 使用 `withMappable` 這樣開發商就能把它放SPA在
1. 另外，註冊 `MapTo` 這樣它就可以 [容器元件](./spa-container-component.md)。

要執行此操作：

1. 開啟遠SPA程項目 `~/Code/wknd-app/aem-guides-wknd-graphql/react-app` 在IDE中
1. 在以下位置建立React元件 `react-app/src/components/aem/AEMTitle.js`
1. 將以下代碼添加到 `AEMTitle.js`。

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

閱讀代碼的注釋，瞭解實施詳細資訊。

的 `AEMTitle.js` 檔案應如下所示：

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### 使用React AEMTitle元件

現在，AEM React核心元件的標題元件已註冊並可在React應用中使用，請替換「首頁」視圖上的硬編碼標題文本。

1. 編輯 `react-app/src/Home.js`
1. 在 `Home()` 在底部，將硬編碼標題替換為 `AEMTitle` 元件：

   ```
   <h2>Current Adventures</h2>
   ```

   替換為

   ```
   <AEMTitle
       pagePath='/content/wknd-app/us/en/home' 
       itemPath='root/title'/>
   ```

   更新 `Home.js` 代碼：

   ```
   ...
   import { AEMTitle } from './aem/AEMTitle';
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

的 `Home.js` 檔案應如下所示：

![Home.js](./assets/spa-fixed-component/home-js.png)

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

+ 從中導入並重AEM新使用React Core組SPA件
+ 將固定但可編輯的元件添加到SPA
+ 在中建立固定組AEM件
+ 查看遠程中的創作內SPA容

## 後續步驟

下一步是 [添加AEMResponsedGrid容器元件](./spa-container-component.md) 允許SPA作者將和可編輯的元件添加到SPA!
