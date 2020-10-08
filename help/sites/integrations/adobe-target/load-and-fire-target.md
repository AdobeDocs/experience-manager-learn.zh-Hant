---
title: 載入並觸發Target呼叫
description: 瞭解如何使用啟動規則載入、將參數傳遞至頁面請求，以及從您的網站頁面觸發Target呼叫。 頁面資訊會使用Adobe用戶端資料層來擷取並傳遞為參數，此資料層可讓您收集和儲存有關訪客在網頁上的體驗資料，然後方便存取此資料。
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 3%

---


# 載入並觸發Target呼叫 {#load-fire-target}

瞭解如何使用啟動規則載入、將參數傳遞至頁面請求，以及從您的網站頁面觸發Target呼叫。 頁面資訊會使用Adobe用戶端資料層來擷取並傳遞為參數，此資料層可讓您收集和儲存有關訪客在網頁上的體驗資料，然後方便存取此資料。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 頁面載入規則

Adobe用戶端資料層是事件導向的資料層。 載入AEM Page資料層時，會觸發事件 `cmp:show` 。 在視訊中，會使 `Launch Library Loaded` 用自訂事件來呼叫規則。 以下是自訂事件和資料元素在視訊中使用的程式碼片段。

### 自訂事件

以下程式碼片段會將函式推送至資料層，以新增事件偵聽器。 觸發事 `cmp:show` 件時，會呼 `pageShownEventHandler` 叫函式。 在此函式中，會新增一些例行性檢查，並 `dataObject` 針對觸發事件的元件，以資料層的最新狀態建構新的例行性檢查。

之後就 `trigger(dataObject)` 叫了。 `trigger()` 是Launch中的保留名稱，將「觸發」啟動規則。 我們將事件物件傳遞為參數，而參數會以Launch命名事件中的其他保留名稱呈現。 Launch中的資料元素現在可以參照各種屬性，例如： `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
      //get the state of the component that triggered the event
      component: window.adobeDataLayer.getState(evt.eventInfo.path)
   };

      //Trigger the Launch Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
      // i.e `event.component['someKey']`
      trigger(event);
   }
}

//set the namespace to avoid a potential race condition
window.adobeDataLayer = window.adobeDataLayer || [];
//push the event listener for cmp:show into the data layer
window.adobeDataLayer.push(function (dl) {
   //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
   dl.addEventListener("cmp:show", pageShownEventHandler);
});
```

### 資料層頁面ID

```
if(event && event.id) {
    return event.id;
}
```

![頁面ID](assets/pageid.png)

### 頁面路徑

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![頁面路徑](assets/pagepath.png)

### 頁面標題

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![頁面標題](assets/pagetitle.png)

### 常見問題

#### 為什麼我的mbox無法在我的網頁上引發？

**未設定mboxDisable Cookie時的錯誤訊息**

![目標Cookie網域錯誤](assets/target-cookie-error.png)

**解決方案**

目標客戶有時會搭配Target使用雲端執行個體，以進行測試或進行簡單的概念驗證。 這些網域和其他許多網域都屬於「公用尾碼清單」。
如果您使用這些網域，現代瀏覽器將不會儲存Cookie，除非您使用自訂 `cookieDomain` 設定 `targetGlobalSettings()`。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## 後續步驟

1. [將體驗片段匯出至Adobe Target](./export-experience-fragment-target.md)

## 支援連結

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloud除錯程式- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud除錯程式- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [使用Adobe用戶端資料層與核心元件檔案](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Adobe Experience Platform Debugger簡介](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)