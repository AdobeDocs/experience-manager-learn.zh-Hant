---
title: 搭配AEM核心元件使用Adobe用戶端資料層
description: Adobe用戶端資料層引進了標準方法，用於收集和儲存網頁上訪客體驗的相關資料，並且讓此資料易於存取。 Adobe用戶端資料層不受平台限制，但已完全整合至核心元件，以便與AEM搭配使用。
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 835657082c0c6bf7b2822b53ef2b99039d77f249
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 0%

---

# 搭配AEM核心元件使用Adobe用戶端資料層 {#overview}

Adobe用戶端資料層引進了標準方法，用於收集和儲存網頁上訪客體驗的相關資料，並且讓此資料易於存取。 Adobe用戶端資料層不受平台限制，但已完全整合至核心元件，以便與AEM搭配使用。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 想在AEM網站上啟用Adobe用戶端資料層嗎？ [請參閱此處的指示](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## 探索資料層

您只需使用瀏覽器的開發人員工具和即時功能，就能了解Adobe用戶端資料層的內建功能 [WKND參考網站](https://wknd.site/).

>[!NOTE]
>
> 從Chrome瀏覽器擷取的下方螢幕擷取畫面。

1. 導覽至 [https://wknd.site](https://wknd.site)
1. 開啟開發人員工具，並在 **主控台**:

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

1. 運行命令 `adobeDataLayer.getState()` 再次，找到 `training-data`.
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

   上述程式碼會檢查 `event` 物件，並使用 `adobeDataLayer.getState` 方法來取得觸發事件之物件的目前狀態。 協助方法接著會檢查 `filter` 條件，且僅限目前 `dataObject` 符合篩選條件即會傳回。

   >[!CAUTION]
   >
   > 這很重要 **not** 在本練習中重新整理瀏覽器，否則主控台JavaScript會遺失。

1. 接下來，輸入事件處理常式，此事件處理常式會在 **Teaser** 元件顯示在 **輪播**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   此 `teaserShownHandler` 將呼叫 `getDataObjectHelper` 在 `wknd/components/teaser` 作為 `@type` 以篩除其他元件觸發的事件。

1. 接下來，將事件接聽程式推送至資料層，以監聽 `cmp:show` 事件。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   此 `cmp:show` 事件會由許多不同的元件觸發，例如新投影片顯示於 **輪播** 或在 **標籤** 元件。

1. 在頁面上切換輪播投影片，並觀察主控台陳述式：

   ![切換轉盤，並查看事件接聽程式](assets/teaser-console-slides.png)

1. 從資料層移除事件接聽程式，以停止監聽 `cmp:show` 事件：

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 返回頁面並切換輪播投影片。 請注意，不會再記錄任何陳述式，且不會監聽事件。

1. 接下來，建立觸發頁面顯示事件時呼叫的事件處理常式：

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   請注意，資源類型 `wknd/components/page` 用於篩選事件。

1. 接下來，將事件接聽程式推送至資料層，以監聽 `cmp:show` 事件，呼叫 `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您應會立即看到頁面資料引發的主控台陳述式：

   ![頁面顯示資料](assets/page-show-console-data.png)

   此 `cmp:show` 頁面的事件會在頁面最上方的每次頁面載入時觸發。 您可能會問，當頁面顯然已載入時，為何觸發事件處理常式？

   這是Adobe客戶端資料層的獨特功能之一，您可以註冊事件偵聽器 **befor** 或 **after** 資料層已初始化。 這是避免種族狀況的關鍵特徵。

   資料層會維護依序發生的所有事件的佇列陣列。 依預設，資料層會觸發 **過去** 以及 **未來**. 可將事件篩選為過去或未來。 [如需詳細資訊，請參閱本檔案](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## 後續步驟

查看下列教學課程，了解如何使用事件導向的Adobe用戶端資料層，以 [收集頁面資料並傳送至Adobe Analytics](../analytics/collect-data-analytics.md).

或者學習如何 [使用AEM元件自訂Adobe用戶端資料層](./data-layer-customize.md)


## 其他資源 {#additional-resources}

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe用戶端資料層與核心元件檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
