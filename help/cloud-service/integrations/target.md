---
title: 整合AEM Headless和Target
description: 瞭解如何整合AEM Headless和Adobe Target，以使用Experience Platform Web SDK個人化Headless體驗。
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Headlessas a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1549
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 0%

---

# 整合AEM Headless和Target

瞭解如何將AEM內容片段匯出至Adobe Target，以將AEM Headless與Adobe Target整合，並使用Adobe Experience Platform Web SDK的alloy.js來使用它們個人化Headless體驗。 [React WKND應用程式](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html)用於探索如何使用內容片段選件來將個人化Target活動新增到體驗中，以促進WKND冒險。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

本教學課程涵蓋設定AEM和Adobe Target的相關步驟：

1. [在AEM Author中建立Adobe Target的Adobe IMS設定](#adobe-ims-configuration)
2. 在AEM作者中[建立Adobe TargetCloud Service](#adobe-target-cloud-service)
3. [在AEM Author中將Adobe TargetCloud Service套用至AEM Assets資料夾](#configure-asset-folders)
4. 在Adobe Admin Console中[許可權Adobe TargetCloud Service](#permission)
5. [從AEM作者將內容片段](#export-content-fragments)匯出至Target
6. [使用Adobe Target中的內容片段選件](#activity)建立活動
7. [在Experience Platform中建立Experience Platform資料流](#datastream-id)
8. [使用AdobeWeb SDK將個人化整合至以React為基礎的AEM Headless應用程式](#code)。

## Adobe IMS設定{#adobe-ims-configuration}

Adobe IMS設定可促進AEM與Adobe Target之間的驗證。

如需如何建立Adobe IMS設定的逐步指示，請參閱[檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html)。

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe TargetCloud Service{#adobe-target-cloud-service}

在AEM中建立Adobe TargetCloud Service，以方便將內容片段匯出至Adobe Target。

如需如何建立Adobe TargetCloud Service的逐步指示，請參閱[檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 設定資產資料夾{#configure-asset-folders}

在內容感知設定中設定的Adobe TargetCloud Service，必須套用至包含要匯出至Adobe Target的內容片段的AEM Assets資料夾階層。

+++展開以取得逐步指示

1. 以DAM管理員身分登入&#x200B;__AEM作者服務__
1. 導覽至&#x200B;__Assets >檔案__，找出已套用`/conf`的資產資料夾
1. 選取資產資料夾，並從上方動作列選取&#x200B;__屬性__
1. 選取&#x200B;__Cloud Service__&#x200B;索引標籤
1. 請確定雲端設定已設為包含Adobe Target Cloud Service設定的內容感知設定(`/conf`)。
1. 從&#x200B;__Cloud Service設定__&#x200B;下拉式清單中選取&#x200B;__Adobe Target__。
1. 選取右上方的&#x200B;__儲存並關閉__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## 許可AEM Target整合{#permission}

Adobe Target整合(顯示為developer.adobe.com專案)必須授予Adobe Admin Console中的&#x200B;__編輯者__&#x200B;產品角色，才能將內容片段匯出至Adobe Target。

+++展開以取得逐步指示

1. 以可在Adobe Admin Console中管理Adobe Target產品的使用者身分登入Experience Cloud
1. 開啟[Adobe Admin Console](https://adminconsole.adobe.com)
1. 選取&#x200B;__產品__，然後開啟&#x200B;__Adobe Target__
1. 在&#x200B;__產品設定檔__&#x200B;索引標籤上，選取&#x200B;__*預設工作區*__
1. 選取&#x200B;__API認證__&#x200B;標籤
1. 在此清單中找出您的developer.adobe.com應用程式，並將其&#x200B;__產品角色__&#x200B;設定為&#x200B;__編輯者__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## 將內容片段匯出至目標{#export-content-fragments}

存在於[已設定的AEM Assets資料夾階層](#apply-adobe-target-cloud-service-to-aem-assets-folders)下的內容片段，可以作為內容片段選件匯出至Adobe Target。 這些內容片段選件（Target中的特殊形式JSON選件）可用於Target活動，在Headless應用程式中提供個人化體驗。

+++展開以取得逐步指示

1. 以DAM使用者身分登入&#x200B;__AEM作者__
1. 導覽至「__Assets >檔案__」，並在「啟用Adobe Target」資料夾下找出要匯出為JSON至Target的內容片段
1. 選取要匯出至Adobe Target的內容片段
1. 在頂端動作列中選取&#x200B;__匯出至Adobe Target選件__
   + 此動作會將內容片段的完全水合JSON表示法匯出至Adobe Target，做為「內容片段選件」
   + 可以在AEM中檢閱完全水合的JSON表示法
      + 選取內容片段
      + 展開側面板
      + 在左側面板中選取&#x200B;__預覽__&#x200B;圖示
      + 匯出至Adobe Target的JSON表示會顯示在主檢視中
1. 以Adobe Target編輯器角色的使用者登入[Adobe Experience Cloud](https://experience.adobe.com)
1. 從[Experience Cloud](https://experience.adobe.com)，從右上方的產品切換器中選取&#x200B;__目標__&#x200B;以開啟Adobe Target。
1. 確定已在右上方的&#x200B;__Workspace切換器__&#x200B;中選取預設Workspace。
1. 選取頂端導覽列中的&#x200B;__選件__&#x200B;索引標籤
1. 選取&#x200B;__型別__&#x200B;下拉式清單，並選取&#x200B;__內容片段__
1. 驗證從AEM匯出的內容片段是否出現在清單中
   + 將滑鼠停留在選件上，並選取&#x200B;__檢視__&#x200B;按鈕
   + 檢閱&#x200B;__選件資訊__&#x200B;並檢視直接在AEM Author服務中開啟內容片段的&#x200B;__AEM深層連結__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 使用內容片段選件的Target活動{#activity}

在Adobe Target中，可建立使用內容片段選件JSON作為內容的活動，允許在Headless應用程式中使用在AEM中建立和管理內容的個人化體驗。

在此範例中，我們使用簡單的A/B活動，但可使用任何Target活動。

+++展開以取得逐步指示

1. 選取頂端導覽列中的&#x200B;__活動__&#x200B;索引標籤
1. 選取&#x200B;__+建立活動__，然後選取要建立的活動型別。
   + 此範例會建立簡單的&#x200B;__A/B測試__，但內容片段選件可支援任何活動型別
1. 在&#x200B;__建立活動__&#x200B;精靈中
   + 選取&#x200B;__網頁__
   + 在&#x200B;__選擇體驗撰寫器__&#x200B;中，選取&#x200B;__表單__
   + 在&#x200B;__選擇Workspace__&#x200B;中選取&#x200B;__預設Workspace__
   + 在&#x200B;__選擇屬性__&#x200B;中，選取活動可用的屬性，或選取&#x200B;__無屬性限制__&#x200B;以允許在所有屬性中使用。
   + 選取&#x200B;__下一步__&#x200B;以建立活動
1. 選取左上方的&#x200B;__重新命名__，以重新命名活動
   + 為活動提供有意義的名稱
1. 在初始體驗中，設定活動目標的&#x200B;__位置1__
   + 在此範例中，將目標定位為名為`wknd-adventure-promo`的自訂位置
1. 在「__內容__」下選取預設內容，然後選取「__變更內容片段__」
1. 選取要用於此體驗的匯出內容片段，並選取&#x200B;__完成__
1. 檢閱內容文字區域中的內容片段選件JSON，這是透過內容片段的預覽動作在AEM作者服務中提供的相同JSON。
1. 在左側欄中新增體驗，並選取要提供的不同內容片段選件
1. 選取&#x200B;__下一步__，並依活動需求設定鎖定目標規則
   + 在此範例中，請將A/B測試保留為手動50/50分割。
1. 選取&#x200B;__下一步__，並完成活動設定
1. 選取&#x200B;__儲存並關閉__，並為它指定有意義的名稱
1. 在Adobe Target的「活動」中，從右上角的「非使用中/啟動/封存」下拉式清單中選取「__啟動__」。

現在可以在AEM Headless應用程式中整合併公開鎖定在`wknd-adventure-promo`位置的Adobe Target活動。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform資料串流ID{#datastream-id}

AEM Headless應用程式需要[Adobe Experience Platform資料流](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) ID，才能使用[AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html)與Adobe Target互動。

+++展開以取得逐步指示

1. 導覽至[Adobe Experience Cloud](https://experience.adobe.com/)
1. 開啟&#x200B;__Experience Platform__
1. 選取&#x200B;__資料收集>資料串流__&#x200B;並選取&#x200B;__新增資料串流__
1. 在「新增資料流」精靈中，輸入：
   + 名稱：`AEM Target integration`
   + 描述： `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 事件結構描述： `Leave blank`
1. 選取&#x200B;__儲存__
1. 選取&#x200B;__新增服務__
1. 在&#x200B;__服務__&#x200B;中選取&#x200B;__Adobe Target__
   + 已啟用： __是__
   + 屬性Token： __留空__
   + 目標環境ID： __保留空白__
      + 可在Adobe Target中的&#x200B;__管理>主機__&#x200B;設定目標環境。
   + 目標協力廠商ID名稱空間： __保留空白__
1. 選取&#x200B;__儲存__
1. 在右側，複製&#x200B;__資料串流ID__&#x200B;以用於[AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html)設定呼叫。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## 將個人化新增至AEM Headless應用程式{#code}

本教學課程探討如何透過[Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)，使用Adobe Target中的內容片段選件，個人化簡單的React應用程式。 此方法可用於個人化任何以JavaScript為基礎的網頁體驗。

Android™和iOS行動體驗可使用[Adobe的行動SDK](https://developer.adobe.com/client-sdks/documentation/)依類似的模式進行個人化。

### 先決條件

+ Node.js 14
+ Git
+ [WKND Shared 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest)安裝在AEM as a Cloud Author和Publish服務上

### 設定

1. 從[Github.com](https://github.com/adobe/aem-guides-wknd-graphql)下載範例React應用程式的原始程式碼

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 在您最喜愛的IDE中，開啟位於`~/Code/aem-guides-wknd-graphql/personalization-tutorial`的程式碼基底
1. 更新您要應用程式連線至`~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`的AEM服務主機

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. 執行應用程式，並確保其連線至已設定的AEM服務。 從命令列，執行：

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. 將[AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package)安裝為NPM套件。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Web SDK可用於程式碼中，以依活動位置擷取內容片段選件JSON。

   設定Web SDK時，需要兩個ID：

   + `edgeConfigId`，即[資料流識別碼](#datastream-id)
   + `orgId`可在&#x200B;__Experience Cloud>設定檔>帳戶資訊>目前組織ID__&#x200B;找到的AEM as a Cloud Service/TargetAdobe組織ID

   叫用Web SDK時，Adobe Target活動位置（在我們的範例中，`wknd-adventure-promo`）必須設定為`decisionScopes`陣列中的值。

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 實作

1. 建立React元件`AdobeTargetActivity.js`以顯示Adobe Target活動。

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   使用叫用AdobeTargetActivity React元件如下：

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. 建立React元件`AdventurePromo.js`，以呈現JSON Adobe Target提供的冒險。

   此React元件採用代表冒險內容片段的完全水合的JSON，並以促銷方式顯示。 根據匯出至Adobe Target的內容片段，顯示從Adobe Target內容片段選件提供JSON服務的React元件可能視需要複雜多樣。

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   叫用此React元件的方式如下：

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. 將AdobeTargetActivity元件新增至React應用程式的`Home.js` （在冒險清單上方）。

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. 如果React應用程式未執行，請使用`npm run start`重新啟動。

   在兩種不同的瀏覽器中開啟React應用程式，以便讓A/B測試為各瀏覽器提供不同的體驗。 如果兩個瀏覽器都顯示相同的冒險選件，請嘗試關閉/重新開啟其中一個瀏覽器，直到顯示另一個體驗為止。

   下圖根據Adobe Target的邏輯，顯示`wknd-adventure-promo`活動的兩個不同內容片段選件。

   ![體驗選件](./assets/target/offers-in-app.png)

## 恭喜！

現在我們已將AEM as a Cloud Service設定為將內容片段匯出至Adobe Target、在Adobe Target活動中使用內容片段選件，並在AEM Headless應用程式中顯示該活動，將體驗個人化。
