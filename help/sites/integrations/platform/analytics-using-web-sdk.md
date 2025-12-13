---
title: 將AEM Sites和Adobe Analytics與Platform Web SDK整合
description: 使用現代的Platform Web SDK方法整合AEM Sites和Adobe Analytics。
version: Experience Manager as a Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2252
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 1%

---

# 將AEM Sites和Adobe Analytics與Platform Web SDK整合

瞭解&#x200B;**現代方法**，瞭解如何使用Platform Web SDK整合Adobe Experience Manager (AEM)和Adobe Analytics。 此全方位的教學課程會引導您完成順暢收集[WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)頁面檢視和CTA點按資料的程式。 透過在Adobe Analysis Workspace中將收集的資料視覺化，在其中您可以探索各種量度和維度，以獲得有價值的見解。 此外，請探索Platform資料集以驗證及分析資料。 加入我們的歷程，利用AEM和Adobe Analytics的強大功能進行資料導向式決策。

## 概觀

瞭解使用者行為是每個行銷團隊的重要目標。 透過瞭解使用者如何與其內容互動，團隊可以做出明智的決策、最佳化策略並帶來更好的結果。 WKND行銷團隊是虛構的實體，已著眼於在其網站上實作Adobe Analytics以實現此目標。 主要目標是收集兩個關鍵量度的資料：頁面檢視和首頁call-to-action (CTA)點按。

透過追蹤頁面檢視，團隊能夠分析哪些頁面最受使用者關注。 此外，追蹤首頁CTA點按次數可針對團隊call-to-action元素的成效提供寶貴見解。 此資料可能會揭示哪些CTA正在與使用者產生共鳴、哪些需要調整，並可能發掘提升使用者參與度並促進轉換的新機會。


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## 先決條件

使用Platform Web SDK整合Adobe Analytics時，需要下列專案。

您已完成&#x200B;**[整合Experience Platform Web SDK](./web-sdk.md)**&#x200B;教學課程的設定步驟。

在&#x200B;**AEM as Cloud Service**&#x200B;中：

+ [AEM管理員對AEM as a Cloud Service環境的存取權](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=zh-Hant)
+ 部署管理員對Cloud Manager的存取權
+ 複製並部署[WKND — 範例Adobe Experience Manager專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)至您的AEM as a Cloud Service環境。

在&#x200B;**Adobe Analytics**&#x200B;中：

+ 存取以建立&#x200B;**報告套裝**
+ 存取以建立&#x200B;**Analysis Workspace**

在&#x200B;**Experience Platform**&#x200B;中：

+ 存取預設生產&#x200B;**Prod**&#x200B;沙箱。
+ 存取資料管理下的&#x200B;**結構描述**
+ 存取資料管理下的&#x200B;**資料集**
+ 存取資料收集底下的&#x200B;**資料串流**
+ 存取資料收集底下的&#x200B;**標籤**

如果您沒有必要的許可權，使用[Adobe Admin Console](https://adminconsole.adobe.com/)的系統管理員可以授與必要的許可權。

使用Platform Web SDK來探討AEM與Analytics的整合程式之前，請&#x200B;_回顧在_&#x200B;整合Experience Platform Web SDK[教學課程中建立的基本元件和關鍵元素](./web-sdk.md)。 為整合提供堅實的基礎。

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

回顧XDM結構、資料流、資料集、標籤屬性以及AEM和標籤屬性連線後，我們就會展開整合歷程。

## 定義Analytics解決方案設計參考(SDR)檔案

在實作流程中，建議您建立解決方案設計參考(SDR)檔案。 此檔案對於定義業務需求和設計有效的資料收集策略至關重要。

SDR檔案提供實施計畫的全面概觀，確保所有利害關係人一致，並瞭解專案的目標和範圍。


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

如需有關SDR檔案中應包含的概念和各種元素的詳細資訊，請造訪[建立和維護解決方案設計參考(SDR)檔案](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html?lang=zh-Hant)。 您也可以下載範例Excel範本，不過[此處](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx)也提供WKND特定版本。

## 設定Analytics — 報表套裝、Analysis Workspace

第一步是設定Adobe Analytics，特別是使用轉換變數(或eVar)和成功事件的報告套裝。 轉換變數可用來測量原因和結果。 成功事件可用來追蹤動作。

在本教學課程中，`eVar5, eVar6, and eVar7`會分別追蹤&#x200B;_WKND頁面名稱、WKND CTA ID和WKND CTA名稱_，而`event7`則會用來追蹤&#x200B;_WKND CTA點選事件_。

為了分析、收集見解並和他人分享這些見解從收集的資料中，Analysis Workspace建立了專案。

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

若要進一步瞭解Analytics設定和概念，強烈建議使用下列資源：

+ [報告套裝](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=zh-Hant)
+ [轉換變數](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html?lang=zh-Hant)
+ [個成功事件](https://experienceleague.adobe.com/zh-hant/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html?lang=zh-Hant)

## 更新資料流 — 新增Analytics服務

資料串流會指示Platform Edge Network將收集到的資料傳送至何處。 在[先前的教學課程](./web-sdk.md)中，Datastream已設定為將資料傳送至Experience Platform。 此資料流已更新，以將資料傳送至在[以上](#setup-analytics---report-suite-analysis-workspace)步驟中設定的Analytics報表套裝。

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## 建立XDM結構描述

體驗資料模型(XDM)結構描述可協助您將收集的資料標準化。 在[先前的教學課程](./web-sdk.md)中，已建立具有`AEP Web SDK ExperienceEvent`欄位群組的XDM結構描述。 此外，使用此XDM結構描述會建立資料集，將收集的資料儲存在Experience Platform中。

但是，該XDM結構描述沒有Adobe Analytics特定的欄位群組來傳送eVar事件資料。 會建立新的XDM結構描述，而非更新現有結構描述，以避免將eVar事件資料儲存在平台中。

新建立的XDM結構描述具有`AEP Web SDK ExperienceEvent`和`Adobe Analytics ExperienceEvent Full Extension`欄位群組。

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## 更新標籤屬性

在[先前的教學課程](./web-sdk.md)中，已建立標籤屬性，其中包含資料元素和規則以收集、對應及傳送pageview資料。 必須為以下專案增強此功能：

+ 將頁面名稱對應至`eVar5`
+ 正在觸發&#x200B;**pageview** Analytics呼叫（或傳送信標）
+ 使用CTA使用者端資料層收集Adobe資料
+ 將CTA ID和名稱分別對應至`eVar6`和`eVar7`。 此外，CTA點按計數為`event7`
+ 正在觸發&#x200B;**連結點選** Analytics呼叫（或傳送信標）


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>影片中顯示的資料元素和規則事件程式碼可供您參考，**展開下列摺疊式功能表元素**。 不過，如果您未使用Adobe使用者端資料層，則必須修改下列程式碼，但仍適用定義資料元素及在規則定義中使用資料元素的概念。

+++ 資料元素和規則事件程式碼

+ `Component ID`資料元素程式碼。

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ `Component Name`資料元素程式碼。

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ `all pages - on load` **Rule-Condition**&#x200B;代碼

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ `home page - cta click` **規則事件**&#x200B;程式碼

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ `home page - cta click` **Rule-Condition**&#x200B;代碼

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

如需整合AEM核心元件與Adobe Client Data Layer的詳細資訊，請參閱[搭配使用Adobe Client Data Layer與AEM核心元件指南](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=zh-Hant)。


>[!INFO]
>
>若要全面瞭解Solution Design Reference (SDR)檔案中的&#x200B;**變數對應**&#x200B;索引標籤屬性詳細資訊，請在[這裡](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx)存取已完成的WKND特定版本以供下載。



## 驗證WKND上已更新的標籤屬性

以確保在WKND網站頁面上建置、發佈和正確使用更新的標籤屬性。 使用Google Chrome網頁瀏覽器的[Adobe Experience Platform Debugger擴充功能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)：

+ 若要確保標籤屬性是最新版本，請檢查建置日期。

+ 若要驗證PageView和HomePage CTA的XDM事件資料，請按一下，使用擴充功能中的Experience Platform Web SDK功能表選項。

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## 模擬網站流量 — Selenium自動化

為了產生有意義的流量用於測試目的，開發了Selenium自動化指令碼。 此自訂指令碼會模擬使用者與WKND網站的互動，例如頁面檢視和按一下CTA。

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## 資料集驗證 — WKND頁面檢視、CTA資料

資料集是資料集合的儲存和管理結構，例如遵循結構描述的資料庫表格。 在[先前的教學課程](./web-sdk.md)中建立的資料集會被重複使用，以驗證Pageview和CTA點選資料是否已擷取到Experience Platform資料集中。 在資料集UI中，各種詳細資訊（例如，記錄總數、大小和擷取的批次）會與視覺上吸引人的長條圖一起顯示。

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - WKND頁面檢視、CTA資料視覺效果

Analysis Workspace是Adobe Analytics中的強大工具，可讓您以靈活且互動的方式探索及視覺化資料。 它提供拖放介面，可建立自訂報表、執行進階分段並套用各種資料視覺效果。

讓我們重新開啟在[設定Analytics](#setup-analytics---report-suite-analysis-workspace)步驟中建立的Analysis Workspace專案。 在&#x200B;**最上層頁面**&#x200B;區段中，檢查各種量度，例如造訪、不重複訪客、登入、跳出率等。 若要評估WKND頁面和首頁CTA的效能，請拖放WKND特定的維度(WKND頁面名稱、WKND CTA名稱)和量度(WKND CTA點選事件)。 這些見解對於行銷人員瞭解哪些CTA更有效率，並根據其業務目標做出資料導向式決策非常有價值。

若要將使用者歷程視覺化，請使用流量視覺效果，從&#x200B;**WKND頁面名稱**&#x200B;開始，並展開至各種路徑。

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## 摘要

做得好！您已使用Platform Web SDK完成AEM和Adobe Analytics的設定，以收集、分析頁面檢視和CTA點按資料。

實施Adobe Analytics對於行銷團隊深入瞭解使用者行為、做出明智決策、協助他們最佳化內容並做出資料導向式決策至關重要。

透過實作建議步驟並使用提供的資源(例如解決方案設計參考(SDR)檔案)和瞭解重要的Analytics概念，行銷人員可以有效地收集和分析資料。

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>若您偏好涵蓋整個整合程式的&#x200B;**端對端影片**，而非個別設定步驟影片，您可以按一下[這裡](https://video.tv.adobe.com/v/3419889/)加以存取。


## 其他資源

+ [整合Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html?lang=zh-Hant)
+ [搭配核心元件使用Adobe使用者端資料層](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=zh-Hant)
+ [整合Experience Platform資料收集標籤和AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=zh-Hant)
+ [Adobe Experience Platform Web SDK和Edge Network概觀](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html?lang=zh-Hant)
+ [資料彙集教學課程](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html?lang=zh-Hant)
+ [Adobe Experience Platform Debugger概觀](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=zh-Hant)
