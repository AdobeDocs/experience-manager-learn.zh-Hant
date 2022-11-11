---
title: 將可編輯的React容器元件新增至遠端SPA
description: 了解如何將可編輯的容器元件新增至遠端SPA，讓AEM作者將元件拖放至其中。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7635
thumbnail: kt-7635.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
exl-id: e5e6204c-d88c-4e79-a7f4-0cfc140bc51c
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 1%

---

# 可編輯的容器元件

[固定元件](./spa-fixed-component.md) 為編寫SPA內容提供了一些彈性，但此方法很剛性，需要開發人員定義可編輯內容的確切組成。 為了支援作者建立優異的體驗，SPA Editor支援在SPA中使用容器元件。 容器元件可讓作者將允許的元件拖放至容器中，並像傳統AEM Sites編寫中一樣加以編寫！

![可編輯的容器元件](./assets/spa-container-component/intro.png)

在本章中，我們將可編輯的容器新增至首頁檢視，讓作者可以直接在SPA中使用可編輯的React元件來撰寫和配置豐富的內容體驗。

## 更新WKND應用程式

要向「首頁」視圖添加容器元件：

+ 匯入AEM React可編輯元件的 `ResponsiveGrid` 元件
+ 匯入並註冊自訂可編輯的React元件（文字和影像），以用於ResponsiveGrid元件

### 使用ResponsiveGrid元件

要向「首頁」視圖添加可編輯區域，請執行以下操作：

1. 開啟和編輯 `react-app/src/components/Home.js`
1. 匯入 `ResponsiveGrid` 元件 `@adobe/aem-react-editable-components` 並加入 `Home` 元件。
1. 在 `<ResponsiveGrid...>` 元件
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   這會指示 `ResponsiveGrid` 從AEM資源擷取其內容的元件：

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   此 `itemPath` 映射至 `responsivegrid` 在中定義的節點 `Remote SPA Page` AEM範本和會自動建立在從 `Remote SPA Page` AEM範本。

   更新 `Home.js` 若要新增 `<ResponsiveGrid...>` 元件。

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

此 `Home.js` 檔案看起來應該像這樣：

![Home.js](./assets/spa-container-component/home-js.png)

## 建立可編輯的元件

若要充分運用SPA編輯器中提供的彈性製作體驗容器。 我們已建立可編輯的標題元件，但我們將再做幾項，讓作者在新新增的ResponsiveGrid元件中使用可編輯的文字和影像元件。

新的可編輯Text和Image React元件是使用 [可編輯的固定元件](./spa-fixed-component.md).

### 可編輯的文字元件

1. 在IDE中開啟SPA專案
1. 在建立React元件 `src/components/editable/core/Text.js`
1. 將下列程式碼新增至 `Text.js`

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

1. 在建立可編輯的React元件 `src/components/editable/EditableText.js`
1. 將下列程式碼新增至 `EditableText.js`

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

可編輯的文字元件實作看起來應該類似：

![可編輯的文字元件](./assets/spa-container-component/text-js.png)

### 影像元件

1. 在IDE中開啟SPA專案
1. 在建立React元件 `src/components/editable/core/Image.js`
1. 將下列程式碼新增至 `Image.js`

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

1. 在建立可編輯的React元件 `src/components/editable/EditableImage.js`
1. 將下列程式碼新增至 `EditableImage.js`

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


1. 建立SCSS檔案 `src/components/editable/EditableImage.scss` 可為 `EditableImage.scss`. 這些樣式以可編輯的React元件的CSS類別為目標。
1. 將以下SCSS添加到 `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 匯入 `EditableImage.scss` in `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

可編輯的影像元件實作看起來應該類似：

![可編輯的影像元件](./assets/spa-container-component/image-js.png)


### 匯入可編輯的元件

新建立的 `EditableText` 和 `EditableImage` SPA會參考React元件，並根據AEM傳回的JSON以動態方式具現化。 若要確保SPA可使用這些元件，請在 `Home.js`

1. 在IDE中開啟SPA專案
1. 開啟檔案 `src/Home.js`
1. 為添加導入語句 `AEMText` 和 `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

結果應該如下：

![Home.js](./assets/spa-container-component/home-js-imports.png)

如果這些匯入 _not_ 新增： `EditableText` 和 `EditableImage` SPA不會叫用程式碼，因此元件不會對應至提供的資源類型。

## 在AEM中設定容器

AEM容器元件使用原則指定其允許的元件。 使用SPA編輯器時，這是重要的設定，因為SPA只會轉譯已對應SPA元件的AEM元件。 確保僅允許我們為SPA實作提供的元件：

+ `EditableTitle` 已對應至 `wknd-app/components/title`
+ `EditableText` 已對應至 `wknd-app/components/text`
+ `EditableImage` 已對應至 `wknd-app/components/image`

要配置遠程SPA頁面模板的Reponsivegrid容器：

1. 登入AEM作者
1. 導覽至 __工具>一般>範本> WKND應用程式__
1. 編輯 __報表SPA頁面__

   ![響應式網格策略](./assets/spa-container-component/templates-remote-spa-page.png)

1. 選擇 __結構__ 在右上角的模式切換器中
1. 點選以選取 __版面容器__
1. 點選 __原則__ 圖示

   ![響應式網格策略](./assets/spa-container-component/templates-policies-action.png)

1. 在右下 __允許的元件__ 標籤，展開 __WKND應用程式 — 內容__
1. 請確定僅選取下列項目：
   + 影像
   + 文字
   + 標題

   ![遠端SPA頁面](./assets/spa-container-component/templates-allowed-components.png)

1. 點選 __完成__

## 在AEM中編寫容器

更新SPA以內嵌 `<ResponsiveGrid...>`，包裝三個可編輯的React元件(`EditableTitle`, `EditableText`，和 `EditableImage`)，而AEM已更新為相符的範本原則，我們就可以開始在容器元件中編寫內容。

1. 登入AEM作者
1. 導覽至 __Sites > WKND應用程式__
1. 點選 __首頁__ 選取 __編輯__ 從頂端動作列
   + 隨即顯示「Hello World」文字元件，因為這是從AEM專案原型產生專案時自動新增的
1. 選擇 __編輯__ 從頁面編輯器右上方的模式選取器
1. 找出 __版面容器__ 標題下方的可編輯區域
1. 開啟 __頁面編輯器側邊列__，然後選取 __元件視圖__
1. 將下列元件拖曳至 __版面容器__
   + 影像
   + 標題
1. 拖曳元件以依下列順序重新排序：
   1. 標題
   1. 影像
   1. 文字
1. __作者__ the __標題__ 元件
   1. 點選「標題」元件，然後點選 __扳手__ 圖示 __編輯__ 標題元件
   1. 新增下列文字：
      + 標題： __夏天就要來了，讓我們好好利用！__
      + 類型： __H1__
   1. 點選 __完成__
1. __作者__ the __影像__ 元件
   1. 從影像元件的側邊列（切換至「資產」檢視後）拖曳影像進入
   1. 點選「影像」元件，然後點選 __扳手__ 編輯
   1. 檢查 __影像是裝飾__ 核取方塊
   1. 點選 __完成__
1. __作者__ the __文字__ 元件
   1. 點選「文字」元件，然後點選「 」，編輯「文字」元件 __扳手__ 圖示
   1. 新增下列文字：
      + _現在，在所有1週的冒險中，你可以獲得15%的回報，在所有2週或更長的冒險中，你可以獲得20%的回報！ 結帳時，新增促銷活動代碼SUMMERISCOMING以取得折扣！_
   1. 點選 __完成__

1. 您的元件現已撰寫完成，但垂直堆疊。

   ![製作的元件](./assets/spa-container-component/authored-components.png)

使用「AEM版面模式」可讓我們調整元件的大小和版面。

1. 切換至 __版面模式__ 使用右上角的模式選取器
1. __調整大小__ 影像和文字元件，使它們並排
   + __影像__ 元件應 __寬8欄__
   + __文字__ 元件應 __寬3欄__

   ![版面元件](./assets/spa-container-component/layout-components.png)

1. __預覽__ AEM頁面編輯器中的變更
1. 重新整理在本機執行的WKND應用程式 [http://localhost:3000](http://localhost:3000) 來查看所撰寫的變更！

   ![SPA中的容器元件](./assets/spa-container-component/localhost-final.png)


## 恭喜！

您已新增容器元件，讓作者可將可編輯的元件新增至WKND應用程式！ 您現在知道如何：

+ 使用AEM React可編輯元件的 `ResponsiveGrid` 元件(在SPA中)
+ 建立並註冊可編輯的React元件（文字和影像），以透過容器元件用於SPA
+ 設定「遠端SPA頁面」範本，以允許啟用SPA的元件
+ 將可編輯的元件新增至容器元件
+ 在SPA編輯器中製作和配置元件

## 後續步驟

下一步會使用相同的技術 [將可編輯的元件添加到冒險詳細資訊路徑](./spa-dynamic-routes.md) 在SPA中。
