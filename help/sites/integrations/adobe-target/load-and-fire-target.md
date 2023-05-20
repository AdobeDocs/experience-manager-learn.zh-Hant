---
title: 載入並激發目標呼叫
description: 瞭解如何載入、將參數傳遞到頁面請求，以及使用啟動規則從站點頁面觸發目標調用。 使用Adobe客戶端資料層檢索並作為參數傳遞頁面資訊，該資料層允許您收集和儲存有關訪問者在網頁上的體驗的資料，然後使訪問此資料變得容易。
feature: Core Components, Adobe Client Data Layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: Cloud Service
kt: 6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 1%

---

# 載入並激發目標呼叫 {#load-fire-target}

瞭解如何載入、將參數傳遞到頁面請求，以及使用啟動規則從站點頁面觸發目標調用。 使用Adobe客戶端資料層檢索並作為參數傳遞網頁資訊，該資料層允許您收集和儲存有關訪問者體驗的資料，然後使訪問這些資料變得容易。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 頁面載入規則

Adobe客戶端資料層是事件驅動資料層。 載入頁AEM資料層時，它將觸發一個事件 `cmp:show` 。 在視頻中， `Launch Library Loaded` 使用自定義事件調用規則。 在下面，您可以找到視頻中用於自定義事件和資料元素的代碼片段。

### 自定義頁面顯示事件{#page-event}

![頁顯示事件配置和自定義代碼](assets/load-and-fire-target-call.png)

在「啟動」屬性中，添加新 **事件** 到 **規則**

+ __擴展：__ 核心
+ __事件類型：__ 自定義代碼
+ __名稱：__ 頁面顯示事件處理程式（或描述性內容）

點擊 __開啟編輯器__ 按鈕，在以下代碼段中貼上。 此代碼 __必須__ 被添加到 __事件配置__ 和 __操作__。

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

自定義函式定義 `pageShownEventHandler`，並監聽核心元件發AEM出的事件，獲取核心元件的相關資訊，將其打包到事件對象中，並在其負載處使用派生的事件資訊觸發啟動事件。

啟動規則使用啟動 `trigger(...)` 函式 __僅__ 可從規則的Event的Custom Code代碼段定義中獲得。

的 `trigger(...)` 函式將事件對象作為參數，在啟動資料元素中，該參數又通過啟動中另一個保留名稱公開，該名稱名為 `event`。 啟動中的資料元素現在可以從 `event` 對象使用語法 `event.component['someKey']`。

如果 `trigger(...)` 在事件的自定義代碼事件類型（例如，在操作中）的上下文之外使用，JavaScript錯誤 `trigger is undefined` 在與Launch屬性整合的網站上拋出。


### 資料元素

![資料元素](assets/data-elements.png)

Adobe啟動資料元素映射事件對象中的資料 [在自定義「顯示頁面」事件中觸發](#page-event) 通過核心擴展的自定義代碼資料元素類型將變數轉換為可用的Adobe Target變數。

#### 頁ID資料元素

```
if (event && event.id) {
    return event.id;
}
```

此代碼返回核心元件的生成唯一ID。

![頁ID](assets/pageid.png)

### 頁路徑資料元素

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

此代碼返AEM回頁面路徑。

![頁面路徑](assets/pagepath.png)

### 頁面標題資料元素

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

此代碼返AEM回頁面標題。

![頁面標題](assets/pagetitle.png)

## 疑難排解

### 為什麼我的郵箱不在我的網頁上開啟？

#### 未設定mboxDisablecookie時出現錯誤消息

![目標Cookie域錯誤](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解決方案

目標客戶有時使用基於雲的實例與目標一起進行測試或進行簡單的概念驗證。 這些域和許多其他域是「公共尾碼清單」的一部分。
如果您使用這些域，則現代瀏覽器將不保存Cookie，除非您自定義 `cookieDomain` 設定使用 `targetGlobalSettings()`。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 後續步驟

+ [向Adobe Target出口經驗](./export-experience-fragment-target.md)

## 支援連結

+ [Adobe客戶端資料層文檔](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud調試器 — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud調試器 — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [使用Adobe客戶端資料層和核心元件文檔](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform調試器簡介](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html)
