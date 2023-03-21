---
title: 使用Adobe Analytics收集頁面資料
description: 使用事件導向的Adobe用戶端資料層，收集使用Adobe Experience Manager建置之網站上使用者活動的相關資料。 了解如何使用Experience Platform Launch中的規則來監聽這些事件，並將資料傳送至Adobe Analytics報表套裝。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: ef1fe712921bd5516cb389862cacf226a71aa193
workflow-type: tm+mt
source-wordcount: '2371'
ht-degree: 1%

---

# 使用Adobe Analytics收集頁面資料

了解如何使用 [Adobe用戶端資料層與AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) 收集Adobe Experience Manager Sites中某頁面的相關資料。 [Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) 和 [Adobe Analytics擴充功能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) 可用來建立規則，以將頁面資料傳送至Adobe Analytics。

## 您將建置的

![頁面資料追蹤](assets/collect-data-analytics/analytics-page-data-tracking.png)

在本教學課程中，您將根據Adobe用戶端資料層的事件觸發Launch規則、新增應觸發規則的條件，並傳送 **頁面名稱** 和 **頁面範本** 的AEM頁面。

### 目標 {#objective}

1. 根據資料層變更，在Launch中建立事件導向規則
1. 將頁面資料層屬性對應至Launch中的資料元素
1. 收集頁面資料並透過頁面檢視信標傳送至Adobe Analytics

## 必備條件

需要下列項目：

* **Experience Platform Launch** 屬性
* **Adobe Analytics** 測試/開發報表套裝ID和追蹤伺服器。 請參閱下列檔案，以了解 [建立新報表套裝](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform偵錯器](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) 瀏覽器擴充功能。 本教學課程中的螢幕擷取畫面是從Chrome瀏覽器擷取。
* （選用）AEM Site與 [Adobe用戶端資料層已啟用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). 本教學課程將使用公開的網站 [https://wknd.site/us/en.html](https://wknd.site/us/en.html) 但歡迎您使用自己的網站。

>[!NOTE]
>
> 需要整合Launch和您的AEM網站的協助嗎？ [請觀看此影片系列](../experience-platform/data-collection/tags/overview.md).

## 為WKND站點切換啟動環境

[https://wknd.site](https://wknd.site) 是以 [開放原始碼專案](https://github.com/adobe/aem-guides-wknd) 設計為參考， [教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hant) AEM實作。

您不必設定AEM環境並安裝WKND程式碼基底，而是可以使用Experience Platform偵錯器，以 **開關** 現場 [https://wknd.site/](https://wknd.site/) to *您的* 啟動屬性。 當然，如果您的AEM網站已具備 [Adobe用戶端資料層已啟用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. 登入Experience Platform Launch和 [建立Launch屬性](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html) （如果尚未）。
1. 確保初次啟動 [程式庫已建立](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) 並提升至Launch [環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. 從您的程式庫已發佈到的環境中複製Launch內嵌程式碼。

   ![複製Launch內嵌程式碼](assets/collect-data-analytics/launch-environment-copy.png)

1. 在您的瀏覽器中開啟新標籤，並導覽至 [https://wknd.site/](https://wknd.site/)
1. 開啟Experience Platform偵錯器瀏覽器擴充功能

   ![Experience Platform偵錯器](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 導覽至 **Launch** > **設定** 和 **插入的內嵌程式碼** 將現有的Launch內嵌程式碼取代為 *您的* 從步驟3複製的內嵌程式碼。

   ![取代內嵌程式碼](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 啟用 **主控台記錄** 和 **鎖定** WKND標籤上的除錯程式。

   ![主控台記錄](assets/collect-data-analytics/console-logging-lock-debugger.png)

## 驗證WKND站點上的Adobe客戶端資料層

此 [WKND參考專案](https://github.com/adobe/aem-guides-wknd) 是以AEM核心元件建置，且 [Adobe用戶端資料層已啟用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 依預設。 接下來，驗證Adobe客戶端資料層是否已啟用。

1. 導覽至 [https://wknd.site](https://wknd.site).
1. 開啟瀏覽器的開發人員工具，並導覽至 **主控台**. 執行下列命令：

   ```js
   adobeDataLayer.getState();
   ```

   這會傳回Adobe用戶端資料層的目前狀態。

   ![Adobe資料層狀態](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 展開回應並檢查 `page` 的下界。 您應會看到下列資料結構：

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   我們將使用衍生自 [頁面結構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page),  `dc:title`, `xdm:language` 和 `xdm:template` 將頁面資料傳送至Adobe Analytics的資料層。

   >[!NOTE]
   >
   > 看不到 `adobeDataLayer` javascript物件？ 確保 [Adobe客戶端資料層已啟用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 在您的網站上。

## 建立頁面載入規則

Adobe用戶端資料層是 **事件** 驅動資料層。 當AEM **頁面** 資料層已載入，將觸發事件 `cmp:show`. 建立根據 `cmp:show` 事件。

1. 導覽至Experience Platform Launch，並導覽至與AEM網站整合的Web屬性。
1. 導覽至 **規則** 區段，然後按一下 **建立新規則**.

   ![建立規則](assets/collect-data-analytics/analytics-create-rule.png)

1. 為規則命名 **已載入頁面**.
1. 按一下 **事件** **新增** 開啟 **事件設定** 嚮導。
1. 在 **事件類型** 選取 **自訂程式碼**.

   ![為規則命名並新增自訂程式碼事件](assets/collect-data-analytics/custom-code-event.png)

1. 按一下 **開啟編輯器** 在主面板中，輸入下列程式碼片段：

   ```js
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
   
         //Trigger the Launch Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   上述程式碼片段將借由 [推送函式](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 進入資料層。 當 `cmp:show` 事件已觸發 `pageShownEventHandler` 函式時，才會呼叫。 在此函式中，會新增一些健全性檢查，並新增 `event` 是使用最新 [資料層的狀態](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 用於觸發事件的元件。

   之後 `trigger(event)` 的URL。 `trigger()` 是Launch中的保留名稱，且將「觸發」Launch規則。 我們通過 `event` 物件，而此參數又會由Launch中另一個保留名稱公開，名稱為 `event`. Launch中的資料元素現在可以參照各種屬性，如下所示： `event.component['someKey']`.

1. 儲存變更。
1. 下一個 **動作** 按一下 **新增** 開啟 **動作設定** 嚮導。
1. 在 **動作類型** 選擇 **自訂程式碼**.

   ![自訂程式碼動作類型](assets/collect-data-analytics/action-custom-code.png)

1. 按一下 **開啟編輯器** 在主面板中，輸入下列程式碼片段：

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   此 `event` 物件是從 `trigger()` 方法在自訂事件中呼叫。 `component` 是從資料層衍生的目前頁面 `getState` 在自訂事件中。 回想一下 [頁面結構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) 由資料層公開，以便查看現成公開的各種索引鍵。

1. 儲存變更並執行 [建置](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 在Launch中，將程式碼推廣至 [環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) 用於AEM網站。

   >[!NOTE]
   >
   > 使用 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) 將內嵌程式碼切換為 **開發** 環境。

1. 導覽至您的AEM網站，並開啟開發人員工具以檢視主控台。 重新整理頁面，您應該會看到主控台訊息已記錄：

   ![頁面載入的主控台訊息](assets/collect-data-analytics/page-show-event-console.png)

## 建立資料元素

接著，建立數個資料元素，以從Adobe用戶端資料層擷取不同值。 如先前練習所示，我們已看到可透過自訂程式碼直接存取資料層的屬性。 使用資料元素的好處是，可在各個Launch規則中重複使用資料元素。

回想一下 [頁面結構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) 由資料層公開：

資料元素會對應至 `@type`, `dc:title`，和 `xdm:template` 屬性。

### 元件資源類型

1. 導覽至Experience Platform Launch，並導覽至與AEM網站整合的Web屬性。
1. 導覽至 **資料元素** 區段，按一下 **建立新資料元素**.
1. 針對 **名稱** 輸入 **元件資源類型**.
1. 針對 **資料元素類型** 選取 **自訂程式碼**.

   ![元件資源類型](assets/collect-data-analytics/component-resource-type-form.png)

1. 按一下 **開啟編輯器** 並在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   儲存變更。

   >[!NOTE]
   >
   > 回想一下 `event` 物件可供使用，且範圍取決於觸發 **規則** 在Launch中。 在資料元素為 *引用* 規則內。 因此，在規則內使用此資料元素是安全的，如 **已載入頁面** 在上一步中建立的規則 *但* 在其他情況下使用將不安全。

### 頁面名稱

1. 按一下 **新增資料元素**.
1. 針對 **名稱** 輸入 **頁面名稱**.
1. 針對 **資料元素類型** 選取 **自訂程式碼**.
1. 按一下 **開啟編輯器** 並在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   儲存變更。

### 頁面範本

1. 按一下 **新增資料元素**.
1. 針對 **名稱** 輸入 **頁面範本**.
1. 針對 **資料元素類型** 選取 **自訂程式碼**.
1. 按一下 **開啟編輯器** 並在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   儲存變更。

1. 您現在應該有三個資料元素作為規則的一部分：

   ![規則中的資料元素](assets/collect-data-analytics/data-elements-page-rule.png)

## 新增Analytics擴充功能

接下來，將Analytics擴充功能新增至您的Launch屬性。 我們需要將這些資料傳送到某處！

1. 導覽至Experience Platform Launch，並導覽至與AEM網站整合的Web屬性。
1. 前往 **擴充功能** > **目錄**
1. 找出 **Adobe Analytics** 擴充功能，按一下 **安裝**

   ![Adobe Analytics擴充功能](assets/collect-data-analytics/analytics-catalog-install.png)

1. 在 **程式庫管理** > **報表套裝**，請輸入您要用於每個Launch環境的報表套裝id。

   ![輸入報表套裝ID](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 在本教學課程中，您可以針對所有環境使用一個報表套裝，但在實際情況中，您會想要使用個別的報表套裝，如下圖所示

   >[!TIP]
   >
   >建議您使用 *為我管理程式庫選項* 作為「程式庫管理」設定，因為它可讓 `AppMeasurement.js` 程式庫的最新資訊。

1. 核取方塊以啟用 **使用Activity Map**.

   ![啟用使用Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. 在 **一般** > **追蹤伺服器**，請輸入您的追蹤伺服器，例如 `tmd.sc.omtrdc.net`. 如果您的網站支援，請輸入SSL追蹤伺服器 `https://`

   ![輸入追蹤伺服器](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 按一下 **儲存** 以儲存變更。

## 新增條件至頁面載入規則

接下來，更新 **已載入頁面** 規則來使用 **元件資源類型** 資料元素，以確保規則只在 `cmp:show` 事件適用於 **頁面**. 其他元件可以觸發 `cmp:show` 事件，例如當投影片變更時，輪播元件就會引發。 因此，請務必為此規則新增條件。

1. 在Launch UI中，導覽至 **已載入頁面** 規則。
1. 在 **條件** 按一下 **新增** 開啟 **條件設定** 嚮導。
1. 針對 **條件類型** 選取 **值比較**.
1. 將表單欄位中的第一個值設為 `%Component Resource Type%`. 您可以使用資料元素圖示 ![data-element圖示](assets/collect-data-analytics/cylinder-icon.png) ，選擇 **元件資源類型** 資料元素。 將比較器設定為 `Equals`.
1. 將第二個值設為 `wknd/components/page`.

   ![頁面載入規則的條件設定](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 可以在監聽的自訂程式碼函式中新增此條件 `cmp:show` 在教學課程先前建立的事件。 不過，在UI中新增該規則，可讓可能需要變更規則的其他使用者更清楚了解。 此外，我們還可以使用資料元素！

1. 儲存變更。

## 設定Analytics變數並觸發頁面檢視信標

目前 **已載入頁面** 規則只會輸出console陳述式。 接下來，使用資料元素和Analytics擴充功能，將Analytics變數設為 **動作** 在 **已載入頁面** 規則。 我們也會設定其他動作來觸發 **頁面檢視信標** 並將收集的資料傳送至Adobe Analytics。

1. 在 **已載入頁面** 規則 **移除** the **核心 — 自訂程式碼** 動作（控制台陳述式）:

   ![移除自訂程式碼動作](assets/collect-data-analytics/remove-console-statements.png)

1. 在「動作」底下，按一下 **新增** 以新增動作。
1. 設定 **擴充功能** 類型 **Adobe Analytics** 並設定 **動作類型** to  **設定變數**

   ![將動作擴充功能設為Analytics設定變數](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 在主面板中選取可用 **eVar** 並設為資料元素的值 **頁面範本**. 使用資料元素圖示 ![資料元素圖示](assets/collect-data-analytics/cylinder-icon.png) ，選擇 **頁面範本** 元素。

   ![設定為eVar頁面範本](assets/collect-data-analytics/set-evar-page-template.png)

1. 向下捲動，在下方 **其他設定** set **頁面名稱** 至資料元素 **頁面名稱**:

   ![頁面名稱環境變數集](assets/collect-data-analytics/page-name-env-variable-set.png)

   儲存變更。

1. 接下來，在 **Adobe Analytics — 設定變數** 點選 **plus** 圖示：

   ![新增其他Launch動作](assets/collect-data-analytics/add-additional-launch-action.png)

1. 設定 **擴充功能** 類型 **Adobe Analytics** 並設定 **動作類型** to  **傳送信標**. 由於這被視為頁面檢視，請將預設追蹤設為 **`s.t()`**.

   ![傳送信標Adobe Analytics動作](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 儲存變更。此 **已載入頁面** 規則現在應具備下列設定：

   ![最終啟動設定](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 聽 `cmp:show` 事件。
   * **2.** 檢查頁面是否觸發事件。
   * **3.** 為 **頁面名稱** 和 **頁面範本**
   * **4.** 傳送Analytics頁面檢視信標
1. 儲存所有變更並建置您的Launch程式庫，並提升至適當的環境。

## 驗證頁面檢視信標和Analytics呼叫

現在， **已載入頁面** 規則會傳送Analytics信標，則您應該可以使用Analytics Debugger查看Analytics追蹤變數。

1. 開啟 [WKND站點](https://wknd.site/us/en.html) 在瀏覽器中。
1. 按一下Debugger圖示 ![Experience Platform Debugger圖示](assets/collect-data-analytics/experience-cloud-debugger.png) 以開啟Experience Platform偵錯器。
1. 確認Debugger將Launch屬性對應至 *您的* 開發環境，如先前和 **主控台記錄** 已勾選。
1. 開啟Analytics功能表，並確認報表套裝已設為 *您的* 報表套裝。 「頁面名稱」也應填入：

   ![Analytics標籤除錯程式](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 向下捲動並展開 **網路請求**. 您應該能夠找到 **evar** 為 **頁面範本**:

   ![Evar和頁面名稱集](assets/collect-data-analytics/evar-page-name-set.png)

1. 返回瀏覽器，開啟開發人員主控台。 點進 **輪播** 頁面頂端。

   ![點進轉盤頁面](assets/collect-data-analytics/click-carousel-page.png)

1. 在瀏覽器主控台中觀察主控台陳述式：

   ![不符合條件](assets/collect-data-analytics/condition-not-met.png)

   這是因為轉盤會觸發 `cmp:show` 事件 *但* 因為我們檢查了 **元件資源類型**，則不會引發任何事件。

   >[!NOTE]
   >
   > 如果您沒有看見任何主控台記錄，請確定 **主控台記錄** 在下方勾選 **Launch** 在Experience Platform偵錯器中。

1. 導覽至文章頁面，如 [西澳大利亞](https://wknd.site/us/en/magazine/western-australia.html). 觀察「頁面名稱」和「範本類型」變更。

## 恭喜！

您只是使用事件導向的Adobe用戶端資料層和Experience Platform Launch，從AEM網站收集資料頁面資料，並將其傳送至Adobe Analytics。

### 後續步驟

查看下列教學課程，了解如何使用事件導向的Adobe用戶端資料層，以 [追蹤Adobe Experience Manager網站上特定元件的點按次數](track-clicked-component.md).
