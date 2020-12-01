---
title: 使用AEM Assets和Adobe Launch設定資產見解
description: 在這5部影片系列中，我們逐步瞭解透過Launch By Adobe部署的Experience Manager資產分析的設定與設定。
contentOwner: selvaraj
feature: asset-insights
topics: integrations, development, metadata
audience: developer, architect, administrator
doc-type: article
activity: implement
version: 6.3, 6.4, 6.5
redirect-form: https://docs.adobe.com/content/help/en/experience-manager-learn/assets/analytics/asset-insights-launch-tutorial-setup.html
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---


# 使用AEM Assets和Adobe Experience Platform Launch設定資產見解

在這5部影片系列中，我們逐步瞭解透過Adobe Launch部署的Experience Manager資產分析的設定與設定。

## 第1部分：資產前瞻分析概述{#overview}

資產前瞻分析概述。 安裝核心元件、範例影像元件和其他內容套件，讓您的環境準備就緒。

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### 體系結構圖{#architecture-diagram}

![體系結構圖](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>請務必下載[最新版核心元件](https://github.com/adobe/aem-core-wcm-components)以供實施。

視訊使用的核心元件版本為2.2.2，不再是最新版本；請務必使用最新版本，然後再繼續下一節。

* 下載[資產前瞻分析範例影像內容](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 下載[最新的AEM WCM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)

## 第二部分：啟用範例影像元件{#sample-image-component-asset-insights}的資產前瞻分析追蹤

增強核心元件及資產前瞻分析使用代理元件（範例影像元件）。 編輯內容頁面範本原則，以啟用參考網站的範例影像元件。

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>影像核心元件包含停用UUID追蹤的功能，方法是停用資產的UUID（在JCR中建立的節點的唯一識別碼值）追蹤

核心影像元件在影像標籤的父&lt;div>內使用&#x200B;***data-asset-id***&#x200B;屬性來啟用／停用此功能。 Proxy元件會以下列變更覆寫核心元件。

* 從image.html中&lt;img>元素的父div移除&#x200B;***data-asset-id***
* 將&#x200B;***data-aem-asset-id***&#x200B;直接新增至image.html內的&lt;img>元素
* 將&#x200B;***data-trackable=&#39;true&#39;***&#x200B;值新增至image.html中的&lt;img>元素
* ***data-aem-asset-*** id和 ***data-trackable=&#39;true&#39;*** 維持在相同的節點層級

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;*  *and data-trackable=&#39;true&#39;* 是資產印象所需的關鍵屬性。對於資產點按前瞻分析，除了&lt;img>標籤上出現的上述資料屬性外，父&lt;a>標籤必須有有效的href值。

## 第3部分：Adobe Analytics — 建立報表套裝，啟用即時資料收集和AEM Assets報表{#adobe-analytics-asset-insights}

會針對資產追蹤建立包含即時資料收集的報表套裝。 AEM Assets Insights設定是使用Adobe Analytics認證來設定。

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
您的Adobe Analytics報表套裝必須啟用即時資料收集和AEM資產報表。 啟用AEM Asset Reporting會保留分析變數，以追蹤資產見解。

若為AEM Assets Insights設定，您需要下列認證

* 資料中心
* Analytics公司名稱
* Analytics使用者名稱
* 共用密碼（可從&#x200B;*Adobe Analytics >管理>公司設定>網站服務*&#x200B;取得）。
* 報表套裝（請確定選取用於資產報表的正確報表套裝）

## 第四部分：使用Adobe Experience Platform Launch新增Adobe Analytics擴充功能{#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

新增Adobe Analytics擴充功能、建立頁面載入規則以及將AEM與Launch整合至Adobe IMS技術帳戶。

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
請務必將您的所有變更從作者執行個體複製至發佈執行個體。

### 規則1:頁面追蹤器(pagetracker.js){#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

頁面追蹤器實作兩個回呼（已在資產內嵌代碼中註冊）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;/code>** &lt;code>&lt;code>:呼叫。&lt;/code>&lt;/code>
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;/code>** &lt;code>&lt;code>:呼叫時，只有當asset-DOM-element具有父項錨記且具有有效的外部&#39;href&#39;屬性時，才會對asset-DOM-element傳送&#39;click&#39;事件，而此情況相關&lt;/code>&lt;/code>

最後，Pagetracker將初始化函式實作。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:呼叫，以初始化Pagetracker元件。&lt;/code>&lt;/code> 必須在從網頁產生任何資產前瞻分析事件（印象和／或點按）之前呼叫此ID。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:（可選）接受AppMeasurement物件— 如果提供，則不會嘗試建立AppMeasurement物件的新例項。&lt;/code>&lt;/code>

### 規則2:影像追蹤器— 動作1(asset-insights.js){#rule-image-tracker-action-asset-insights-js}

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

### 規則2:影像追蹤器— 動作2(image-tracker.js){#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded():在頁面載入完成時呼叫，且會觸發所有可追蹤影像的資產印象
* 載入資產清單的Analytics變數：**contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked():在資產DOM元素具有有效href值的錨記時呼叫。 點按資產時，會以點按的資產ID為值建立Cookie。**(Cookie名稱：a.assets.clickedid)**
* 載入資產清單的Analytics變數：**contextData[&#39;c.a.assets.clickedid&#39;]**
* 來源：**contextData[&#39;c.a.assets.source&#39;]**

### 控制台調試語句{#console-debug-statements}

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

視訊中會參照兩個Google Chrome瀏覽器擴充功能，做為Analytics除錯的方式。 其他瀏覽器也提供類似的擴充功能。

* [啟動Switch Chrome Extension](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud除錯程式](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

您也可以使用下列Chrome擴充功能，將DTM切換至除錯模式：[啟動和DTM交換機](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)。 這可讓您更輕鬆地查看是否有與DTM部署相關的錯誤。 此外，您還可以透過任何瀏覽器&#x200B;*開發人員工具-> JS Console*，手動將DTM切換為除錯模式，方法是新增下列程式碼片段：

## 第五部分：測試分析追蹤與同步分析資料{#analytics-tracking-asset-insights}

設定AEM Asset Reporting Sync Job Scheduler和Assets Insights報表

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
