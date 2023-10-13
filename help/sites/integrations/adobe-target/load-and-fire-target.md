---
title: 載入及觸發Target呼叫
description: 瞭解如何使用Launch規則來載入、傳遞引數至頁面請求，以及從您的網站頁面觸發Target呼叫。 系統會使用Adobe使用者端資料層，擷取頁面資訊並以引數形式傳遞，此資料層可讓您收集和儲存訪客的網頁體驗相關資料，並且讓此資料易於存取。
feature: Core Components, Adobe Client Data Layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
version: Cloud Service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 2%

---

# 載入及觸發Target呼叫 {#load-fire-target}

瞭解如何使用Launch規則來載入、傳遞引數至頁面請求，以及從您的網站頁面觸發Target呼叫。 系統會使用Adobe使用者端資料層，擷取網頁資訊並以引數形式傳遞，此資料層可讓您收集和儲存訪客的網頁體驗相關資料，並且讓此資料易於存取。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 頁面載入規則

Adobe使用者端資料層是事件導向的資料層。 載入AEM Page資料層時，會觸發事件 `cmp:show` . 在影片中， `Launch Library Loaded` 使用自訂事件叫用規則。 您可以在下方找到視訊中用於自訂事件和資料元素的程式碼片段。

### 自訂頁面顯示事件{#page-event}

![頁面顯示的事件設定和自訂程式碼](assets/load-and-fire-target-call.png)

在Launch屬性中，新增 **事件** 至 **規則**

+ __副檔名：__ 核心
+ __事件型別：__ 自訂程式碼
+ __名稱：__ 頁面顯示事件處理常式（或描述性內容）

點選 __開啟編輯器__ 按鈕，並貼入下列程式碼片段。 此程式碼 __必須__ 已新增至 __事件設定__ 和後續的 __動作__.

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

自訂函式會定義 `pageShownEventHandler`，會偵聽AEM核心元件發出的事件、衍生核心元件的相關資訊、將其封裝為事件物件，並在其裝載處以衍生的事件資訊觸發Launch事件。

Launch規則是使用Launch的 `trigger(...)` 函式： __僅限__ 可從規則事件的自訂程式碼片段定義中使用。

此 `trigger(...)` 函式會將事件物件視為引數，而引數會由Launch中另一個名為的保留名稱在Launch資料元素中公開 `event`. Launch中的資料元素現在可以從參照此事件物件資料的 `event` 物件，使用類似以下的語法 `event.component['someKey']`.

如果 `trigger(...)` 在事件的自訂程式碼事件型別內容之外使用（例如，在動作中），即JavaScript錯誤 `trigger is undefined` 在與Launch屬性整合的網站上擲回。


### 資料元素

![資料元素](assets/data-elements.png)

AdobeLaunch資料元素會對應事件物件中的資料 [在自訂頁面顯示事件中觸發](#page-event) 至可在Adobe Target中取得的變數，透過核心擴充功能的自訂程式碼資料元素型別。

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

![Target Cookie網域錯誤](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解決方案

客戶有時使用雲端型例項搭配Target進行Target測試或簡單的概念證明用途。 這些網域和許多其他網域均屬於公用字尾清單。
如果您使用這些網域，則現代瀏覽器不會儲存Cookie，除非您自訂 `cookieDomain` 設定，使用 `targetGlobalSettings()`.

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
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [使用Adobe使用者端資料層和核心元件檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform Debugger簡介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
