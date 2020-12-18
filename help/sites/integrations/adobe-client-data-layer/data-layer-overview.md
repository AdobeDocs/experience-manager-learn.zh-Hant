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
source-git-commit: aa48c94413f83e794c5d062daaac85c97b451b82
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 1%

---


# 搭配AEM核心元件使用Adobe用戶端資料層{#overview}

Adobe用戶端資料層採用標準方法來收集和儲存網頁上訪客體驗的資料，然後方便存取此資料。 Adobe用戶端資料層不受平台限制，但已完全整合至核心元件，可與AEM搭配使用。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 想要在您的AEM網站上啟用Adobe用戶端資料層嗎？ [請參閱這裡的指示](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 探索資料層

您只需使用瀏覽器的開發人員工具和即時[WKND參考網站](https://wknd.site/)，即可瞭解Adobe用戶端資料層的內建功能。

>[!NOTE]
>
> 以下是從Chrome瀏覽器擷取的螢幕擷取畫面。

1. 導覽至[https://wknd.site](https://wknd.site)
1. 開啟您的開發人員工具，並在&#x200B;**Console**&#x200B;中輸入下列命令：

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

1. 再次運行命令`adobeDataLayer.getState()`並查找`training-data`的條目。
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

   上述程式碼會檢查`event`物件，並使用`adobeDataLayer.getState`方法來取得觸發事件之物件的目前狀態。 然後，幫助程式方法將檢查`filter`條件，並且僅當當前`dataObject`滿足篩選器時才會返回該條件。

   >[!CAUTION]
   >
   > 在本練習中重新整理瀏覽器很重要，否則主控台JavaScript將會遺失。****

1. 接著，輸入事件處理常式，當&#x200B;**Teaser**&#x200B;元件顯示在&#x200B;**Carousel**&#x200B;中時，將呼叫該事件處理常式。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   `teaserShownHandler`會呼叫`getDataObjectHelper`方法，並將`wknd/components/teaser`的篩選傳入`@type`，以篩選由其他元件觸發的事件。

1. 接著，將事件偵聽器推送至資料層以監聽`cmp:show`事件。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   `cmp:show`事件由許多不同的元件觸發，例如當新的投影片顯示在&#x200B;**轉盤**&#x200B;或當在&#x200B;**轉盤**&#x200B;元件中選取新的標籤時。

1. 在頁面上切換轉盤投影片，並觀察主控台陳述式：

   ![切換轉盤並檢視事件接聽程式](assets/teaser-console-slides.png)

1. 從資料層移除事件偵聽程式以停止監聽`cmp:show`事件：

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

   請注意，資源類型`wknd/components/page`用於篩選事件。

1. 接著，將事件偵聽器推送至資料層以監聽`cmp:show`事件，呼叫`pageShownHandler`。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您應立即看到已引發的控制台陳述式與頁面資料：

   ![頁面顯示資料](assets/page-show-console-data.png)

   頁面的`cmp:show`事件會在頁面最上方的每個頁面載入時觸發。 您可能會問，當頁面顯然已載入時，為何觸發事件處理常式？

   這是Adobe用戶端資料層的獨特功能之一，其中，您可在&#x200B;**之前或**&#x200B;之後，在資料層初始化後，註冊事件接聽程式。 ****&#x200B;這是避免比賽條件的關鍵功能。

   資料層會維護依序發生的所有事件的佇列陣列。 依預設，資料層將觸發&#x200B;**past**&#x200B;中發生的事件，以及&#x200B;**future**&#x200B;中的事件回呼。 它可以篩選事件至過去或未來。 [有關更多資訊，請參閱說明檔案](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 後續步驟

請參閱下列教學課程，瞭解如何使用事件導向的Adobe用戶端資料層來收集頁面資料並傳送至Adobe Analytics[。](../analytics/collect-data-analytics.md)

或者，瞭解如何[使用AEM元件自訂Adobe用戶端資料層](./data-layer-customize.md)


## 其他資源 {#additional-resources}

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe用戶端資料層與核心元件檔案](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
