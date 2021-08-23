---
title: 使用AEM Assets和Adobe啟動設定Asset Insights
description: 在這5個部分影片系列中，我們會逐步說明透過Launch by Adobe部署的Experience Manager資產深入分析的設定和設定。
feature: 資產 Insights
version: 6.3, 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 1%

---


# 使用AEM Assets和Adobe Experience Platform Launch設定Asset Insights

在這5個部分影片系列中，我們會逐步說明透過Launch部署的Experience Manager資產深入分析的設定和設定。

## 第1部分：資產前瞻分析概述 {#overview}

資產前瞻分析概述。 安裝核心元件、範例影像元件和其他內容套件，讓您的環境準備就緒。

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### 架構圖 {#architecture-diagram}

![架構圖](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>請務必下載實作適用的[最新版核心元件](https://github.com/adobe/aem-core-wcm-components)。

影片使用核心元件v2.2.2，而非最新版本；請務必使用最新版本，再繼續前往下一節。

* 下載[資產前瞻分析範例影像內容](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 下載[最新AEM WCM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)

## 第2部分：為範例影像元件啟用資產前瞻分析追蹤 {#sample-image-component-asset-insights}

增強核心元件，以及為資產分析使用代理元件（範例影像元件）。 編輯內容頁面範本原則，以啟用參考網站的範例影像元件。

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>影像核心元件包含停用資產UUID（JCR內建立之節點的唯一識別碼值）追蹤功能，以停用UUID追蹤功能

核心影像元件在影像標籤的上層&lt;div>內使用&#x200B;***data-asset-id***&#x200B;屬性來啟用/停用此功能。 代理元件會以下列變更覆寫核心元件。

* 從image.html中&lt;img>元素的父div移除&#x200B;***data-asset-id***
* 直接將&#x200B;***data-aem-asset-id***&#x200B;新增至image.html中的&lt;img>元素
* 將&#x200B;***data-trackable=&#39;true&#39;***&#x200B;值新增至image.html中的&lt;img>元素
* ***data-aem-asset-*** id和 ***data-trackable=&#39;true&#39;*** 會保留在相同的節點層級

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* 和 *data-trackable=&#39;true&#39;* 為資產曝光數所需的重要屬性。若為「資產點擊前瞻分析」，除了&lt;img>標籤上出現的上述資料屬性外，父&lt;a>標籤必須具有有效的href值。

## 第3部分：Adobe Analytics — 建立報表套裝，啟用即時資料收集和AEM Assets報表 {#adobe-analytics-asset-insights}

系統會為資產追蹤建立即時資料收集的報表套裝。 AEM Assets Insights設定是使用Adobe Analytics憑證設定。

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
您必須為Adobe Analytics報表套裝啟用即時資料收集和AEM資產報表。 啟用AEM Asset Reporting會保留分析變數，以追蹤資產分析。

若為AEM Assets Insights設定，您需要下列憑證

* 資料中心
* Analytics公司名稱
* Analytics使用者名稱
* 共用機密(可從&#x200B;*Adobe Analytics >管理員>公司設定>網站服務*&#x200B;取得)。
* 報表套裝（請務必選取正確的報表套裝，以用於資產報表）

## 第4部分：使用Adobe Experience Platform Launch來新增Adobe Analytics擴充功能 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

新增Adobe Analytics擴充功能、建立頁面載入規則以及整合AEM與Launch與AdobeIMS技術帳戶。

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
請務必將所有變更從製作執行個體複製到發佈執行個體。

### 規則1 :頁面追蹤器(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

頁面追蹤器實作兩個回呼（在asset-embed-code中註冊）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;/code>** &lt;code>&lt;code>:為asset-DOM元素發送「載入」事件時呼叫。&lt;/code>&lt;/code>
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;/code>** &lt;code>&lt;code>:當資產DOM元素的「點按」事件發送時，才會呼叫，只有當資產DOM元素具有錨點標籤作為父項，且具有有效的外部「href」屬性時，才相關&lt;/code>&lt;/code>

最後，Pagetracker將初始化函式實作為。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:呼叫，以初始化Pagetracker元件。&lt;/code>&lt;/code> 必須先叫用此ID，才能從網頁產生任何資產深入分析事件（曝光次數和/或點按次數）。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:可選擇接受AppMeasurement物件 — 如果有提供，則不會嘗試建立AppMeasurement物件的新例項。&lt;/code>&lt;/code>

### 規則2:影像追蹤器 — 動作1(asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### 規則2:影像追蹤器 — 動作2(image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded():會在頁面載入完成時叫用，並會觸發所有可追蹤影像的資產曝光數
* 包含載入資產清單的Analytics變數：**contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked():當資產DOM元素具有具有有效href值的錨點標籤時，即會叫用。 點按資產時，系統會以點按的資產ID作為Cookie的值來建立。**(Cookie名稱：a.assets.clickedid)**
* 包含載入資產清單的Analytics變數：**contextData[&#39;c.a.assets.clickedid&#39;]**
* 來源：**contextData[&#39;c.a.assets.source&#39;]**

### 主控台除錯陳述式 {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

影片中會參照兩個Google Chrome瀏覽器擴充功能，作為Analytics除錯的方式。 其他瀏覽器也提供類似的擴充功能。

* [Launch Switch Chrome擴充功能](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

您也可以透過下列Chrome擴充功能，將DTM切換至除錯模式：[Launch和DTM交換機](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)。 這可讓您更輕鬆查看是否有任何與DTM部署相關的錯誤。 此外，您可以透過任何瀏覽器&#x200B;*開發人員工具 — > JS Console*&#x200B;手動將DTM切換為除錯模式，方法是新增下列程式碼片段：

## 第5部分：測試Analytic追蹤和同步分析資料{#analytics-tracking-asset-insights}

設定AEM Asset Reporting同步作業排程器和Assets Insights報表

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
