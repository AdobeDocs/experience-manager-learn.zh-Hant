---
title: 搭配AEM核心元件使用Adobe用戶端資料層
description: Adobe用戶端資料層採用標準方法來收集和儲存網頁上訪客體驗的資料，然後方便存取此資料。 Adobe用戶端資料層不受平台限制，但已完全整合至核心元件，可與AEM搭配使用。
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: e13a5171fbeb9e1eb5f78d1c691bc8b4b896a998
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 1%

---


# 搭配AEM核心元件使用Adobe用戶端資料層 {#overview}

Adobe用戶端資料層採用標準方法來收集和儲存網頁上訪客體驗的資料，然後方便存取此資料。 Adobe用戶端資料層不受平台限制，但已完全整合至核心元件，可與AEM搭配使用。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 想要在您的AEM網站上啟用Adobe用戶端資料層嗎？ [請參閱這裡的指示](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 探索資料層

您只需使用瀏覽器的開發人員工具和即時 [WKND參考網站，就能瞭解Adobe用戶端資料層的內建功能](https://wknd.site/)。

>[!NOTE]
>
> 以下是從Chrome瀏覽器擷取的螢幕擷取畫面。

1. 導覽至 [https://wknd.site](https://wknd.site)
1. 開啟您的開發人員工具，並在主控台中輸入下列 **命令**:

   ```js
   window.adobeDataLayer.getState();
   ```

   檢查回應，以查看AEM網站上資料層的目前狀態。 您應該會看到頁面和個別元件的相關資訊。

   ![Adobe資料層回應](assets/data-layer-state-response.png)

1. 在主控台中輸入下列項目，將資料物件推送至資料層：

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. 再次運行 `adobeDataLayer.getState()` 命令並查找條目 `training-data`。
1. 接著新增路徑參數，只傳回元件的特定狀態：

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![僅傳回單一元件資料輸入](assets/return-just-single-component.png)

## 使用事件

根據資料層的事件觸發任何自訂程式碼是最佳實務。 接下來，請探索註冊並聆聽不同的事件。

1. 在控制台中輸入以下幫助方法：

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   上述程式碼會檢查 `event` 物件，並使用 `adobeDataLayer.getState` 方法來取得觸發事件之物件的目前狀態。 Helper方法接著會檢查標 `filter` 準，而且只有當目前符合 `dataObject` 篩選條件時才會傳回它。

   >[!CAUTION]
   >
   > 在本練習中 **不要重新整** 理瀏覽器，否則主控台JavaScript將會遺失。

1. 接著，輸入事件處理常式，當Teaser元件顯示在 **Carousel中時** ，將會呼叫 **該處理常式**。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   將 `teaserShownHandler` 會呼叫方 `getDataObjectHelper` 法，並傳入篩選為 `wknd/components/teaser` ，以 `@type` 篩選由其他元件觸發的事件。

1. 接著，將事件偵聽器推送至資料層以監聽該 `cmp:show` 事件。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   事 `cmp:show` 件由許多不同的元件觸發，例如當轉盤中顯示新投影片時，或在Tab元件中選取新的 **標籤時****** 。

1. 在頁面上切換轉盤投影片，並觀察主控台陳述式：

   ![切換轉盤並檢視事件接聽程式](assets/teaser-console-slides.png)

1. 從資料層移除事件接聽程式，以停止監聽 `cmp:show` 事件：

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 返回頁面並切換轉盤投影片。 請注意，不再記錄任何陳述，且未聽取該事件。

1. 接著，輸入事件處理常式，在觸發頁面顯示事件時呼叫：

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   請注意，資源類 `wknd/components/page` 型用於篩選事件。

1. 接著，將事件偵聽器推送至資料層，以監聽 `cmp:show` 事件，呼叫 `pageShownHandler`。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您應立即看到已引發的控制台陳述式與頁面資料：

   ![頁面顯示資料](assets/page-show-console-data.png)

   頁面 `cmp:show` 的事件會在頁面最上方的每個頁面載入時觸發。 您可能會問，當頁面顯然已載入時，為何觸發事件處理常式？

   這是Adobe用戶端資料層的獨特功能之一，您可在資料層初始化之前 ******或之** 後，註冊事件偵聽器。 這是避免比賽條件的關鍵功能。

   資料層會維護依序發生的所有事件的佇列陣列。 依預設，資料層會觸發事件回呼，以回呼過去發 **生的** ，以及將來發 **生的**。 它可以篩選事件至過去或未來。 [有關更多資訊，請參閱說明檔案](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 後續步驟

請參閱下列教學課程，瞭解如何使用事件導向的Adobe用戶端資料層來收 [集頁面資料並傳送至Adobe Analytics](../analytics/collect-data-analytics.md)。


## 其他資源 {#additional-resources}

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe用戶端資料層與核心元件檔案](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html)
