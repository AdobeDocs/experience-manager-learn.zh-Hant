---
title: 載入及觸發Target呼叫
description: 了解如何使用Launch規則載入、傳遞參數至頁面請求，以及從您的網站頁面觸發Target呼叫。 系統會使用Adobe用戶端資料層來擷取頁面資訊並以參數形式傳遞，此資料層可讓您收集和儲存訪客在網頁上的體驗相關資料，並讓此資料易於存取。
feature: 核心元件，Adobe用戶端資料層
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 1%

---


# 載入及觸發Target呼叫 {#load-fire-target}

了解如何使用Launch規則載入、傳遞參數至頁面請求，以及從您的網站頁面觸發Target呼叫。 網頁資訊是使用Adobe用戶端資料層來擷取並以參數形式傳遞的，該資料層可讓您收集和儲存訪客在網頁上的體驗相關資料，並且讓此資料易於存取。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 頁面載入規則

Adobe用戶端資料層是事件導向的資料層。 載入AEM頁面資料層時，會觸發事件`cmp:show` 。 在影片中，使用自訂事件叫用`Launch Library Loaded`規則。 以下是影片中使用的程式碼片段，用於自訂事件和資料元素。

### 自訂頁面顯示事件{#page-event}

![頁面顯示事件設定和自訂程式碼](assets/load-and-fire-target-call.png)

在Launch屬性中，將新的&#x200B;**Event**&#x200B;新增至&#x200B;**Rule**

+ __擴充功能：__ 核心
+ __事件類型：__ 自訂程式碼
+ __名稱：__ 頁面顯示事件處理常式（或描述性內容）

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

自訂函式會定義`pageShownEventHandler`，監聽AEM核心元件發出的事件、衍生核心元件的相關資訊、將其封裝至事件物件中，並在其裝載處觸發衍生事件資訊的啟動事件。

Launch規則是使用Launch的`trigger(...)`函式來觸發，此函式為&#x200B;__only__，可從規則的事件的自訂程式碼片段定義中取得。

`trigger(...)`函式會將事件物件視為參數，而參數會依名稱為`event`的Launch中其他保留名稱公開於Launch資料元素中。 Launch中的資料元素現在可以使用`event.component['someKey']`等語法，參照`event`物件中此事件物件的資料。

如果在事件的自訂程式碼事件類型（例如，在動作中）的內容之外使用`trigger(...)`，則在與Launch屬性整合的網站上擲回JavaScript錯誤`trigger is undefined`。


### 資料元素

![資料元素](assets/data-elements.png)

Adobe啟動資料元素會透過核心擴充功能的「自訂程式碼資料元素類型」，將自訂顯示頁面事件](#page-event)中觸發的事件物件[的資料對應至Adobe Target中可用的變數。

#### 頁面ID資料元素

```
if (event && event.id) {
    return event.id;
}
```

此程式碼會傳回核心元件的產生唯一ID。

![頁面ID](assets/pageid.png)

### 頁面路徑資料元素

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

此程式碼會傳回AEM頁面的路徑。

![頁面路徑](assets/pagepath.png)

### 頁面標題資料元素

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

此程式碼會傳回AEM頁面的標題。

![頁面標題](assets/pagetitle.png)

## 疑難排解

### 為什麼我的mbox無法在我的網頁上觸發？

#### 未設定mboxDisable Cookie時出現錯誤訊息

![Target Cookie網域錯誤](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解決方案

Target客戶有時會搭配Target使用雲端型例項，以進行測試或簡單的概念證明用途。 這些網域和其他許多網域都是公用尾碼清單的一部分。
如果您使用這些網域，則現代瀏覽器不會儲存Cookie，除非您使用`targetGlobalSettings()`自訂`cookieDomain`設定。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 後續步驟

+ [將體驗片段匯出至Adobe Target](./export-experience-fragment-target.md)

## 支援連結

+ [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [使用Adobe用戶端資料層與核心元件檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform Debugger簡介](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html)