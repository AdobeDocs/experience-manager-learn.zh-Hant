---
title: 通過AEM Assets和Adobe發佈設定Asset Insights
description: 在這5個部分視頻系列中，我們將介紹Asset Insights的設定和配置，以便通過Launch by Adobe部署Experience Manager。
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---

# 與AEM Assets和Adobe Experience Platform Launch建立Asset Insights

在這5個部分視頻系列中，我們將介紹通過Adobe啟動部署的Experience ManagerAsset Insights的設定和配置。

## 第1部分：資產透視概覽 {#overview}

資產透視概覽。 安裝核心元件、示例映像元件和其他內容包，以使您的環境準備就緒。

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### 體系結構圖 {#architecture-diagram}

![體系結構圖](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>確保下載 [最新版本的核心元件](https://github.com/adobe/aem-core-wcm-components) 執行。

視頻使用的核心元件v2.2.2不再是最新版本；在繼續下一節之前，請務必使用最新版本。

* 下載 [Asset Insights示例影像內容](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 下載 [最新AEM的WCM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)

## 第二部分：為示例影像元件啟用Asset Insights跟蹤 {#sample-image-component-asset-insights}

對核心元件和使用代理元件（示例映像元件）進行Asset Insights增強。 編輯內容頁面模板策略以啟用參考站點的示例影像元件。

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>映像核心元件包括通過禁用對資產UUID（在JCR中建立的節點的唯一標識符值）的跟蹤來禁用UUID跟蹤的功能

核心映像元件使用 ***資料資產ID*** 父級中的屬性 &lt;div> 影像標籤中，啟用/禁用此功能。 代理元件將覆蓋核心元件，具有以下更改。

* 刪除 ***資料資產ID*** 從image.html中 &lt;img> 元素的父div
* 添加 ***資料 — 資產 — ID*** 直接到 &lt;img> image.html中的元素
* 添加 ***data trackable=&#39;true&#39;*** image.html &lt;img> 中元素的值
* ***資料 — 資產 — ID*** 和 ***data trackable=&#39;true&#39;*** 保持在同一節點級別

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* 和 *data trackable=&#39;true&#39;* 是資產印象所需的關鍵屬性。 對於資產按一下透視，除了標籤中存在的上述資料屬性外，父 &lt;img> 標籤必須具有有效的href值。

## 第三部分：Adobe Analytics — 建立報告套件，啟用即時資料收集和AEM Assets報告 {#adobe-analytics-asset-insights}

建立具有即時資料收集的報表套件以進行資產跟蹤。 AEM Assets洞察器配置是使用Adobe Analytics憑據設定的。

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
需要為您的Adobe Analytics報AEM告套件啟用即時資料收集和資產報告。 啟用AEMAsset Reporting會保留分析變數以跟蹤資產透視。

對於AEM Assets洞察器配置，您需要以下憑據

* 資料中心
* 分析公司名稱
* 分析用戶名
* 共用密鑰(可從 *Adobe Analytics>管理>公司設定> Web服務*)。
* 報告套件（確保選擇用於資產報告的正確報告套件）

## 第四部分：使用Adobe Experience Platform Launch添加Adobe Analytics分機 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

添加Adobe Analytics擴展、建立頁面載入規則和AEM與Adobe IMS技術帳戶整合。

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
確保將所有更改從作者實例複製到發佈實例。

### 規則1 :頁面跟蹤器(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

頁面跟蹤器實現兩個回叫（在asset-embed-code中註冊）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** :為asset-DOM-element調度「load」事件時調用。
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** :在為asset-DOM-element調度「click」事件時調用，僅當asset-DOM-element的錨點標籤具有有效的外部「href」屬性作為父項時，才與此相關

最後，Pagetracker將初始化函式實現為。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :已調用以初始化Pagetracker元件。 必須在從網頁生成任何資產透視事件（印象和/或點擊）之前調用此項。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :（可選）接受AppMeasurement對象 — 如果提供，則不會嘗試建立AppMeasurement對象的新實例。

### 規則2:影像跟蹤器 — 操作1(asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### 規則2:影像跟蹤器 — 操作2(image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded():在頁面載入完成時調用，並觸發所有可跟蹤映像的資產印象
* 攜帶已載入資產清單的分析變數： **上下文資料[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked():當資產DOM元素具有帶有有效href值的錨點標籤時調用。 按一下資產時，將建立以按一下的資產ID為其值的cookie。**(Cookie名稱：a.assets.clicked)**
* 攜帶已載入資產清單的分析變數： **上下文資料[「c.a.assets.clickedid」]**
* 來源： **上下文資料[&#39;c.a.assets.source&#39;]**

### 控制台調試語句 {#console-debug-statements}

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

視頻中引用了兩個GoogleChrome瀏覽器擴展，作為調試分析的方法。 其他瀏覽器也可使用類似的擴展。

* [啟動交換機Chrome擴展](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud調試器](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

還可以使用以下Chrome擴展將DTM切換到調試模式： [啟動和DTM交換機](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)。 這樣便更容易查看是否存在與DTM部署相關的錯誤。 此外，您還可以通過任何瀏覽器手動將DTM切換到調試模式 *開發人員工具 — > JS控制台* 添加以下代碼段：

## 第五部分：測試分析跟蹤和同步Insight資料{#analytics-tracking-asset-insights}

配置AEMAsset Reporting Sync作業計畫程式和Assets Insights報告

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
