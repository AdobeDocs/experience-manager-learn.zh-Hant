---
title: 將可編輯的容器元件新增至遠端SPA
description: 瞭解如何將可編輯的容器元件新增至遠端，讓SPA作AEM者將元件拖放至遠端。
topic: 無頭、SPA開發
feature: 編SPA輯器、核心元件、API、開發
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1178'
ht-degree: 1%

---


# 可編輯的容器元件

[固定](./spa-fixed-component.md) 元件可提供製作內容的彈性，SPA但此方法很僵化，需要開發人員定義可編輯內容的精確構成。為支援作者建立卓越的體驗，SPA Editor支援在中使用容器元SPA件。 容器元件可讓作者將允許的元件拖放至容器中，並像在傳統的AEM Sites製作中一樣來編寫元件！

![可編輯的容器元件](./assets/spa-container-component/intro.png)

在本章中，我們將在首頁檢視中新增可編輯的容器，讓作者直接在React Core Components中編寫和排版豐AEM富的內容體SPA驗。

## 更新WKND應用程式

若要將容器元件新增至「首頁」檢視：

+ 匯入AEMReact Editable元件的ResponsiveGrid元件
+ 匯入並注AEM冊React Core Components（文字和影像），以用於容器元件

### 在ResponsiveGrid容器元件中匯入

若要將可編輯區域放置到「首頁」(Home)視圖，我們必須：

1. 從`@adobe/aem-react-editable-components`匯入ResponsiveGrid元件
1. 使用`withMappable`進行註冊，讓開發人員將它置於
1. 此外，請註冊`MapTo`，以便在其他容器元件中重複使用，有效巢狀化容器。

要執行此操作：

1. 在IDESPA中開啟項目
1. 在`src/components/aem/AEMResponsiveGrid.js`建立React元件
1. 將下列程式碼新增至`AEMResponsiveGrid.js`

   ```
   // Import the withMappable API provided bu the AEM SPA Editor JS SDK
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   
   // Import the base ResponsiveGrid component
   import { ResponsiveGrid } from "@adobe/aem-react-editable-components";
   
   // The sling:resourceType for which this Core Component is registered with in AEM
   const RESOURCE_TYPE = "wcm/foundation/components/responsivegrid";
   
   // Create an EditConfig to allow the AEM SPA Editor to properly render the component in the Editor's context
   const EditConfig = {
       emptyLabel: "Layout Container",  // The component placeholder in AEM SPA Editor
       isEmpty: function(props) { 
           return props.cqItemsOrder == null || props.cqItemsOrder.length === 0;
       },                              // The function to determine if this component has been authored
       resourceType: RESOURCE_TYPE     // The sling:resourceType this SPA component is mapped to
   };
   
   // MapTo allows the AEM SPA Editor JS SDK to dynamically render components added to SPA Editor Containers
   MapTo(RESOURCE_TYPE)(ResponsiveGrid, EditConfig);
   
   // withMappable allows the component to be hardcoded into the SPA; <AEMResponsiveGrid .../>
   const AEMResponsiveGrid = withMappable(ResponsiveGrid, EditConfig);
   
   export default AEMResponsiveGrid;
   ```

程式碼與[匯入Reach Core Components&#39; Title元件](./spa-fixed-component.md)的程式碼類似。`AEMTitle.js`


`AEMResponsiveGrid.js`檔案應該如下所示：

![AEMResponsiveGrid.js](./assets/spa-container-component/aem-responsive-grid-js.png)

### 使用AEMResponsiveGrid組SPA件

現在，AEM ResponsiveGrid元件已註冊並可在中使用，SPA我們可將它置於「首頁」檢視中。

1. 開啟並編輯`react-app/src/App.js`
1. 匯入`AEMResponsiveGrid`元件，並將其置於`<AEMTitle ...>`元件上方。
1. 在`<AEMResponsiveGrid...>`元件上設定以下屬性
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   這將指示此`AEMResponsiveGrid`元件從資源中檢索其內AEM容：

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath`會映射至`Remote SPA Page`範本中定義的`responsivegrid`節點，並會在從`Remote SPA Page`範本建立的新AEM頁面上自動AEM建立。

   更新`App.js`以添加`<AEMResponsiveGrid...>`元件。

   ```
   ...
   import AEMResponsiveGrid from './components/aem/AEMResponsiveGrid';
   ...
   
   function Home() {
   return (
       <div className="Home">
           <AEMResponsiveGrid
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='root/responsivegrid'/>
   
           <AEMTitle
               pagePath='/content/wknd-app/us/en/home' 
               itemPath='title'/>
           <Adventures />
       </div>
   );
   }
   ```

`Apps.js`檔案應該如下所示：

![App.js](./assets/spa-container-component/app-js.png)

## 建立可編輯的元件

如要充份運用Editor中提供的彈性製作體驗容器SPA。 我們已建立可編輯的「標題」元件，但讓我們再做幾個，讓作者在新加入的容器元件中使用「文字」和「影像AEM WCM核心元件」。

### 文字元件

1. 在IDESPA中開啟項目
1. 在`src/components/aem/AEMText.js`建立React元件
1. 將下列程式碼新增至`AEMText.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { TextV2, TextV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {    
       emptyLabel: "Text",
       isEmpty: TextV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(TextV2, EditConfig);
   
   const AEMText = withMappable(TextV2, EditConfig);
   
   export default AEMText;
   ```

`AEMText.js`檔案應該如下所示：

![AEMText.js](./assets/spa-container-component/aem-text-js.png)

### 影像元件

1. 在IDESPA中開啟項目
1. 在`src/components/aem/AEMImage.js`建立React元件
1. 將下列程式碼新增至`AEMImage.js`

   ```
   import { withMappable, MapTo } from '@adobe/aem-react-editable-components';
   import { ImageV2, ImageV2IsEmptyFn } from "@adobe/aem-core-components-react-base";
   
   const RESOURCE_TYPE = "wknd-app/components/image";
   
   const EditConfig = {    
       emptyLabel: "Image",
       isEmpty: ImageV2IsEmptyFn,
       resourceType: RESOURCE_TYPE
   };
   
   MapTo(RESOURCE_TYPE)(ImageV2, EditConfig);
   
   const AEMImage = withMappable(ImageV2, EditConfig);
   
   export default AEMImage;
   ```

1. 建立SCSS檔案`src/components/aem/AEMImage.scss`，提供`AEMImage.scss`的自訂樣式。 這些樣式以AEM React Core Component的BEM-notation CSS類為目標。
1. 將以下SCSS添加到`AEMImage.scss`

   ```
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 在`AEMImage.js`中導入`AEMImage.scss`

   ```
   ...
   import './AEMImage.scss';
   ...
   ```

`AEMImage.js`和`AEMImage.scss`看起來應該如下：

![AEMImage.js和AEMImage.scss](./assets/spa-container-component/aem-image-js-scss.png)

### 匯入可編輯的元件

新建立的`AEMText`和`AEMImage`元SPA件在中引SPA用，並會根據傳回的JSON動態執行個體AEM化。 為確保這些元件可用於SPA，請在`App.js`中為它們建立import語句

1. 在IDESPA中開啟項目
1. 開啟檔案`src/App.js`
1. 添加`AEMText`和`AEMImage`的導入語句

   ```
   ...
   import AEMText from './components/aem/AEMText';
   import AEMImage from './components/aem/AEMImage';
   ...
   ```


結果應該如下：

![Apps.js](./assets/spa-container-component/app-js-imports.png)

如果這些導入是&#x200B;_not_&#x200B;添加的，則`AEMText`和`AEMImage`代碼將不被調用SPA，因此，元件不會根據提供的資源類型進行註冊。

## 在中設定容AEM器

容AEM器元件使用原則來指定其允許的元件。 這是使用編輯器時的重SPA要配置，因為只有AEM已映射元件對應項的SPAWCM核心元件才可由SPA渲染。 請僅確保我們為實施提供的SPA元件是允許的：

+ `AEMTitle` 映射至  `wknd-app/components/title`
+ `AEMText` 映射至  `wknd-app/components/text`
+ `AEMImage` 映射至  `wknd-app/components/image`

若要設定「遠端頁SPA面」範本的Reponsivegrid容器：

1. 登入AEM作者
1. 導覽至&#x200B;__工具>一般>範本> WKND應用程式__
1. 編輯&#x200B;__報SPA表頁面__

   ![自適應格線原則](./assets/spa-container-component/templates-remote-spa-page.png)

1. 在右上角的模式切換器中選擇&#x200B;__結構__
1. 點選以選取&#x200B;__版面容器__
1. 點選快顯列中的&#x200B;__Policy__&#x200B;圖示

   ![自適應格線原則](./assets/spa-container-component/templates-policies-action.png)

1. 在右側的「__允許的元件__」標籤下，展開&#x200B;__WKND APP - CONTENT__
1. 請確定僅選擇下列項目：
   + 影像
   + 文字
   + 標題

   ![遠端頁SPA面](./assets/spa-container-component/templates-allowed-components.png)

1. 點選&#x200B;__Done__

## 在中編寫容AEM器

更新SPA為內嵌`<AEMResponsiveGrid...>`、三個AEM React Core元件（`AEMTitle`、`AEMText`和`AEMImage`）的包裝函式，並以符合的範本原則進行更新AEM，我們可以開始在容器元件中編寫內容。

1. 登入AEM作者
1. 導覽至「__網站> WKND應用程式__」
1. 點選&#x200B;__首頁__&#x200B;並從頂端動作列選擇&#x200B;__編輯__
   + 會顯示「Hello World」文字元件，因為當從「專案」原型產生專案時，會自AEM動新增此元件
1. 從頁面編輯器右上角的模式選擇器中選擇&#x200B;__編輯__
1. 在「Title（標題）」下方找到「Layout Container（佈局容器）」__可編輯區域__
1. 開啟&#x200B;__頁面編輯器的側欄__，然後選擇&#x200B;__元件視圖__
1. 將下列元件拖曳至&#x200B;__版面容器__
   + 影像
   + 標題
1. 拖曳元件以依下列順序重新排序：
   1. 標題
   1. 影像
   1. 文字
1. __Authorthe__ Titlecomponent(Authorthe Titlecomponent) ____ 
   1. 點選Title元件，點選&#x200B;__wrench__&#x200B;圖示至&#x200B;__edit__ Title元件
   1. 新增下列文字：
      + 標題：__夏天就要來了，讓我們充分利用它！__
      + 類型：__H1__
   1. 點選&#x200B;__Done__
1. __Authorthe__ Imagecomponent(Authorthe  ____ Imagecomponent)
   1. 從影像元件的「側邊」列（切換至「資產」檢視後）拖曳影像
   1. 點選影像元件，點選&#x200B;__wrnch__&#x200B;圖示以進行編輯
   1. 選中&#x200B;__Image is decortial__&#x200B;複選框
   1. 點選&#x200B;__Done__
1. __Authorthe__ Textcomponent(Authorthe  ____ Textcomponent)
   1. 點選Text元件並點選&#x200B;__wrench__&#x200B;圖示，以編輯Text元件
   1. 新增下列文字：
      + _現在，您可以在1週的冒險中獲得15%的折扣，在2週或更長的冒險中獲得20%的折扣！結帳時，只要新增促銷活動程式碼SUMMERISCOMING即可享有折扣！_
   1. 點選&#x200B;__Done__

1. 您的元件現在已編寫，但會垂直堆疊。

   ![製作的元件](./assets/spa-container-component/authored-components.png)

   使用AEM「版面模式」可讓我們調整元件的大小和版面。

1. 使用右上角的模式選擇器切換至「版面模式」____
1. __調整__ 影像和文字元件的大小，使它們並排顯示
   + ____ Imagecomponent應為 __8欄寬__
   + ____ Textcomponent應為 __3欄寬__

   ![版面元件](./assets/spa-container-component/layout-components.png)

1. __在頁__ 面編輯器中預AEM覽變更
1. 重新整理在[http://localhost:3000](http://localhost:3000)本機執行的WKND應用程式，以檢視撰寫的變更！

   ![容器元SPA件](./assets/spa-container-component/localhost-final.png)


## 恭喜！

您已新增容器元件，可讓作者將可編輯的元件新增至WKND應用程式！ 您現在知道如何：

+ 使用AEMReact Editable元件的ResponsiveGrid元件(位於
+ 註冊AEMReact Core Components（文字和影像），以透過容器元SPA件在
+ 設定「遠端頁SPA面」範本以允許啟SPA用的核心元件
+ 將可編輯的元件新增至容器元件
+ 編輯器中的作者和版面元SPA件

## 後續步驟

下一步將使用此相同的技巧，將可編輯的元件新增至[中的「冒險詳細資訊」路由](./spa-dynamic-routes.md)SPA。
