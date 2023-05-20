---
title: 整合Experience PlatformWeb SDK
description: 瞭解如何將AEMas a Cloud Service與Experience PlatformWeb SDK整合。 這一基礎性步驟對於整合Adobe Experience Cloud產品(如Adobe Analytics、塔吉特或Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer等最新創新產品)至關重要。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
exl-id: b5182d35-ec38-4ffd-ae5a-ade2dd3f856d
source-git-commit: 63afa03de70d6f8f695d552018344d53a5cec6f5
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 1%

---

# 整合Experience PlatformWeb SDK

瞭解如何將AEMas a Cloud Service與Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html)。 這一基礎性步驟對於整合Adobe Experience Cloud產品(如Adobe Analytics、塔吉特或Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer等最新創新產品)至關重要。

您還將學習如何收集和發送 [WKND -Adobe Experience Manager項目示例](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 頁面查看資料 [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html)。

完成此設定後，您已實現了堅實的基礎。 另外，您已準備好使用應用程式(如 [Real-time Customer Data Platform(Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html)。 [Customer Journey Analytics語(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), [Adobe Journey Optimizer(AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html)。 高級實施有助於通過標準化Web和客戶資料來促進更好的客戶參與。

## 必備條件

整合Experience PlatformWeb SDK時，需要執行以下操作。

在 **AEM作為Cloud Service**:

+ 管AEM理員訪問AEMas a Cloud Service環境
+ 部署管理器對雲管理器的訪問
+ 克隆和部署 [WKND -Adobe Experience Manager項目示例](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 你的AEMas a Cloud Service。

在 **Experience Platform**:

+ 訪問預設生產， **生產** 沙盒。
+ 訪問 **架構** 在資料管理下
+ 訪問 **資料集** 在資料管理下
+ 訪問 **資料流** 在資料收集下
+ 訪問 **標籤** （以前稱為啟動）

如果您沒有必要的權限，系統管理員將使用 [Adobe Admin Console](https://adminconsole.adobe.com/) 可以授予必要的權限。

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## 建立XDM架構 — Experience Platform

體驗資料模型(XDM)架構可幫助您標準化客戶體驗資料。 收集 **WKND頁視圖** 資料，建立XDM架構並使用Adobe提供的欄位組 `AEP Web SDK ExperienceEvent` 的子菜單。

有一些通用的和特定的行業，例如零售、金融服務、醫療保健等，還有一套參考資料模型，請參見 [行業資料模型概述](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) 的子菜單。


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

瞭解XDM架構及相關概念，如欄位組、類型、類和資料類型 [XDM系統概述](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html)。

的 [XDM系統概述](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) 是瞭解XDM架構和相關概念（如欄位組、類型、類和資料類型）的絕佳資源。 它全面瞭解了XDM資料模型，以及如何建立和管理XDM架構，以在整個企業中標準化資料。 瞭解XDM架構，瞭解它如何使您的資料收集和管理過程受益。

## 建立資料流 — Experience Platform

資料流指示平台邊緣網路將收集到的資料發送到何處。 例如，它可以發送到Experience Platform或分析，或者Adobe Target。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

通過訪問 [資料流概述](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) 的子菜單。

## 建立標籤屬性 — Experience Platform

瞭解如何在Experience Platform中建立標籤（以前稱為Launch）屬性，以將Web SDK JavaScript庫添加到WKND網站。 新定義的標籤屬性具有以下資源：

+ 標籤擴展： [核心](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 和 [Adobe Experience PlatformWeb SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ 資料元素：使用WKND站點的Adobe客戶端資料層提取頁名、站點節和主機名的自定義代碼類型的資料元素。 此外，符合早期新建立的WKND XDM架構內置的XDM對象類型資料元素 [建立XDM架構](#create-xdm-schema---experience-platform) 的子菜單。
+ 規則：每當使用觸發的Adobe客戶端資料層訪問WKND網頁時，就將資料發送到平台邊緣網路 `cmp:show` 的子菜單。

使用 **發佈流**，可以使用 **添加所有更改的資源** 按鈕 要選擇所有資源，例如「資料元素」、「規則」和「標籤擴展」，而不是標識和挑選單個資源。 此外，在開發階段，您可以將庫發佈到 _開發_ 環境，然後驗證並升級到 _舞台_ 或 _生產_ 環境。

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>視頻中顯示的資料元素和規則事件代碼可供您參考， **展開下面的折疊元素**。 但是，如果您未使用Adobe客戶端資料層，則必須修改以下代碼，但定義資料元素並在規則定義中使用它們的概念仍然適用。


+++ 資料元素和規則事件代碼

+ 的 `Page Name` 資料元素代碼。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ 的 `Site Section` 資料元素代碼。

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

+ 的 `Host Name` 資料元素代碼。

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ 的 `all pages - on load` 規則 — 事件代碼

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


的 [標籤概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 提供了有關資料元素、規則和擴展等重要概念的深入知識。

有關將核心元件與AEMAdobe客戶端資料層整合的其他資訊，請參閱 [使用帶核心元件指南的Adobe客AEM戶端資料層](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)。

## 將標籤屬性連接到AEM

瞭解如何通過中的Adobe IMS和AEMAdobe啟動配置將最近建立的標籤屬性鏈AEM接到。 在建AEM立as a Cloud Service環境時，會自動生成多個Adobe IMS技術帳戶配置，包括Adobe啟動。 但是，對AEM於6.5版，必須手動配置一個。

連結標籤屬性後，WKND站點能夠使用Adobe啟動雲服務配置將標籤屬性的JavaScript庫載入到網頁上。

### 驗證WKND上載入的標籤屬性

使用Adobe Experience Platform調試器 [鉻](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 或 [火狐](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 擴展，驗證標籤屬性是否正在WKND頁上載入。 你可以驗證，

+ 標籤屬性詳細資訊，如擴展、版本、名稱等。
+ 平台Web SDK庫版本，資料流ID
+ XDM對象作為部件 `events` Experience PlatformWeb SDK中的屬性

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## 建立資料集 — Experience Platform

使用Web SDK收集的頁面視圖資料作為資料集儲存在Experience Platform資料湖中。 該資料集是儲存和管理構造，用於資料的集合，如模式後面的資料庫表。 瞭解如何建立資料集並配置先前建立的資料流以將資料發送到Experience Platform。


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

的 [資料集概述](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) 提供了有關概念、配置和其他攝取功能的詳細資訊。


## WKND頁查看Experience Platform中的資料

在Web SDK的安裝之AEM後，特別是在WKND站點上，是時候通過瀏覽網站頁面來生成流量了。 然後確認頁面視圖資料正被攝入到Experience Platform資料集中。 在資料集UI中，各種詳細資訊（如總記錄、大小和攝取的批）與直觀的條形圖一起顯示。

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 摘要

做得好！您已使用Experience PlatformWeb SDKAEM完成了安裝，以從網站收集和接收資料。 利用此基礎，您現在可以探索進一步的可能性來增強和整合分析、目標、Customer Journey Analytics(CJA)等產品，並為客戶建立豐富、個性化的體驗。 繼續學習和探索，充分發揮Adobe Experience Cloud的潛力。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

## 其他資源

+ [將Adobe客戶端資料層與核心元件一起使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [整合Experience Platform資料收集標AEM簽](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience PlatformWeb SDK和邊緣網路概述](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [資料收集教學課程](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform調試器概述](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
