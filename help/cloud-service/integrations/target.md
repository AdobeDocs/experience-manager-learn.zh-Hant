---
title: 無AEM頭和目標個性化
description: 本教程探AEM討如何將內容片段導出到Adobe Target，然後使用AdobeWeb SDK對無頭體驗進行個性化。
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

# 利用內AEM容片段個性化無頭體驗

>[!IMPORTANT]
>
> Adobe Experience Manager內容片段向Adobe Target的出口在as a Cloud ServiceAEM中 [預釋放通道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=en#new-features)。



本教程探AEM討如何將內容片段導出到Adobe Target，然後使用AdobeWeb SDK對無頭體驗進行個性化。 的 [反應WKND應用](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) 用於探索如何向體驗中添加使用內容片段提供的個性化目標活動，以推廣WKND冒險。

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

本教程介紹了設定和Adobe Target所涉AEM的步驟：

1. [為Adobe Target建立Adobe IMS配置](#adobe-ims-configuration) 在AEM作者中
1. [建立Adobe TargetCloud Service](#adobe-target-cloud-service) 在AEM作者中
1. [將Adobe TargetCloud Service應用於AEM Assets資料夾](#configure-asset-folders) 在AEM作者中
1. [許可Adobe TargetCloud Service](#permission) 在Adobe Admin Console
1. [導出內容片段](#export-content-fragments) 從AEM作者到目標
1. [使用內容片段提供建立活動](#activity) 在Adobe Target
1. [建立Experience Platform資料流](#datastream-id) Experience Platform
1. [將個性化功能整合到基於反應的無AEM頭應用](#code) 使用AdobeWeb SDK。

## Adobe IMS配置{#adobe-ims-configuration}

一種Adobe IMS配置，便於在和Adobe Target之AEM間進行驗證。

審閱 [文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) 有關如何建立Adobe IMS配置的逐步說明。

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe TargetCloud Service{#adobe-target-cloud-service}

建立Adobe TargetCloud ServiceAEM，以便向Adobe Target出口內容片段。

審閱 [文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) 有關如何建立Adobe TargetCloud Service的逐步說明。

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## 配置資產資料夾{#configure-asset-folders}

在上下文感知配置中配置的Adobe TargetCloud Service必須應用於包含要導出到Adobe Target的內容片段的AEM Assets資料夾層次結構。

+++展開以獲取逐步說明

1. 登錄到 __AEM作者服務__ 作為DAM管理員
1. 導航到 __資產>檔案__，查找具有 `/conf` 應用於
1. 選擇資產資料夾，然後選擇 __屬性__ 從頂部操作欄
1. 選擇 __Cloud Services__ 頁籤
1. 確保將雲配置設定為上下文感知配置(`/conf`)中的「Oracle」(O)。
1. 選擇 __Adobe Target__ 從 __Cloud Service配置__ 下拉清單。
1. 選擇 __保存並關閉__ 右上角

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## 允許目標AEM整合{#permission}

必須授予Adobe Target整合（以developer.adobe.com項目的形式顯示） __編輯器__ 產品在Adobe Admin Console的角色，以便將內容片段導出到Adobe Target。

+++展開以獲取逐步說明

1. 以可以管理Adobe Admin ConsoleAdobe Target產品的用戶身份登錄到Experience Cloud
1. 開啟 [Adobe Admin Console](https://adminconsole.adobe.com)
1. 選擇 __產品__ 然後開啟 __Adobe Target__
1. 在 __產品配置檔案__ 頁籤 __*預設工作區*__
1. 選擇 __API憑據__ 頁籤
1. 在此清單中查找您的developer.adobe.com應用並設定其 __產品角色__ 至 __編輯器__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## 將內容片段導出到目標{#export-content-fragments}

位於 [配置的AEM Assets資料夾層次結構](#apply-adobe-target-cloud-service-to-aem-assets-folders) 可以作為內容片段提供導出到Adobe Target。 這些內容片段提供是目標中的JSON提供的一種特殊形式，可用於目標活動，以在無頭應用中提供個性化體驗。

+++展開以獲取逐步說明

1. 登錄到 __AEM作者__ DAM用戶
1. 導航到 __資產>檔案__，並在「啟用Adobe Target」資料夾下找到要作為JSON導出到目標的內容片段
1. 選擇要導出到Adobe Target的內容片段
1. 選擇 __對Adobe Target的出口優惠__ 從頂部操作欄
   + 此操作將內容片段的完全水合JSON表示法導出到Adobe Target，作為「內容片段提供」
   + 可以在以下方面查看完全水合的JSON表AEM示
      + 選擇內容片段
      + 展開側面板
      + 選擇 __預覽__ 表徵圖
      + 導出到Adobe Target的JSON表示形式顯示在主視圖中
1. 登錄到 [Adobe Experience Cloud](https://experience.adobe.com) 用戶在Adobe Target的編輯器角色中
1. 從 [Experience Cloud](https://experience.adobe.com)選中 __目標__ 從右上角的產品切換器開啟Adobe Target。
1. 確保在 __工作區切換器__ 右上角。
1. 選擇 __優惠__ 頁籤
1. 選擇 __類型__ 下拉清單，然後選擇 __內容片段__
1. 驗證從中導出的內容AEM片段是否出現在清單中
   + 將滑鼠懸停在優惠上，然後選擇 __視圖__ 按鈕
   + 查看 __服務資訊__ 看 __深AEM鏈__ 直接在AEM Author服務中開啟內容片段

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## 使用內容片段提供的目標活動{#activity}

在Adobe Target，可以建立一個活動，該活動使用內容片段提供JSON作為內容，允許在無頭應用中個性化體驗，並在中建立和管理內AEM容。

在本示例中，我們使用一個簡單的A/B活動，但可以使用任何Target活動。

+++展開以獲取逐步說明

1. 選擇 __活動__ 頁籤
1. 選擇 __+建立活動__，然後選擇要建立的活動類型。
   + 此示例建立一個 __A/BTest__ 但內容片段提供可以支援任何活動類型
1. 在 __建立活動__ 嚮導
   + 選擇 __Web__
   + 在 __選擇體驗作曲家__&#x200B;選中 __窗體__
   + 在 __選擇工作區__&#x200B;選中 __預設工作區__
   + 在 __選擇屬性__，選擇「活動可用的屬性」，或選擇 __無屬性限制__ 允許在所有屬性中使用。
   + 選擇 __下一個__ 建立活動
1. 通過選擇 __更名__ 左上角
   + 為活動指定有意義的名稱
1. 在初始經驗中，設定 __位置1__ 為目標活動
   + 在此示例中，以名為 `wknd-adventure-promo`
1. 下 __內容__ 選擇預設內容，然後選擇 __更改內容片段__
1. 選擇要用於此體驗的導出內容片段，然後選擇 __完成__
1. 查看「內容」文本區域中的「內容片段提供JSON」，這與通過「內容片段的預覽」操作在AEM Author服務中提供的JSON相同。
1. 在左滑軌中，添加體驗，並選擇其他內容片段服務
1. 選擇 __下一個__，並根據活動的需要配置目標規則
   + 在本示例中，將A/B測試保留為手動50/50剝離。
1. 選擇 __下一個__，並完成活動設定
1. 選擇 __保存並關閉__ 給它起個有意義的名字
1. 在Adobe Target的活動中，選擇 __激活__ 從右上角的Inactive/Activate/Archive（非活動/激活/存檔）下拉清單中。

Adobe Target的活動 `wknd-adventure-promo` 現在，位置可以整合併在無頭應用AEM中公開。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Experience Platform資料流ID{#datastream-id}

安 [Adobe Experience Platform資料流](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) Headless應用需要IDAEM才能使用 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html)。

+++展開以獲取逐步說明

1. 導航到 [Adobe Experience Cloud](https://experience.adobe.com/)
1. 開啟 __Experience Platform__
1. 選擇 __資料收集>資料流__ 選擇 __新建資料流__
1. 在「新建資料流」嚮導中，輸入：
   + 名稱: `AEM Target integration`
   + 說明: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + 事件架構： `Leave blank`
1. 選擇 __保存__
1. 選擇 __添加服務__
1. 在 __服務__ 選擇 __Adobe Target__
   + 已啟用： __是__
   + 屬性令牌： __留空__
   + 目標環境ID: __留空__
      + 目標環境可在Adobe Target設定 __管理>主機__。
   + 目標第三方ID命名空間： __留空__
1. 選擇 __保存__
1. 右邊，複製 __資料流ID__ 用於 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) 配置調用。

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## 向無頭應用添AEM加個性化{#code}

本教程通過以下途徑探索在Adobe Target使用內容片段產品個性化一個簡單的React應用 [Adobe Experience PlatformWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)。 此方法可用於個性化任何基於JavaScript的Web體驗。

Android™和iOS的移動體驗可以按照類似模式使用 [Adobe的移動SDK](https://developer.adobe.com/client-sdks/documentation/)。

### 必備條件

+ Node.js 14
+ Git
+ [WKND共用2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 作為雲AEM作者和發佈服務安裝

### 設定

1. 從下載示例React應用的原始碼 [吉圖布網](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開放代碼庫位於 `~/Code/aem-guides-wknd-graphql/personalization-tutorial` 在您最喜愛的IDE中
1. 更新AEM要應用程式連接到的服務主機 `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. 運行應用，並確保它連接到已配置的AEM服務。 從命令行執行：

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. 安裝 [AdobeWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) 作為NPM包。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Web SDK可用於代碼中，以按活動位置獲取內容片段提供JSON。

   配置Web SDK時，需要兩個ID:

   + `edgeConfigId` 即 [資料流ID](#datastream-id)
   + `orgId` 可AEM以在以下位置找到的as a Cloud Service/目標Adobe組織ID __Experience Cloud>配置檔案>帳戶資訊>當前組織ID__

   調用Web SDK時，Adobe Target活動位置(在本例中， `wknd-adventure-promo`)必須設定為 `decisionScopes` 陣列。

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### 實施

1. 建立React元件 `AdobeTargetActivity.js` 讓Adobe Target的活動浮出水面。

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

   調用AdobeTargetActivity React元件時，如下所示：

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. 建立React元件 `AdventurePromo.js` 讓冒險家JSONAdobe Target服務。

   此React元件採用表示冒險內容片段的完全水合JSON，並以促銷方式顯示。 根據導出到Adobe Target的內容片段，顯示從Adobe Target內容片段提供服務的JSON的React元件可根據需要變化和複雜。

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

1. 將AdobeTargetActivity元件添加到React應用 `Home.js` 在歷險記上。

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

1. 如果React應用未運行，請重新啟動 `npm run start`。

   在兩個不同的瀏覽器中開啟React應用，以便A/Btest為每個瀏覽器提供不同的體驗。 如果兩個瀏覽器都顯示相同的冒險產品，請嘗試關閉/重新開啟其中一個瀏覽器，直到顯示其他體驗。

   下圖顯示了為 `wknd-adventure-promo` 基於Adobe Target的邏輯。

   ![體驗服務](./assets/target/offers-in-app.png)

## 恭喜！

現在，我們已配置AEMas a Cloud Service將內容片段導出到Adobe Target，使用了Adobe Target活動中的內容片段優惠，並在無頭應用中發現了該活AEM動，使體驗個性化。
