---
title: 載入及觸發Target呼叫
description: 瞭解如何使用標籤規則，將引數載入、傳遞至頁面請求，以及從您的網站頁面觸發Target呼叫。
feature: Core Components, Adobe Client Data Layer
version: Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 588
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 1%

---

# 載入及觸發Target呼叫 {#load-fire-target}

瞭解如何使用標籤規則，將引數載入、傳遞至頁面請求，以及從您的網站頁面觸發Target呼叫。 系統會使用Adobe使用者端資料層，擷取網頁資訊並以引數形式傳遞，此資料層可讓您收集和儲存訪客的網頁體驗相關資料，並且讓此資料易於存取。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 頁面載入規則

Adobe使用者端資料層是事件導向的資料層。 載入AEM Page資料層時，會觸發事件`cmp:show` 。 在影片中，使用自訂事件叫用`tags Library Loaded`規則。 您可以在下方找到視訊中用於自訂事件和資料元素的程式碼片段。

### 自訂頁面顯示事件{#page-event}

![頁面顯示的事件設定和自訂程式碼](assets/load-and-fire-target-call.png)

在標籤屬性中，將新的&#x200B;**事件**&#x200B;新增至&#x200B;**規則**

+ __延伸模組：__&#x200B;核心
+ __事件型別：__&#x200B;自訂程式碼
+ __名稱：__&#x200B;頁面顯示事件處理常式（或描述性內容）

點選&#x200B;__開啟編輯器__&#x200B;按鈕，然後貼入下列程式碼片段。 此程式碼&#x200B;__必須__&#x200B;新增至&#x200B;__事件組態__&#x200B;和後續的&#x200B;__動作__。

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the tags Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        // Trigger the tags Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other tags data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Data Collection, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

自訂函式定義`pageShownEventHandler`，並接聽AEM核心元件發出的事件、衍生核心元件的相關資訊、將其封裝成事件物件，並在其裝載處以衍生的事件資訊觸發標籤事件。

使用標籤的`trigger(...)`函式（僅&#x200B;__2}可在規則事件的自訂程式碼片段定義中使用）觸發標籤規則。__

`trigger(...)`函式將事件物件視為引數，而這個引數又在標籤「資料元素」中公開，並以標籤中名為`event`的另一個保留名稱公開。 標籤中的資料元素現在可以使用如`event.component['someKey']`的語法，從`event`物件參照此事件物件的資料。

如果在事件的Custom Code事件型別內容之外使用`trigger(...)` （例如，在動作中），則在與tags屬性整合的網站上擲回JavaScript錯誤`trigger is undefined`。


### 資料元素

![資料元素](assets/data-elements.png)

標籤資料元素會透過核心擴充功能的自訂程式碼資料元素型別，將自訂頁面顯示事件](#page-event)中觸發之事件物件[的資料對應至Adobe Target中的可用變數。

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

### 為何mbox沒有在我的網頁上觸發？

#### 未設定mboxDisable Cookie時的錯誤訊息

![目標Cookie網域錯誤](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解決方案

客戶有時使用雲端型例項搭配Target進行Target測試或簡單的概念證明用途。 這些網域和許多其他網域均屬於公用字尾清單。
如果您使用這些網域，則新式瀏覽器不會儲存Cookie，除非您使用`targetGlobalSettings()`自訂`cookieDomain`設定。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 後續步驟

+ [將體驗片段匯出至Adobe Target](./export-experience-fragment-target.md)

## 支援連結

+ [Adobe使用者端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [使用Adobe使用者端資料層和核心元件檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform Debugger簡介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
