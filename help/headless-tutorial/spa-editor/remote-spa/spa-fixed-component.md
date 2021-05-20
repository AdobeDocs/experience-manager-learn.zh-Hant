---
title: 將可編輯的固定元件添加到遠程SPA
description: 了解如何將可編輯的固定元件新增至遠端SPA。
topic: 無頭、SPA、開發
feature: SPA編輯器，核心元件， API，開發
role: Developer, Architect
level: Beginner
kt: 7634
thumbnail: kt-7634.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 1%

---


# 可編輯的固定元件

可編輯的React元件可以是「固定」元件，或硬式編碼至SPA檢視。 這可讓開發人員將SPA Editor相容的元件放入SPA檢視中，並讓使用者在AEM SPA Editor中製作元件的內容。

![固定元件](./assets/spa-fixed-component/intro.png)

在本章中，我們將首頁檢視的標題「Current Adventures」（目前歷險）取代為固定但可編輯的標題元件，此元件為`Home.js`中的硬式編碼文字。 固定元件可保證標題的放置，但也允許編寫標題的文字，並在開發週期之外進行變更。

## 更新WKND應用程式

要將「固定」元件添加到「首頁」視圖：

+ 匯入AEM React核心元件標題元件並註冊至專案標題的資源類型
+ 將可編輯的Title元件放置在SPA首頁檢視上

### 在AEM React核心元件的標題元件中匯入

在SPA首頁檢視中，將硬式編碼文字`<h2>Current Adventures</h2>`取代為AEM React核心元件的標題元件。 必須先執行下列動作，才能使用Title元件：

1. 從`@adobe/aem-core-components-react-base`導入Title元件
1. 使用`withMappable`註冊，讓開發人員可將其放入SPA
1. 此外，請註冊`MapTo`，以便以後在[容器元件中使用](./spa-container-component.md)。

要執行此操作：

1. 在IDE的`~/Code/wknd-app/aem-guides-wknd-graphql/react-app`處開啟遠程SPA項目
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

閱讀程式碼的註解，了解實作詳細資訊。

`AEMTitle.js`檔案應該如下所示：

![AEMTitle.js](./assets/spa-fixed-component/aem-title-js.png)

### 使用React AEMTitle元件

現在AEM React核心元件的標題元件已在中註冊，且可在React應用程式中使用，請取代首頁檢視上的硬式編碼標題文字。

1. 編輯 `react-app/src/App.js`
1. 在底部的`Home()`中，以新的`AEMTitle`元件取代硬式編碼標題：

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

## 在AEM中製作標題元件

1. 登入AEM作者
1. 導覽至&#x200B;__Sites > WKND App__
1. 點選&#x200B;__Home__&#x200B;並從頂端動作列選取&#x200B;__Edit__
1. 從頁面編輯器右上角的編輯模式選擇器中選擇&#x200B;__Edit__
1. 將滑鼠移到WKND標誌下方和歷險清單上方的預設標題文字上，直到顯示藍色的編輯大綱
1. 點選以顯示元件的動作列，然後點選&#x200B;__扳手__&#x200B;以編輯

   ![標題元件動作列](./assets/spa-fixed-component/title-action-bar.png)

1. 製作標題元件：
   + 標題：__WKND冒險__
   + 類型/大小：__H2__

      ![標題元件對話方塊](./assets/spa-fixed-component/title-dialog.png)

1. 點選&#x200B;__Done__&#x200B;以儲存
1. 在AEM SPA編輯器中預覽變更
1. 重新整理本機在[http://localhost:3000](http://localhost:3000)上執行的WKND應用程式，並立即查看所撰寫的標題變更。

   ![SPA中的標題元件](./assets/spa-fixed-component/title-final.png)

## 恭喜！

您已將固定的可編輯元件新增至WKND應用程式！ 您現在知道如何：

+ 在SPA中匯入AEM React核心元件並加以重複使用
+ 將已修正但可編輯的元件新增至SPA
+ 在AEM中製作固定元件
+ 在遠端SPA中查看製作內容

## 後續步驟

接下來的步驟是將[AEM ResponsiveGrid容器元件](./spa-container-component.md)新增至SPA，讓作者可將可編輯的元件新增至SPA!
