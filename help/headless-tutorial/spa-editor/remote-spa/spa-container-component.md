---
title: 將可編輯的React容器元件添加到遠SPA程
description: 瞭解如何將可編輯的容器元件添加SPA到遠程AEM中，以允許作者將元件拖放到其中。
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

[固定元件](./spa-fixed-component.md) 為創作內容提供一SPA些靈活性，但此方法是剛性的，需要開發人員定義可編輯內容的準確組合。 為支援作者建立出眾的體驗，SPA編輯器支援在中使用容器組SPA件。 容器元件允許作者將允許的元件拖放到容器中，並創作它們，就像在傳統的AEM Sites創作中一樣！

![可編輯的容器元件](./assets/spa-container-component/intro.png)

在本章中，我們將可編輯容器添加到主視圖中，使作者能夠直接使用中的可編輯React元件來撰寫和佈局豐富的內容體SPA驗。

## 更新WKND應用

要將容器元件添加到「首頁」視圖：

+ 導入AEMReact Editable元件 `ResponsiveGrid` 元件
+ 導入和註冊自定義可編輯的React元件（文本和影像），以用於ResponsedGrid元件

### 使用ResponsedGrid元件

要向「首頁」視圖添加可編輯區域：

1. 開啟並編輯 `react-app/src/components/Home.js`
1. 導入 `ResponsiveGrid` 元件 `@adobe/aem-react-editable-components` 並加到 `Home` 元件。
1. 在 `<ResponsiveGrid...>` 元件
   + `pagePath = '/content/wknd-app/us/en/home'`
   + `itemPath = 'root/responsivegrid'`

   這指示 `ResponsiveGrid` 從資源中檢索其內容的組AEM件：

   + `/content/wknd-app/us/en/home/jcr:content/root/responsivegrid`

   的 `itemPath` 映射到 `responsivegrid` 在 `Remote SPA Page` 模AEM板，並在從 `Remote SPA Page` 模AEM板。

   更新 `Home.js` 的 `<ResponsiveGrid...>` 元件。

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

的 `Home.js` 檔案應如下所示：

![Home.js](./assets/spa-container-component/home-js.png)

## 建立可編輯的元件

要充分利用編輯器中提供的靈活創作體驗容SPA器。 我們已經建立了可編輯的標題元件，但讓我們再做幾個，讓作者在新添加的ResponsiveGrid元件中使用可編輯的文本和影像元件。

新的可編輯「文本」(Text)和「影像」(Image)「反應」(React)元件是使用中顯示的可編輯元件定義模式建立的 [可編輯的固定元件](./spa-fixed-component.md)。

### 可編輯文本元件

1. 在IDESPA中開啟項目
1. 在以下位置建立React元件 `src/components/editable/core/Text.js`
1. 將以下代碼添加到 `Text.js`

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

1. 在以下位置建立可編輯的React元件 `src/components/editable/EditableText.js`
1. 將以下代碼添加到 `EditableText.js`

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

可編輯的Text元件實現應如下所示：

![可編輯文本元件](./assets/spa-container-component/text-js.png)

### 影像元件

1. 在IDESPA中開啟項目
1. 在以下位置建立React元件 `src/components/editable/core/Image.js`
1. 將以下代碼添加到 `Image.js`

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

1. 在以下位置建立可編輯的React元件 `src/components/editable/EditableImage.js`
1. 將以下代碼添加到 `EditableImage.js`

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


1. 建立SCSS檔案 `src/components/editable/EditableImage.scss` 提供自定義樣式 `EditableImage.scss`。 這些樣式針對可編輯的React元件的CSS類。
1. 將以下SCS添加到 `EditableImage.scss`

   ```css
   .cmp-image__image {
       margin: 1rem 0;
       width: 100%;
       border: 0;
    }
   ```

1. 導入 `EditableImage.scss` 在 `EditableImage.js`

   ```javascript
   ...
   import './EditableImage.scss';
   ...
   ```

可編輯的映像元件實現應如下所示：

![可編輯的影像元件](./assets/spa-container-component/image-js.png)


### 導入可編輯的元件

新建立的 `EditableText` 和 `EditableImage` 反應元件在中引SPA用，並基於返回的JSON動態實AEM例化。 要確保這些元件可供使用，請SPA在中為它們建立導入語句 `Home.js`

1. 在IDESPA中開啟項目
1. 開啟檔案 `src/Home.js`
1. 添加導入語句 `AEMText` 和 `AEMImage`

   ```javascript
   ...
   // The following need to be imported, so that MapTo is run for the components
   import EditableText from './editable/EditableText';
   import EditableImage from './editable/EditableImage';
   ...
   ```

結果應該是：

![Home.js](./assets/spa-container-component/home-js-imports.png)

如果這些進口 _不_ 添加 `EditableText` 和 `EditableImage` 代碼不被調用SPA，因此元件不會映射到提供的資源類型。

## 在中配置容AEM器

容AEM器元件使用策略指定其允許的元件。 這是使用編輯器時的SPA關鍵配置，AEM因為只有映射了元件對SPA應項的元件才可由SPA呈現。 確保僅允許我們為實施提供SPA的元件：

+ `EditableTitle` 映射到 `wknd-app/components/title`
+ `EditableText` 映射到 `wknd-app/components/text`
+ `EditableImage` 映射到 `wknd-app/components/image`

要配置遠程頁模SPA板的reponsivegrid容器，請執行以下操作：

1. 登錄到AEM作者
1. 導航到 __「工具」>「常規」>「模板」>「WKND應用」__
1. 編輯 __報告頁SPA面__

   ![響應性網格策略](./assets/spa-container-component/templates-remote-spa-page.png)

1. 選擇 __結構__ 在右上角的模式切換器中
1. 點擊以選擇 __佈局容器__
1. 點擊 __策略__ 表徵圖

   ![響應性網格策略](./assets/spa-container-component/templates-policies-action.png)

1. 右下 __允許的元件__ 頁籤，展開 __WKND應用 — 內容__
1. 確保僅選擇以下內容：
   + 影像
   + 文字
   + 標題

   ![遠程SPA頁](./assets/spa-container-component/templates-allowed-components.png)

1. 點擊 __完成__

## 在中創作容AEM器

更新後SPA嵌入 `<ResponsiveGrid...>`，三個可編輯的React元件的包裝(`EditableTitle`。 `EditableText`, `EditableImage`)，並且AEM使用匹配的模板策略進行更新，我們可以開始在容器元件中創作內容。

1. 登錄到AEM作者
1. 導航到 __站點> WKND應用__
1. 點擊 __首頁__ 選擇 __編輯__ 從頂部操作欄
   + 將顯示「Hello World」文本元件，因為在從「項目」原型生成項目時會自動添加AEM該元件
1. 選擇 __編輯__ 從頁面編輯器右上角的模式選擇器
1. 查找 __佈局容器__ 標題下的可編輯區域
1. 開啟 __頁面編輯器側欄__，然後選擇 __元件視圖__
1. 將以下元件拖到 __佈局容器__
   + 影像
   + 標題
1. 拖動元件以按以下順序重新排序：
   1. 標題
   1. 影像
   1. 文字
1. __作者__ 這樣 __標題__ 元件
   1. 按一下「Title（標題）」元件，然後按一下 __扳__ 表徵圖 __編輯__ 標題元件
   1. 添加以下文本：
      + 標題： __夏天來了，我們好好利用！__
      + 類型： __H1__
   1. 點擊 __完成__
1. __作者__ 這樣 __影像__ 元件
   1. 將影像從「影像」元件的「側」欄（切換到「資產」視圖後）拖入
   1. 按一下「Image（影像）」元件，然後按一下 __扳__ 表徵圖
   1. 檢查 __影像是裝飾性的__ 複選框
   1. 點擊 __完成__
1. __作者__ 這樣 __文本__ 元件
   1. 按一下「文本」(Text)元件，然後按一下 __扳__ 表徵圖
   1. 添加以下文本：
      + _現在，你可以在所有一週的冒險中獲得15%的收益，在所有2週或更長的冒險中獲得20%的收益！ 結帳時，添加促銷代碼SUMMERISCOMING以獲得折扣！_
   1. 點擊 __完成__

1. 您的元件現在已創作，但垂直堆疊。

   ![創作的元件](./assets/spa-container-component/authored-components.png)

使AEM用佈局模式可以調整元件的大小和佈局。

1. 切換到 __佈局模式__ 在右上角使用模式選擇器
1. __調整大小__ 影像和文本元件，使它們並排
   + __影像__ 元件應 __寬8列__
   + __文本__ 元件應 __寬__

   ![佈局元件](./assets/spa-container-component/layout-components.png)

1. __預覽__ 在頁面編輯器AEM中所做的更改
1. 刷新本地運行的WKND應用 [http://localhost:3000](http://localhost:3000) 查看已創作的更改！

   ![中的容器組SPA件](./assets/spa-container-component/localhost-final.png)


## 恭喜！

您已添加容器元件，允許作者將可編輯元件添加到WKND應用！ 您現在知道如何：

+ 使用「AEMReact Editable Component」（可編輯的元件） `ResponsiveGrid` 組SPA件
+ 建立和註冊可編輯的React元件（文本和影像），以便通過容SPA器元件在中使用
+ 配置遠程SPA頁模板以允許啟SPA用的元件
+ 將可編輯元件添加到容器元件
+ 編輯器中的作者和佈局SPA元件

## 後續步驟

下一步使用相同的技術 [將可編輯元件添加到「冒險詳細資訊」路由](./spa-dynamic-routes.md) 的上界SPA。
