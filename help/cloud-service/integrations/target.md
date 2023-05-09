---
title: AEM無頭和Target個人化
description: 本教學課程探討AEM內容片段如何匯出至Adobe Target，然後使用AdobeWeb SDK個人化無頭體驗。
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
source-git-commit: b3cc9c4fbd36cdf5be46e4546a174fea0c8da05c
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 1%

---

# 使用內容片段個人化AEM無頭體驗

>[!IMPORTANT]
>
> Adobe Experience Manager內容片段匯出至Adobe Target的功能可在AEMas a Cloud Service中取得 [預發行管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=en#new-features).



本教學課程探討AEM內容片段如何匯出至Adobe Target，然後使用AdobeWeb SDK個人化無頭體驗。 此 [React WKND App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) 可用來探索如何使用內容片段選件將個人化Target活動新增至體驗，以推廣WKND冒險。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

本教學課程涵蓋設定AEM和Adobe Target的相關步驟：

1. [為Adobe Target建立Adobe IMS設定](#adobe-ims-configuration) 在AEM作者中
1. [建立Adobe TargetCloud Service](#adobe-target-cloud-service) 在AEM作者中
1. [將Adobe TargetCloud Service套用至AEM Assets資料夾](#configure-asset-folders) 在AEM作者中
1. [權限Adobe TargetCloud Service](#permission) 在Adobe Admin Console
1. [匯出內容片段](#export-content-fragments) 從AEM作者到Target
1. [使用內容片段選件建立活動](#activity) 在Adobe Target
1. [建立Experience Platform資料流](#datastream-id) Experience Platform
1. [將個人化整合至React型AEM Headless應用程式](#code) 使用AdobeWeb SDK。

## Adobe IMS設定{#adobe-ims-configuration}

Adobe IMS設定可促進AEM與Adobe Target之間的驗證。

檢閱 [檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) 以取得如何建立Adobe IMS設定的逐步指示。

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe TargetCloud Service{#adobe-target-cloud-service}

在AEM中建立Adobe TargetCloud Service，以便將內容片段匯出至Adobe Target。

檢閱 [檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) 以取得如何建立Adobe TargetCloud Service的逐步指示。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 設定資產資料夾{#configure-asset-folders}

Adobe TargetCloud Service（在內容感知設定中設定）必須套用至包含要匯出至Adobe Target之內容片段的AEM Assets資料夾階層。

+++展開以取得逐步指示

1. 登入 __AEM作者服務__ DAM管理員
1. 導覽至 __資產>檔案__，找出具有 `/conf` 套用至
1. 選取資產資料夾，然後選取 __屬性__ 從頂端動作列
1. 選取 __Cloud Services__ 標籤
1. 確認雲端設定已設為內容感知設定(`/conf`)，其中包含Adobe TargetCloud Services設定。
1. 選擇 __Adobe Target__ 從 __Cloud Service配置__ 下拉式清單。
1. 選擇 __儲存並關閉__ 在右上角

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## AEM Target整合的權限{#permission}

Adobe Target整合（以developer.adobe.com專案的形式顯示）必須授予 __編輯器__ 產品角色，以便將內容片段匯出至Adobe Target。

+++展開以取得逐步指示

1. 以可在Adobe Admin Console中管理Adobe Target產品的使用者身分登入Experience Cloud
1. 開啟 [Adobe Admin Console](https://adminconsole.adobe.com)
1. 選擇 __產品__ 然後開啟 __Adobe Target__
1. 在 __產品設定檔__ 索引標籤，選取 __*預設工作區*__
1. 選取 __API憑證__ 標籤
1. 在此清單中找到您的developer.adobe.com應用程式，並設定其 __產品角色__ to __編輯器__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## 將內容片段匯出至Target{#export-content-fragments}

存在於 [已設定AEM Assets資料夾階層](#apply-adobe-target-cloud-service-to-aem-assets-folders) 可匯出為「內容片段選件」。 這些內容片段選件是Target中特殊的JSON選件形式，可用於Target活動，以在無頭式應用程式中提供個人化體驗。

+++展開以取得逐步指示

1. 登入 __AEM作者__ 作為DAM使用者
1. 導覽至 __資產>檔案__，並在「已啟用Adobe Target」資料夾下，找到要匯出為JSON至Target的內容片段
1. 選取要匯出至Adobe Target的內容片段
1. 選擇 __匯出至Adobe Target選件__ 從頂端動作列
   + 此動作會將內容片段的完全水合JSON表示法匯出至Adobe Target，作為「內容片段選件」
   + 可在AEM中檢閱完全水合的JSON表示法
      + 選取內容片段
      + 展開側面板
      + 選擇 __預覽__ 表徵圖
      + 匯出至Adobe Target的JSON表示會顯示在主檢視中
1. 登入 [Adobe Experience Cloud](https://experience.adobe.com) 使用者擔任Adobe Target的編輯者角色
1. 從 [Experience Cloud](https://experience.adobe.com)，選取 __目標__ 從右上角的產品切換器開啟Adobe Target。
1. 確定已在 __工作區切換器__ 在右上角。
1. 選取 __選件__ 標籤
1. 選取 __類型__ 下拉式清單，然後選取 __內容片段__
1. 確認從AEM匯出的內容片段顯示在清單中
   + 將滑鼠指標暫留在選件上，然後選取 __檢視__ 按鈕
   + 檢閱 __選件資訊__ 並查看 __AEM深層連結__ 會直接在AEM製作服務中開啟內容片段

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 使用內容片段選件來鎖定活動{#activity}

在Adobe Target中，可以建立使用內容片段選件JSON作為內容的活動，以在無頭式應用程式中提供個人化體驗，並在AEM中建立和管理內容。

在此範例中，我們使用簡單的A/B活動，但可使用任何Target活動。

+++展開以取得逐步指示

1. 選取 __活動__ 標籤
1. 選擇 __+建立活動__，然後選取要建立的活動類型。
   + 此範例會建立簡單 __A/B測試__ 但內容片段選件可以支援任何活動類型
1. 在 __建立活動__ 精靈
   + 選擇 __Web__
   + 在 __選擇體驗撰寫器__，選取 __表單__
   + 在 __選擇工作區__，選取 __預設工作區__
   + 在 __選擇屬性__，選取「活動可用的屬性」 ，或選取 __無屬性限制__ 以允許在所有屬性中使用。
   + 選擇 __下一個__ 建立活動的方式
1. 透過選取 __重新命名__ 左上角
   + 為活動指定有意義的名稱
1. 在初始體驗中，設定 __位置1__ ，以定位活動
   + 在此範例中，定位名為 `wknd-adventure-promo`
1. 在 __內容__ 選取「預設內容」 ，然後選取 __變更內容片段__
1. 選取要為此體驗提供的匯出內容片段，然後選取 __完成__
1. 在「內容」文字區域中檢閱內容片段選件JSON，這與AEM製作服務中透過內容片段的預覽動作提供的JSON相同。
1. 在左側邊欄中，新增體驗，並選取要提供的不同內容片段選件
1. 選擇 __下一個__，並視活動需要設定定位規則
   + 在此範例中，將A/B測試保留為手動50/50分割。
1. 選擇 __下一個__，並完成活動設定
1. 選擇 __儲存並關閉__ 給它一個有意義的名字
1. 在Adobe Target中的活動中，選取 __啟動__ 從右上角的「非使用中/啟動/封存」下拉式清單。

目標為的Adobe Target活動 `wknd-adventure-promo` 位置現在可整合併公開於AEM Headless應用程式中。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform資料流ID{#datastream-id}

安 [Adobe Experience Platform Datastream](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) AEM Headless應用程式必須有ID，才能使用 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++展開以取得逐步指示

1. 導覽至 [Adobe Experience Cloud](https://experience.adobe.com/)
1. 開啟 __Experience Platform__
1. 選擇 __資料收集>資料流__ 選取 __新資料流__
1. 在「新增資料流」精靈中，輸入：
   + 名稱: `AEM Target integration`
   + 說明: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 事件結構： `Leave blank`
1. 選擇 __儲存__
1. 選擇 __添加服務__
1. 在 __服務__ 選取 __Adobe Target__
   + 已啟用： __是__
   + 屬性代號： __留空__
   + 目標環境ID: __留空__
      + 目標環境可在Adobe Target中設定，位於 __管理>主機__.
   + Target第三方ID命名空間： __留空__
1. 選擇 __儲存__
1. 在右側，複製 __資料流ID__ 用於 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) 設定呼叫。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## 將個人化新增至AEM Headless應用程式{#code}

本教學課程探討如何使用Adobe Target中的內容片段選件，個人化簡單的React應用程式，透過 [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 此方法可用來個人化任何JavaScript型網頁體驗。

Android™和iOS行動體驗可依照類似模式，透過 [Adobe的行動SDK](https://developer.adobe.com/client-sdks/documentation/).

### 必備條件

+ Node.js 14
+ Git
+ [WKND共用2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 安裝在AEM as a Cloud Author和Publish Services上

### 設定

1. 從下載範例React應用程式的原始碼 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟程式碼基底於 `~/Code/aem-guides-wknd-graphql/personalization-tutorial` 在您喜愛的IDE中
1. 更新您要應用程式連線至的AEM服務主機 `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. 執行應用程式，並確認其已連線至設定的AEM服務。 從命令列執行：

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. 安裝 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) 作為NPM套件。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Web SDK可用於程式碼中，以依活動位置擷取內容片段選件JSON。

   設定Web SDK時，需要兩個ID:

   + `edgeConfigId` 即 [資料流ID](#datastream-id)
   + `orgId` AEMas a Cloud Service/目標Adobe組織ID，可在 __Experience Cloud>設定檔>帳戶資訊>目前的組織ID__

   叫用Web SDK時，Adobe Target活動位置(在範例中， `wknd-adventure-promo`)必須設為 `decisionScopes` 陣列。

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 實施

1. 建立React元件 `AdobeTargetActivity.js` 呈現Adobe Target活動。

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

   AdobeTargetActivity React元件是使用下列方式叫用：

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. 建立React元件 `AdventurePromo.js` 轉譯冒險JSON Adobe Target伺服器。

   此React元件會取用代表冒險內容片段的完全水合JSON，並以促銷方式顯示。 根據匯出至Adobe Target的內容片段，顯示從Adobe Target內容片段選件所服務之JSON的React元件可視需要多樣且複雜。

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

   調用此React元件的方式如下：

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. 新增AdobeTargetActivity元件至React應用程式的 `Home.js` 在歷險清單上。

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

1. 如果React應用程式未執行，請使用 `npm run start`.

   在兩個不同的瀏覽器中開啟React應用程式，以便A/B測試將不同的體驗提供給每個瀏覽器。 如果兩個瀏覽器都顯示相同的冒險選件，請嘗試關閉/重新開啟其中一個瀏覽器，直到其他體驗顯示為止。

   下圖顯示為 `wknd-adventure-promo` 活動，根據Adobe Target的邏輯。

   ![體驗選件](./assets/target/offers-in-app.png)

## 恭喜！

我們已設定AEM as a Cloud Service將內容片段匯出至Adobe Target、使用Adobe Target活動中的內容片段選件，並在AEM無頭應用程式中呈現該活動，將體驗個人化。
