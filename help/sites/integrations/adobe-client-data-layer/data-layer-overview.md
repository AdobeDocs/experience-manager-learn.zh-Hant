---
title: 搭配AEM核心元件使用Adobe用戶端資料層
description: Adobe用戶端資料層引進了標準方法，用於收集和儲存網頁上訪客體驗的相關資料，並且讓此資料易於存取。 Adobe用戶端資料層不受平台限制，但已完全整合至核心元件，以便與AEM搭配使用。
feature: 核心元件
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
topic: Integrations
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 1%

---


# 搭配AEM核心元件使用Adobe用戶端資料層 {#overview}

Adobe用戶端資料層引進了標準方法，用於收集和儲存網頁上訪客體驗的相關資料，並且讓此資料易於存取。 Adobe用戶端資料層不受平台限制，但已完全整合至核心元件，以便與AEM搭配使用。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 想在AEM網站上啟用Adobe用戶端資料層嗎？ [請參閱這裡的指示](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 探索資料層

您只需使用瀏覽器的開發人員工具和即時[ WKND參考網站](https://wknd.site/)，即可了解Adobe用戶端資料層的內建功能。

>[!NOTE]
>
> 從Chrome瀏覽器擷取的下方螢幕擷取畫面。

1. 導覽至[https://wknd.site](https://wknd.site)
1. 開啟開發人員工具，並在&#x200B;**主控台**&#x200B;中輸入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect回應，可查看AEM網站上資料層的目前狀態。 您應該會看到頁面和個別元件的相關資訊。

   ![Adobe資料層回應](assets/data-layer-state-response.png)

1. 在主控台中輸入下列內容，將資料物件推送至資料層：

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
1. 接下來新增路徑參數，只傳回元件的特定狀態：

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![只傳回單一元件資料項目](assets/return-just-single-component.png)

## 使用事件

根據資料層的事件觸發任何自訂程式碼是最佳作法。 接下來，探索註冊和聆聽不同事件。

1. 在主控台中輸入下列協助方法：

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

   上述程式碼將檢查`event`物件，並使用`adobeDataLayer.getState`方法來取得觸發事件之物件的目前狀態。 協助方法接著會檢查`filter`條件，且只有目前的`dataObject`符合篩選條件時，才會傳回該條件。

   >[!CAUTION]
   >
   > 在本練習中重新整理瀏覽器很重要，否則主控台JavaScript將會遺失。****

1. 接下來，輸入當&#x200B;**Teaser**&#x200B;元件顯示在&#x200B;**Carousel**&#x200B;中時將調用的事件處理程式。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   `teaserShownHandler`將呼叫`getDataObjectHelper`方法，並傳入`wknd/components/teaser`篩選器作為`@type`，以篩選掉其他元件所觸發的事件。

1. 接下來，將事件接聽程式推送至資料層，以接聽`cmp:show`事件。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   `cmp:show`事件由許多不同的元件觸發，例如當新幻燈片顯示在&#x200B;**轉盤**&#x200B;中，或當在&#x200B;**標籤**&#x200B;元件中選擇了新頁簽時。

1. 在頁面上切換輪播投影片，並觀察主控台陳述式：

   ![切換轉盤，並查看事件接聽程式](assets/teaser-console-slides.png)

1. 從資料層中移除事件監聽器，以停止`cmp:show`事件的監聽：

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 返回頁面並切換輪播投影片。 請注意，不會再記錄任何陳述式，且不會監聽事件。

1. 接下來，輸入觸發頁面顯示事件時將呼叫的事件處理常式：

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

1. 接下來，將事件接聽程式推送至資料層，以接聽`cmp:show`事件，並呼叫`pageShownHandler`。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您應會立即看到頁面資料引發的主控台陳述式：

   ![頁面顯示資料](assets/page-show-console-data.png)

   頁面的`cmp:show`事件會在頁面最上方的每個頁面載入時觸發。 您可能會問，當頁面顯然已載入時，為何觸發事件處理常式？

   這是Adobe客戶端資料層的獨特功能之一，因為您可以在&#x200B;**之前註冊事件偵聽器**，或在&#x200B;**之後註冊事件偵聽器**。 這是避免種族狀況的關鍵特徵。

   資料層會維護依序發生的所有事件的佇列陣列。 依預設，資料層會針對&#x200B;**past**&#x200B;中發生的事件，以及&#x200B;**future**&#x200B;中的事件觸發事件回呼。 可將事件篩選為過去或未來。 [如需詳細資訊，請參閱本檔案](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 後續步驟

請查看下列教學課程，了解如何使用事件導向的Adobe用戶端資料層來[收集頁面資料並傳送至Adobe Analytics](../analytics/collect-data-analytics.md)。

或者，了解如何使用AEM元件](./data-layer-customize.md)自訂Adobe用戶端資料層[


## 其他資源 {#additional-resources}

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe用戶端資料層與核心元件檔案](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
