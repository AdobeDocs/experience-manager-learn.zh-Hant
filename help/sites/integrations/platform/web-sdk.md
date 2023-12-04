---
title: 整合AEM Sites和Experience Platform Web SDK
description: 瞭解如何將AEM Sitesas a Cloud Service與Experience Platform Web SDK整合。 此基礎步驟對於整合Adobe Experience Cloud產品(例如Adobe Analytics、Target)或最近的創新產品(例如Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer)至關重要。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service " before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
duration: 1398
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1235'
ht-degree: 1%

---

# 整合AEM Sites和Experience Platform Web SDK

瞭解如何將AEMas a Cloud Service與Experience Platform整合 [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 此基礎步驟對於整合Adobe Experience Cloud產品(例如Adobe Analytics、Target)或最近的創新產品(例如Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer)至關重要。

您也會學習如何收集和傳送 [WKND — 範例Adobe Experience Manager專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) pageview資料於 [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

完成此設定後，您已實作堅實的基礎。 此外，您已準備好使用類似以下的應用程式來推進Experience Platform實施 [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=zh-Hant)， [Customer Journey Analytics(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html)、和 [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). 進階實作可標準化網頁和客戶資料，協助促進更佳的客戶參與度。

## 先決條件

整合Experience Platform Web SDK時，需要下列專案。

在 **AEM作為Cloud Service**：

+ AEM管理員對AEMas a Cloud Service環境的存取權
+ 部署管理員對Cloud Manager的存取權
+ 複製並部署 [WKND — 範例Adobe Experience Manager專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 至您的AEMas a Cloud Service環境。

在 **Experience Platform**：

+ 存取預設生產、 **Prod** 沙箱。
+ 存取目標 **方案** 在資料管理底下
+ 存取目標 **資料集** 在資料管理底下
+ 存取目標 **資料串流** 在「資料收集」底下
+ 存取目標 **標籤** （先前稱為Launch），位於「資料收集」下

如果您沒有必要的許可權，您的系統管理員使用 [Adobe Admin Console](https://adminconsole.adobe.com/) 可以授與必要的許可權。

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## 建立XDM結構描述 — Experience Platform

體驗資料模型(XDM)結構描述可幫助您標準化客戶體驗資料。 若要收集 **wknd頁面檢視** 資料，建立XDM結構描述並使用Adobe提供的欄位群組 `AEP Web SDK ExperienceEvent` 用於網頁資料收集。

有一般和特定產業，例如零售、金融服務、醫療保健等參照資料模型套件，請參閱 [產業資料模型概觀](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) 以取得詳細資訊。


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

瞭解XDM方案和相關的概念，如欄位群組、型別、類別和資料型別，從 [XDM系統概覽](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

此 [XDM系統概覽](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) 是瞭解XDM結構和相關概念（例如欄位群組、型別、類別和資料型別）的絕佳資源。 它提供對於XDM資料模型以及如何建立和管理XDM結構描述的全面瞭解，以標準化整個企業的資料。 探索它以更深入瞭解XDM結構描述，以及它如何能讓您的資料收集和管理流程受益。

## 建立資料串流 — Experience Platform

資料串流會指示Platform Edge Network將收集到的資料傳送至何處。 例如，可傳送至Experience Platform、Analytics或Adobe Target。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

如需瞭解資料串流的概念及相關主題，例如資料控管和設定，請造訪 [資料串流概觀](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) 頁面。

## 建立標籤屬性 — Experience Platform

瞭解如何在Experience Platform中建立標籤（先前稱為Launch）屬性，以將Web SDK JavaScript程式庫新增至WKND網站。 新定義的標籤屬性有下列資源：

+ 標籤擴充功能： [核心](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 和 [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ 資料元素：使用WKND網站的Adobe使用者端資料層擷取page-name、site-section和host-name的自訂程式碼型別資料元素。 此外，XDM物件型別資料元素也符合之前新建立的WKND XDM結構描述內建 [建立XDM結構描述](#create-xdm-schema---experience-platform) 步驟。
+ 規則：每當使用觸發的Adobe使用者端資料層造訪WKND網頁時，將資料傳送至Platform Edge Network `cmp:show` 事件。

使用建置和發佈標籤庫時 **發佈流程**，您可以使用 **新增所有變更的資源** 按鈕。 若要選取所有資源，例如資料元素、規則和標籤擴充功能，而非識別及挑選個別資源。 此外，在開發階段中，您可以將程式庫發佈到 _開發_ 環境，然後驗證並將其提升至 _階段_ 或 _生產_ 環境。

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>影片中顯示的資料元素和規則事件程式碼可供您參考， **展開下列摺疊式功能表元素**. 不過，如果您未使用Adobe使用者端資料層，則必須修改下列程式碼，但定義資料元素並在規則定義中使用這些元素的概念仍適用。


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


此 [標籤總覽](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 提供關於資料元素、規則和擴充功能等重要概念的深入知識。

如需整合AEM核心元件與Adobe使用者端資料層的詳細資訊，請參閱 [搭配使用Adobe Client Data Layer與AEM核心元件指南](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).

## 將標籤屬性連線至AEM

瞭解如何透過Adobe IMS和AEM中的AdobeLaunch設定，將最近建立的標籤屬性連結至AEM。 建立AEMas a Cloud Service環境時，會自動產生數個Adobe IMS技術帳戶設定，包括Adobe Launch。 不過，對於AEM 6.5版本，您必須手動設定。

連結標籤屬性後，WKND網站便能使用Adobe Launch雲端服務設定，將標籤屬性的JavaScript程式庫載入網頁。

### 驗證WKND上的標籤屬性載入

使用Adobe Experience Platform Debugger [鉻黃](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 或 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 擴充功能，驗證WKND頁面上是否載入標籤屬性。 您可以確認，

+ 標籤屬性詳細資訊，例如，擴充功能、版本、名稱等。
+ Platform Web SDK程式庫版本，資料流ID
+ XDM物件作為部分 `events` Experience Platform Web SDK中的屬性

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## 建立資料集 — Experience Platform

使用Web SDK收集的Pageview資料會以資料集的形式儲存在Experience Platform資料湖中。 資料集是資料集合的儲存和管理結構，例如遵循結構描述的資料庫表格。 瞭解如何建立資料集並設定先前建立的資料流，以將資料傳送至Experience Platform。


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

此 [資料集總覽](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) 提供有關概念、設定和其他擷取功能的詳細資訊。


## Experience Platform中的WKND pageview資料

在使用AEM設定Web SDK後，尤其是在WKND網站上，您可以導覽網站頁面來產生流量。 然後確認正在將Pageview資料擷取至Experience Platform資料集。 在資料集UI中，各種詳細資訊（例如，記錄總數、大小和擷取的批次）會與視覺上吸引人的長條圖一起顯示。

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 摘要

做得好！您已使用Experience Platform Web SDK完成AEM的設定，以收集並從網站擷取資料。 有了這個基礎，您現在可以探索更多可能性來增強和整合Analytics、Target、Customer Journey Analytics (CJA)和其他許多產品，為您的客戶打造豐富的個人化體驗。 持續學習和探索，以釋放Adobe Experience Cloud的完整潛能。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>如果您偏好使用 **端對端視訊** 涵蓋整個整合程式，而非個別設定步驟影片，您可以 [此處](https://video.tv.adobe.com/v/3418905/) 以存取它。

## 其他資源

+ [搭配核心元件使用Adobe使用者端資料層](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [整合Experience Platform資料收集標籤和AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK和Edge Network概觀](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [資料收集教學課程](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger概觀](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
