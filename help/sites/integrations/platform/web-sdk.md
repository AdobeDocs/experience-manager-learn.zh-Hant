---
title: 整合Experience PlatformWeb SDK
description: 了解如何整合AEMas a Cloud Service與Experience PlatformWeb SDK。 此基礎步驟對於整合Adobe Experience Cloud產品(例如Adobe Analytics、Target或Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer等最新創新產品)至關重要。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
source-git-commit: 3f129fb4fc53e55d118802d3a0e566a9a9bcb9a2
workflow-type: tm+mt
source-wordcount: '1153'
ht-degree: 1%

---


# 整合Experience PlatformWeb SDK

了解如何整合AEMas a Cloud Service與Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 此基礎步驟對於整合Adobe Experience Cloud產品(例如Adobe Analytics、Target或Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer等最新創新產品)至關重要。

您也會了解如何收集和傳送 [WKND — 範例Adobe Experience Manager專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) pageview中的資料 [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

完成此設定後，您可以繼續實作Experience Platform和相關應用程式，例如 [Real-time Customer Data Platform(Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html), [Customer Journey Analytics(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) 和 [Adobe Journey Optimizer(AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). 通過標準化Web和客戶資料來促進更好的客戶參與。

## 必備條件

整合Experience PlatformWeb SDK時，需要下列項目。

在 **AEM作為Cloud Service**:

+ AEM管理員對AEMas a Cloud Service環境的存取
+ Deployment Manager對Cloud Manager的訪問
+ 複製並部署 [WKND — 範例Adobe Experience Manager專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 至您的AEMas a Cloud Service環境。

在 **Experience Platform**:

+ 存取預設生產環境， **生產** 沙箱。
+ 存取 **結構** 在資料管理下
+ 存取 **資料集** 在資料管理下
+ 存取 **資料流** 在資料收集下
+ 存取 **標籤** （原稱為Launch）

如果您沒有必要的權限，您的系統管理員會使用 [Adobe Admin Console](https://adminconsole.adobe.com/) 可授予必要的權限。

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## 建立XDM結構 — Experience Platform

體驗資料模型(XDM)結構可協助您將客戶體驗資料標準化。 若要收集 **WKND頁面檢視** 資料、建立XDM結構，並使用Adobe提供的欄位群組 `AEP Web SDK ExperienceEvent` 用於網頁資料收集。


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

請前往 [XDM系統概觀](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

此 [XDM系統概觀](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) 是了解XDM結構和欄位群組、類型、類別和資料類型等相關概念的絕佳資源。 讓您全面了解XDM資料模型，以及如何建立和管理XDM結構，以標準化整個企業的資料。 深入了解XDM架構，以及其如何讓您的資料收集和管理程式受益。

## 建立DataStream -Experience Platform

DataStream會指示Platform Edge Network將收集的資料傳送至何處。 例如，可將其傳送至Experience Platform、Analytics或Adobe Target。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

請造訪 [資料流概觀](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) 頁面。

## 建立標籤屬性 — Experience Platform

了解如何在Experience Platform中建立標籤（舊稱為Launch）屬性，以將Web SDK JavaScript程式庫新增至WKND網站。 新定義的標籤屬性有下列資源：

+ 標籤擴充功能： [核心](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 和 [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ 資料元素：使用WKND網站的Adobe用戶端資料層擷取頁面名稱、網站區域和主機名稱的自訂程式碼類型資料元素。 此外，XDM物件類型資料元素也符合先前新建立的WKND XDM架構內建 [建立XDM結構](#create-xdm-schema---experience-platform) 步驟。
+ 規則：每當使用Adobe端資料層觸發來造訪WKND網頁時，將資料傳送至Platform Edge Network `cmp:show` 事件。


>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


+++ 資料元素和規則事件程式碼

+ 此 `Page Name` 資料元素程式碼。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ 此 `Site Section` 資料元素程式碼。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('repo:path')) {
   let pagePath = event.component['repo:path'];
   
   let siteSection = '';
   
   //Check of html String in URL.
   if (pagePath.indexOf('.html') > -1) { 
    siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
   
    //replace slash with colon
    siteSection = siteSection.replaceAll('/', ':');
   
    //remove `:content`
    siteSection = siteSection.replaceAll(':content:','');
   }
   
       return siteSection 
   }
   ```

+ 此 `Host Name` 資料元素程式碼。

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ 此 `all pages - on load` 規則事件程式碼

   ```javascript
   var pageShownEventHandler = function(evt) {
   // defensive coding to avoid a null pointer exception
   if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
       //trigger Launch Rule and pass event
       console.debug("cmp:show event: " + evt.eventInfo.path);
       var event = {
           //include the path of the component that triggered the event
           path: evt.eventInfo.path,
           //get the state of the component that triggered the event
           component: window.adobeDataLayer.getState(evt.eventInfo.path)
       };
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


此 [標籤概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 提供資料元素、規則和擴充功能等重要概念的深入知識。

如需整合AEM核心元件與Adobe用戶端資料層的詳細資訊，請參閱 [搭配使用Adobe用戶端資料層與AEM核心元件指南](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).

## 將標籤屬性連線至AEM

探索如何透過Adobe IMS和AEM中的Adobe啟動設定，將最近建立的標籤屬性連結至AEM。 建立AEMas a Cloud Service環境時，會自動產生數個Adobe IMS技術帳戶設定，包括Adobe啟動。 不過，若為AEM 6.5版，您必須手動設定。

連結標籤屬性後，WKND網站可使用Launch雲端服務設定，將標籤屬性的JavaScript程式庫載入至網頁。

### 驗證在WKND上載入的標籤屬性

使用Adobe Experience Platform Debugger [鉻黃](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 或 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 擴充功能，確認標籤屬性是否在WKND頁面上載入。 你可以驗證，

+ 標籤屬性詳細資訊，例如擴充功能、版本、名稱等。
+ Platform Web SDK程式庫版本，資料流ID
+ XDM物件作為一部分 `events` 屬性(在Experience PlatformWeb SDK中)

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## 建立資料集 — Experience Platform

使用Web SDK收集的頁面檢視資料會以資料集形式儲存在Experience Platform資料湖中。 資料集是資料集的儲存和管理結構，如遵循架構的資料庫表。 了解如何建立資料集，以及設定先前建立的資料流，以將資料傳送至Experience Platform。


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

此 [資料集概觀](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) 提供概念、設定和其他擷取功能的詳細資訊。


## WKND頁面檢視資料的Experience Platform

透過AEM設定Web SDK（尤其是在WKND網站上）後，您可以透過瀏覽網站頁面來產生流量。 然後確認頁面檢視資料已內嵌至Experience Platform資料集。 在資料集UI中，會顯示各種詳細資訊，例如記錄總數、大小和擷取批次，以及視覺上吸引人的長條圖。

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 摘要

做得好！您已完成使用Adobe Experience Platform(Experience Platform)Web SDK設定AEM，以收集和內嵌來自網站的資料。 透過此基礎，您現在可以探索進一步的可能性，以增強和整合Analytics、Target、Customer Journey Analytics(CJA)等產品，並為客戶建立豐富且個人化的體驗。 不斷學習和探索，充分發揮Adobe Experience Cloud的潛能。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

## 其他資源

+ [搭配核心元件使用Adobe用戶端資料層](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [整合Experience Platform資料收集標籤與AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK和邊緣網路概觀](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [資料收集教學課程](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger概觀](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

