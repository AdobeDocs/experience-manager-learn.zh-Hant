---
title: 載入並觸發Target呼叫
description: 瞭解如何使用啟動規則載入、將參數傳遞至頁面請求，以及從您的網站頁面觸發Target呼叫。 頁面資訊會使用Adobe用戶端資料層來擷取並傳遞為參數，此資料層可讓您收集並儲存有關訪客在網頁上的體驗資料，然後方便存取此資料。
feature: 核心元件，Adobe客戶端資料層
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 3%

---


# 載入並觸發Target呼叫{#load-fire-target}

瞭解如何使用啟動規則載入、將參數傳遞至頁面請求，以及從您的網站頁面觸發Target呼叫。 網頁資訊會使用Adobe用戶端資料層來擷取並傳遞為參數，此資料層可讓您收集和儲存網頁上訪客體驗的相關資料，然後方便存取此資料。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 頁面載入規則

Adobe客戶端資料層是事件驅動的資料層。 載入頁AEM面資料層時，會觸發事件`cmp:show`。 在視訊中，使用自訂事件來呼叫`Launch Library Loaded`規則。 以下是自訂事件和資料元素在視訊中使用的程式碼片段。

### 自訂頁面顯示事件{#page-event}

![頁面顯示事件設定和自訂代碼](assets/load-and-fire-target-call.png)

在「啟動」屬性中，將新&#x200B;**Event**&#x200B;新增至&#x200B;**Rule**

+ __擴充功能：__ 核心
+ __事件類型：自__ 訂代碼
+ __名稱：頁__ 面顯示事件處理常式（或描述性內容）

點選&#x200B;__開啟編輯器__&#x200B;按鈕，然後貼入下列程式碼片段。 此代碼&#x200B;__必須__&#x200B;被添加到&#x200B;__事件配置__&#x200B;和後續的&#x200B;__操作__&#x200B;中。

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

自訂函式會定義`pageShownEventHandler`，並監聽核心元件所發出的事件、衍生核心元件的相關資訊、將其封裝為事件物件，以及在其裝載處以衍生事件資訊觸發啟動事件。

啟動規則是使用啟動的`trigger(...)`函式來觸發，此函式&#x200B;__僅__&#x200B;可從規則事件的自訂程式碼片段定義中使用。

`trigger(...)`函式將事件物件視為參數，而此參數會依序在啟動資料元素中以名為`event`的Launch中的其他保留名稱公開。 Launch中的「資料元素」現在可以使用語法（例如`event.component['someKey']`），參考來自`event`物件的此事件物件資料。

如果在事件的「自訂代碼」事件類型（例如，在「動作」中）的上下文外使用`trigger(...)`，則與「啟動」屬性整合的網站上會擲回JavaScript錯誤`trigger is undefined`。


### 資料元素

![資料元素](assets/data-elements.png)

Adobe啟動資料元素會透過核心擴充功能的自訂代碼資料元素類型，將自訂顯示頁面事件](#page-event)中觸發的事件物件[資料對應至Adobe Target可用的變數。

#### 頁面ID資料元素

```
if (event && event.id) {
    return event.id;
}
```

此程式碼會傳回核心元件的產生唯一Id。

![頁面ID](assets/pageid.png)

### 頁面路徑資料元素

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

此程式碼會傳AEM回頁面的路徑。

![頁面路徑](assets/pagepath.png)

### 頁面標題資料元素

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

此程式碼會傳AEM回頁面的標題。

![頁面標題](assets/pagetitle.png)

## 疑難排解

### 為什麼我的mbox無法在我的網頁上引發？

#### 未設定mboxDisable Cookie時的錯誤訊息

![目標Cookie網域錯誤](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解決方案

目標客戶有時會搭配Target使用雲端執行個體，以進行測試或進行簡單的概念驗證。 這些網域和其他許多網域都屬於「公用尾碼清單」。
如果您使用這些網域，現代瀏覽器將不會儲存Cookie，除非您使用`targetGlobalSettings()`自訂`cookieDomain`設定。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 後續步驟

+ [將體驗片段匯出至Adobe Target](./export-experience-fragment-target.md)

## 支援連結

+ [Adobe客戶端資料層文檔](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud除錯程式- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud除錯程式- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [使用Adobe客戶端資料層和核心元件文檔](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform調試器簡介](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)