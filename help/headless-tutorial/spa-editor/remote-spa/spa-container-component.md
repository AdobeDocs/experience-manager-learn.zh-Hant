---
title: 將可編輯的React容器元件新增至遠端SPA
description: 瞭解如何將可編輯的容器元件新增至遠端SPA，讓AEM作者可將元件拖放至其中。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
duration: 306
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1112'
ht-degree: 0%

---

# 可編輯的容器元件

[固定元件](./spa-fixed-component.md)可為SPA內容的製作提供一些彈性，但這種方法很僵化，需要開發人員定義可編輯內容的確切構成。 為了支援作者建立卓越的體驗，SPA編輯器支援在SPA中使用容器元件。 容器元件可讓作者將允許的元件拖放至容器中並加以撰寫，就像在傳統AEM Sites製作中一樣！

![可編輯的容器元件](./assets/spa-container-component/intro.png)

在本章中，我們將可編輯的容器新增至首頁檢視，讓作者可直接在SPA中使用可編輯的React元件來撰寫和配置豐富的內容體驗。

## 更新WKND應用程式

若要將容器元件新增至「首頁」檢視：

+ 匯入AEM React Editable元件的`ResponsiveGrid`元件
+ 匯入並註冊自訂的可編輯React元件（文字和影像），以用於ResponsiveGrid元件

### 使用ResponsiveGrid元件

若要將可編輯區域新增至「首頁」檢視：

1. 開啟並編輯`react-app/src/components/Home.js`
1. 從`@adobe/aem-react-editable-components`匯入`ResponsiveGrid`元件，並將其新增至`Home`元件。
1. 在`<ResponsiveGrid...>`元件上設定下列屬性
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   這會指示`ResponsiveGrid`元件從AEM資源擷取其內容：

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   `itemPath`對應至`Remote SPA Page` AEM範本中定義的`responsivegrid`節點，且會在從`Remote SPA Page` AEM範本建立的新AEM頁面上自動建立。

   更新`Home.js`以新增`<ResponsiveGrid...>`元件。

   ```javascript
   ...
   import { ResponsiveGrid } from '@adobe/aem-react-editable-components';
   ...
   
   function Home() {
       return (
           <div className="Home">
               <ResponsiveGrid
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='root/responsivegrid'/>
   
               <EditableTitle
                   pagePath='/content/wknd-app/us/en/home' 
                   itemPath='title'/>
   
               <Adventures />
           </div>
       );
   }
   ```

`Home.js`檔案應該如下所示：

![Home.js](./assets/spa-container-component/home-js.png)

## 建立可編輯的元件

以充分發揮SPA編輯器中提供的彈性撰寫體驗容器的效果。 我們已建立可編輯的Title元件，但接下來讓我們介紹幾個專案，讓作者可在新新增的ResponsiveGrid元件中使用可編輯的文字和影像元件。

新的可編輯文字和影像React元件是使用在[可編輯的固定元件](./spa-fixed-component.md)中公開的可編輯元件定義模式所建立。

### 可編輯文字元件

1. 在IDE中開啟SPA專案
1. 在`src/components/editable/core/Text.js`建立React元件
1. 將下列程式碼新增至`Text.js`

   ```javascript
   import React from 'react'
   
   const TextPlain = (props) => <div className={props.baseCssClass}><p className="cmp-text__paragraph">{props.text}</p></div>;
   const TextRich = (props) => {
   const text = props.text;
   const id = (props.id) ? props.id : (props.cqPath ? props.cqPath.substr(props.cqPath.lastIndexOf('/') + 1) : "");
       return <div className={props.baseCssClass} id={id} data-rte-editelement dangerouslySetInnerHTML={{ __html: text }} />
   };
   
   export const Text = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-text'
       }
   
       const { richText = false } = props
   
       return richText ? <TextRich {...props} /> : <TextPlain {...props} />
       }
   
       export function textIsEmpty(props) {
       return props.text == null || props.text.length === 0;
   }
   ```

1. 在`src/components/editable/EditableText.js`建立可編輯的React元件
1. 將下列程式碼新增至`EditableText.js`

   ```javascript
   import React from 'react'
   import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
   import { Text, textIsEmpty } from "./core/Text";
   import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
   import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";
   
   const RESOURCE_TYPE = "wknd-app/components/text";
   
   const EditConfig = {
       emptyLabel: "Text",
       isEmpty: textIsEmpty,
       resourceType: RESOURCE_TYPE
   };
   
   export const WrappedText = (props) => {
       const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Text, "cmp-text"), textIsEmpty, "Text V2")
       return <Wrapped {...props} />
   };
   
   const EditableText = (props) => <EditableComponent config={EditConfig} {...props}><WrappedText /></EditableComponent>
   
   MapTo(RESOURCE_TYPE)(EditableText);
   
   export default EditableText;
   ```

可編輯的文字元件實作應如下所示：

![可編輯的文字元件](./assets/spa-container-component/text-js.png)

### 影像元件

1. 在IDE中開啟SPA專案
1. 在`src/components/editable/core/Image.js`建立React元件
1. 將下列程式碼新增至`Image.js`

   ```javascript
   import React from 'react'
   import { RoutedLink } from "./RoutedLink";
   
   export const imageIsEmpty = (props) => (!props.src) || props.src.trim().length === 0
   
   const ImageInnerContents = (props) => {
   return (<>
       <img src={props.src}
           className={props.baseCssClass + '__image'}
           alt={props.alt} />
       {
           !!(props.title) && <span className={props.baseCssClass + '__title'} itemProp="caption">{props.title}</span>
       }
       {
           props.displayPopupTitle && (!!props.title) && <meta itemProp="caption" content={props.title} />
       }
       </>);
   };
   
   const ImageContents = (props) => {
       if (props.link && props.link.trim().length > 0) {
           return (
           <RoutedLink className={props.baseCssClass + '__link'} isRouted={props.routed} to={props.link}>
               <ImageInnerContents {...props} />
           </RoutedLink>
           )
       }
       return <ImageInnerContents {...props} />
   };
   
   export const Image = (props) => {
       if (!props.baseCssClass) {
           props.baseCssClass = 'cmp-image'
       }
   
       const { isInEditor = false } = props;
       const cssClassName = (isInEditor) ? props.baseCssClass + ' cq-dd-image' : props.baseCssClass;
   
       return (
           <div className={cssClassName}>
               <ImageContents {...props} />
           </div>
       )
   };
   ```

1. 在`src/components/editable/EditableImage.js`建立可編輯的React元件
1. 將下列程式碼新增至`EditableImage.js`

```javascript
import { EditableComponent, MapTo } from '@adobe/aem-react-editable-components';
import { Image, imageIsEmpty } from "./core/Image";
import React from 'react'

import { withConditionalPlaceHolder } from "./core/util/withConditionalPlaceholder";
import { withStandardBaseCssClass } from "./core/util/withStandardBaseCssClass";

const RESOURCE_TYPE = "wknd-app/components/image";

const EditConfig = {
    emptyLabel: "Image",
    isEmpty: imageIsEmpty,
    resourceType: RESOURCE_TYPE
};

const WrappedImage = (props) => {
    const Wrapped = withConditionalPlaceHolder(withStandardBaseCssClass(Image, "cmp-image"), imageIsEmpty, "Image V2");
    return <Wrapped {...props}/>
}

const EditableImage = (props) => <EditableComponent config={EditConfig} {...props}><WrappedImage /></EditableComponent>

MapTo(RESOURCE_TYPE)(EditableImage);

export default EditableImage;
```


1. 建立提供`EditableImage.scss`自訂樣式的SCSS檔案`src/components/editable/EditableImage.scss`。 這些樣式以可編輯的React元件的CSS類別為目標。
1. 將下列SCSS新增至`EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 在`EditableImage.js`中匯入`EditableImage.scss`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

可編輯的影像元件實作應如下所示：

![可編輯的影像元件](./assets/spa-container-component/image-js.png)


### 匯入可編輯的元件

新建立的`EditableText`和`EditableImage` React元件已在SPA中參照，並根據AEM傳回的JSON動態具現化。 若要確保SPA可以使用這些元件，請在`Home.js`中為其建立匯入陳述式

1. 在IDE中開啟SPA專案
1. 開啟檔案`src/Home.js`
1. 新增`AEMText`和`AEMImage`的匯入陳述式

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

結果應如下所示：

![Home.js](./assets/spa-container-component/home-js-imports.png)

如果這些匯入專案是&#x200B;_未_&#x200B;新增，則SPA不會叫用`EditableText`和`EditableImage`程式碼，因此元件不會對應到提供的資源型別。

## 在AEM中設定容器

AEM容器元件會使用原則來指定其允許的元件。 這是使用SPA編輯器時的關鍵設定，因為SPA只能轉譯對應了SPA元件的AEM元件。 請確定僅允許我們提供SPA實施的元件：

+ `EditableTitle`對應至`wknd-app/components/title`
+ `EditableText`對應至`wknd-app/components/text`
+ `EditableImage`對應至`wknd-app/components/image`

若要設定「遠端SPA頁面」範本的responsivegrid容器：

1. 登入AEM Author
1. 導覽至&#x200B;__工具>一般>範本> WKND應用程式__
1. 編輯&#x200B;__報表SPA頁面__

   ![回應式格線原則](./assets/spa-container-component/templates-remote-spa-page.png)

1. 在右上角的模式切換器中選取&#x200B;__結構__
1. 點選以選取&#x200B;__配置容器__
1. 點選快顯列中的&#x200B;__原則__&#x200B;圖示

   ![回應式格線原則](./assets/spa-container-component/templates-policies-action.png)

1. 在右側的&#x200B;__允許的元件__&#x200B;標籤下，展開&#x200B;__WKND APP - CONTENT__
1. 請確定僅選取下列專案：
   + 影像
   + 文字
   + 標題

   ![遠端SPA頁面](./assets/spa-container-component/templates-allowed-components.png)

1. 點選&#x200B;__完成__

## 在AEM中編寫容器

SPA更新為內嵌`<ResponsiveGrid...>`、三個可編輯React元件（`EditableTitle`、`EditableText`和`EditableImage`）的包裝函式，以及AEM更新為相符的範本原則後，我們就可以開始編寫容器元件中的內容。

1. 登入AEM Author
1. 導覽至&#x200B;__網站> WKND應用程式__
1. 點選&#x200B;__首頁__，然後從最上方的動作列選取&#x200B;__編輯__
   + 「Hello World」文字元件隨即顯示，因為從AEM專案原型產生專案時會自動新增此元件
1. 從「頁面編輯器」右上角的模式選取器中選取「__編輯__」
1. 在標題下方找到&#x200B;__配置容器__&#x200B;可編輯區域
1. 開啟&#x200B;__頁面編輯器的側列__，然後選取&#x200B;__元件檢視__
1. 將下列元件拖曳至&#x200B;__配置容器__
   + 影像
   + 標題
1. 拖曳元件以將其重新排序為下列順序：
   1. 標題
   1. 影像
   1. 文字
1. __作者__ __標題__&#x200B;元件
   1. 點選「標題」元件，然後點選&#x200B;__扳手__&#x200B;圖示以&#x200B;__編輯__「標題」元件
   1. 新增下列文字：
      + 標題： __夏天即將到來，讓我們充分利用它！__
      + 型別： __H1__
   1. 點選&#x200B;__完成__
1. __作者__ __影像__&#x200B;元件
   1. 在影像元件上，從側邊欄(切換至Assets檢視後)將影像拖曳到中
   1. 點選「影像」元件，然後點選「__扳手__」圖示以進行編輯
   1. 勾選&#x200B;__影像為裝飾性__&#x200B;核取方塊
   1. 點選&#x200B;__完成__
1. __作者__ __文字__&#x200B;元件
   1. 點選「文字」元件，並點選&#x200B;__扳手__&#x200B;圖示以編輯「文字」元件
   1. 新增下列文字：
      + _現在，您可以在所有的一週冒險中獲得15%的折扣，在所有兩週或更長時間的冒險中獲得20%的折扣！ 結帳時，新增行銷活動代碼SUMMERISCOMING以取得折扣！_
   1. 點選&#x200B;__完成__

1. 您的元件現在已製作，但可垂直棧疊。

   ![編寫的元件](./assets/spa-container-component/authored-components.png)

使用「AEM配置模式」可讓我們調整元件的大小和配置。

1. 使用右上角的模式選擇器切換至&#x200B;__配置模式__
1. __調整影像和文字元件的大小__，讓它們並排
   + __影像__&#x200B;元件應為&#x200B;__8欄寬__
   + __文字__&#x200B;元件應為&#x200B;__3欄寬__

   ![配置元件](./assets/spa-container-component/layout-components.png)

1. 在AEM頁面編輯器中&#x200B;__預覽__&#x200B;您的變更
1. 重新整理在[http://localhost:3000](http://localhost:3000)本機執行的WKND應用程式以檢視編寫的變更！

   SPA](./assets/spa-container-component/localhost-final.png)中的![容器元件


## 恭喜！

您已新增容器元件，讓作者可將可編輯的元件新增到WKND應用程式！ 您現在知道如何：

+ 在SPA中使用AEM React Editable元件的`ResponsiveGrid`元件
+ 建立並註冊可編輯的React元件（文字和影像），以透過容器元件用於SPA
+ 設定遠端SPA頁面範本，以允許啟用SPA的元件
+ 將可編輯的元件新增至容器元件
+ 在SPA編輯器中製作和配置元件

## 後續步驟

下一個步驟使用相同的技巧，在SPA中將可編輯的元件[新增到冒險詳細資訊路由](./spa-dynamic-routes.md)。
