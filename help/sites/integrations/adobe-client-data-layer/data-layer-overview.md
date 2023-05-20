---
title: 將Adobe客戶端資料層與核AEM心元件一起使用
description: Adobe客戶端資料層引入了一種標準方法來收集和儲存有關訪問者體驗的資料，然後使訪問這些資料變得容易。 Adobe客戶端資料層與平台無關，但完全整合到核心元件中，供使用AEM。
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 99b3ecf7823ff9a116c47c88abc901f8878bbd7a
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 0%

---

# 將Adobe客戶端資料層與核AEM心元件一起使用 {#overview}

Adobe客戶端資料層引入了一種標準方法來收集和儲存有關訪問者體驗的資料，然後使訪問這些資料變得容易。 Adobe客戶端資料層與平台無關，但完全整合到核心元件中，供使用AEM。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 是否要在您的站點上啟用Adobe客戶端數AEM據層？ [請參閱此處的說明](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 瀏覽資料層

您只需使用瀏覽器的開發人員工具和即時功能即可瞭解Adobe客戶端資料層的內置功能 [WKND參考站點](https://wknd.site/us/en.html)。

>[!NOTE]
>
> 下面是從Chrome瀏覽器拍攝的螢幕截圖。

1. 導航到 [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 開啟開發人員工具，在 **控制台**:

   ```js
   window.adobeDataLayer.getState();
   ```

   要查看站點上資料層的當前狀態，請AEM檢查響應。 您應看到有關頁面和各個元件的資訊。

   ![Adobe資料層響應](assets/data-layer-state-response.png)

1. 在控制台中輸入以下命令，將資料對象推送到資料層：

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

1. 運行命令 `adobeDataLayer.getState()` 找到 `training-data`。
1. 接下來添加路徑參數以僅返回元件的特定狀態：

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![僅返回單個元件資料條目](assets/return-just-single-component.png)

## 使用事件

根據來自資料層的事件觸發任何自定義代碼是一種最佳做法。 接下來，瀏覽註冊和收聽不同的事件。

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

   上述代碼檢查 `event` 對象和使用 `adobeDataLayer.getState` 方法，獲取觸發事件的對象的當前狀態。 然後helper方法檢查 `filter` 只有當 `dataObject` 滿足返回的篩選條件。

   >[!CAUTION]
   >
   > 很重要 **不** 在本練習中刷新瀏覽器，否則控制台JavaScript將丟失。

1. 接下來，輸入在 **預告** 元件顯示在 **旋轉木馬**。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   的 `teaserShownHandler` 函式調用 `getDataObjectHelper` 函式並傳遞篩選器 `wknd/components/teaser` 的 `@type` 以篩選由其他元件觸發的事件。

1. 接下來，將事件偵聽器推送到資料層以偵聽 `cmp:show` 的子菜單。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   的 `cmp:show` 事件由許多不同的元件觸發，例如，在 **旋轉木馬**，或在 **頁籤** 元件。

1. 在頁面上，切換旋轉軸幻燈片並觀察控制台語句：

   ![切換旋轉傳送器並查看事件偵聽器](assets/teaser-console-slides.png)

1. 停止收聽 `cmp:show` 事件，從資料層中刪除事件偵聽器

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 返回到頁面並切換旋轉拉片。 請注意沒有記錄更多語句，並且事件未被收聽。

1. 接下來，建立在觸發頁面顯示事件時調用的事件處理程式：

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   注意資源類型 `wknd/components/page` 用於篩選事件。

1. 接下來，將事件偵聽器推送到資料層以偵聽 `cmp:show` 事件，調用 `pageShownHandler`。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您應立即看到已觸發的控制台語句，其中包含頁資料：

   ![頁面顯示資料](assets/page-show-console-data.png)

   的 `cmp:show` 頁面的事件在頁面頂部的每個頁面載入時觸發。 您可能會問，當頁面顯然已載入時，為什麼觸發了事件處理程式？

   Adobe客戶端資料層的一個獨特功能是，您可以註冊事件偵聽器 **先** 或 **後** 資料層已初始化，這有助於避免競爭條件。

   資料層維護按順序發生的所有事件的隊列陣列。 預設情況下，資料層將觸發在 **過去** 和 **未來**。 可以過濾過去或將來的事件。 [有關詳細資訊，請參閱文檔](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 後續步驟

有兩種選擇可以繼續學習，第一種，檢查 [收集頁資料併發送到Adobe Analytics](../analytics/collect-data-analytics.md) 演示Adobe客戶端資料層使用的教程。 第二個選擇是，學習如何 [使用元件自定義Adobe客戶端數AEM據層](./data-layer-customize.md)


## 其他資源 {#additional-resources}

* [Adobe客戶端資料層文檔](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe客戶端資料層和核心元件文檔](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
