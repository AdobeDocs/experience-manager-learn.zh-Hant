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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '2375'
ht-degree: 1%

---

# 使用Adobe Analytics收集頁面資料

了解如何搭配AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)使用[Adobe用戶端資料層的內建功能，收集Adobe Experience Manager Sites中某頁面的相關資料。 [Experience Platform](https://www.adobe.com/experience-platform/launch.html) Launch和Adobe Analytics [擴](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) 充功能將用來建立規則，以將頁面資料傳送至Adobe Analytics。

## 您將建置的

![頁面資料追蹤](assets/collect-data-analytics/analytics-page-data-tracking.png)

在本教學課程中，您將根據來自Adobe用戶端資料層的事件觸發Launch規則，新增應觸發規則的條件，並將AEM頁面的&#x200B;**頁面名稱**&#x200B;和&#x200B;**頁面範本**&#x200B;傳送至Adobe Analytics。

### 目標 {#objective}

1. 根據資料層變更，在Launch中建立事件導向規則
1. 將頁面資料層屬性對應至Launch中的資料元素
1. 收集頁面資料並透過頁面檢視信標傳送至Adobe Analytics

## 必備條件

需要下列項目：

* **Experience Platform** LaunchProperty
* **Adobe** Analyticstest/dev報表套裝ID和追蹤伺服器。請參閱下列檔案，了解如何建立新的報表套裝](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。[
* [Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Debuggerbrowser擴充功能。本教學課程中的螢幕擷取畫面是從Chrome瀏覽器擷取。
* （選用）啟用[Adobe用戶端資料層的AEM網站](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。 本教學課程將使用公開的網站[https://wknd.site/us/en.html](https://wknd.site/us/en.html)，但歡迎您使用自己的網站。

>[!NOTE]
>
> 需要整合Launch和您的AEM網站的協助嗎？ [請觀看此影片系列](../experience-platform-launch/overview.md)。

## 為WKND站點切換啟動環境

[https://wknd.site](https://wknd.site) 是以開放原始碼專案為基礎所建置的 [公開](https://github.com/adobe/aem-guides-wknd) 網站，專為AEM實作提供參考和 [](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) 教學課程。

您不必設定AEM環境並安裝WKND程式碼基底，而是可以使用Experience Platform偵錯器來&#x200B;**switch**&#x200B;即時[https://wknd.site/](https://wknd.site/)偵錯至&#x200B;*您的* Launch屬性。 當然，如果您自己的AEM網站已啟用[Adobe用戶端資料層](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)，您就可以使用

1. 登入Experience Platform Launch並[建立Launch屬性](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/configure-launch/launch.html)（如果尚未登入）。
1. 請確定已建立初始的Launch [程式庫](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library)，並升級至Launch [environment](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html)。
1. 從您的程式庫已發佈到的環境中複製Launch內嵌程式碼。

   ![複製Launch內嵌程式碼](assets/collect-data-analytics/launch-environment-copy.png)

1. 在您的瀏覽器中開啟新標籤，並導覽至[https://wknd.site/](https://wknd.site/)
1. 開啟Experience Platform偵錯器瀏覽器擴充功能

   ![Experience Platform偵錯器](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 導覽至&#x200B;**Launch** > **Configuration**，並在&#x200B;**插入的內嵌程式碼**&#x200B;下方，將現有的Launch內嵌程式碼取代為從步驟3複製的&#x200B;*您的*&#x200B;內嵌程式碼。

   ![取代內嵌程式碼](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 在WKND頁簽上啟用&#x200B;**控制台日誌記錄**&#x200B;和&#x200B;**鎖定調試器**。

   ![主控台記錄](assets/collect-data-analytics/console-logging-lock-debugger.png)

## 驗證WKND站點上的Adobe客戶端資料層

[WKND參考專案](https://github.com/adobe/aem-guides-wknd)是使用AEM核心元件建置，預設會啟用[Adobe用戶端資料層](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。 接下來，驗證Adobe客戶端資料層是否已啟用。

1. 導覽至[https://wknd.site](https://wknd.site)。
1. 開啟瀏覽器的開發人員工具，並導覽至&#x200B;**主控台**。 執行下列命令：

   ```js
   adobeDataLayer.getState();
   ```

   這會傳回Adobe用戶端資料層的目前狀態。

   ![Adobe資料層狀態](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 展開回應並檢查`page`項目。 您應會看到下列資料結構：

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

   我們將使用衍生自資料層的[頁面結構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page)、`dc:title`、`xdm:language`和`xdm:template`的標準屬性，將頁面資料傳送至Adobe Analytics。

   >[!NOTE]
   >
   > 沒有看見`adobeDataLayer` javascript物件？ 請確定您的網站上已啟用[Adobe用戶端資料層](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 建立頁面載入規則

Adobe客戶端資料層是&#x200B;**event**&#x200B;驅動的資料層。 載入AEM **Page**&#x200B;資料層時，會觸發事件`cmp:show`。 建立將根據`cmp:show`事件觸發的規則。

1. 導覽至Experience Platform Launch，並導覽至與AEM網站整合的Web屬性。
1. 導覽至Launch UI中的&#x200B;**Rules**&#x200B;區段，然後按一下&#x200B;**建立新規則**。

   ![建立規則](assets/collect-data-analytics/analytics-create-rule.png)

1. 將規則命名為&#x200B;**Page Loaded**。
1. 按一下「**事件** **添加**」以開啟「**事件配置**」嚮導。
1. 在&#x200B;**事件類型**&#x200B;下，選擇&#x200B;**自訂代碼**。

   ![為規則命名並新增自訂程式碼事件](assets/collect-data-analytics/custom-code-event.png)

1. 按一下主面板中的&#x200B;**開啟編輯器** ，然後輸入下列程式碼片段：

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

   上述程式碼片段會透過[推送函式](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)至資料層來新增事件監聽器。 觸發`cmp:show`事件時，會呼叫`pageShownEventHandler`函式。 在此函式中，會新增一些健全性檢查，並針對觸發事件的元件，使用資料層](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)的最新[狀態來建構新的`event`。

   之後呼叫`trigger(event)`。 `trigger()` 是Launch中的保留名稱，且將「觸發」Launch規則。我們會以參數形式傳遞`event`物件，而此參數將會由Launch中名為`event`的其他保留名稱公開。 Launch中的資料元素現在可以參照各種屬性，如下所示：`event.component['someKey']`。

1. 儲存變更。
1. 下一步在&#x200B;**Actions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Action Configuration**&#x200B;精靈。
1. 在「**動作類型**」下，選擇「**自訂程式碼**」。

   ![自訂程式碼動作類型](assets/collect-data-analytics/action-custom-code.png)

1. 按一下主面板中的&#x200B;**開啟編輯器** ，然後輸入下列程式碼片段：

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   `event`物件是從自訂事件中呼叫的`trigger()`方法傳遞。 `component` 是從自訂事件中的資料層 `getState` 衍生的目前頁面。回想一下資料層公開的[頁面架構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page)之前的內容，以查看現成可用的各種索引鍵。

1. 儲存變更並在Launch中執行[build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html)，將程式碼推廣至AEM網站上使用的[environment](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html)。

   >[!NOTE]
   >
   > 使用[Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html)將內嵌程式碼切換至&#x200B;**開發**&#x200B;環境，會相當實用。

1. 導覽至您的AEM網站，並開啟開發人員工具以檢視主控台。 重新整理頁面，您應該會看到主控台訊息已記錄：

   ![頁面載入的主控台訊息](assets/collect-data-analytics/page-show-event-console.png)

## 建立資料元素

接著，建立數個資料元素，以從Adobe用戶端資料層擷取不同值。 如先前練習所示，我們已看到可透過自訂程式碼直接存取資料層的屬性。 使用資料元素的好處是，可在各個Launch規則中重複使用資料元素。

從資料層公開的[頁面架構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page)前面回想：

資料元素將對應至`@type`、`dc:title`和`xdm:template`屬性。

### 元件資源類型

1. 導覽至Experience Platform Launch，並導覽至與AEM網站整合的Web屬性。
1. 導覽至&#x200B;**資料元素**&#x200B;區段，然後按一下&#x200B;**建立新資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**元件資源類型**。
1. 對於&#x200B;**資料元素類型**，請選擇&#x200B;**自訂代碼**。

   ![元件資源類型](assets/collect-data-analytics/component-resource-type-form.png)

1. 按一下「**開啟編輯器**」，然後在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   儲存變更。

   >[!NOTE]
   >
   > 回想一下，`event`物件已可供使用，且範圍是根據Launch中觸發&#x200B;**Rule**&#x200B;的事件而定。 在規則內的資料元素為&#x200B;*referenced*&#x200B;之前，資料元素的值不會設定。 因此，在規則內使用此資料元素是安全的，例如在前一個步驟&#x200B;*中建立的&#x200B;**Page Loaded**規則，但在其他內容中使用*&#x200B;則不安全。

### 頁面名稱

1. 按一下「**新增資料元素**」。
1. 對於&#x200B;**名稱**，輸入&#x200B;**頁面名稱**。
1. 對於&#x200B;**資料元素類型**，請選擇&#x200B;**自訂代碼**。
1. 按一下「**開啟編輯器**」，然後在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   儲存變更。

### 頁面範本

1. 按一下「**新增資料元素**」。
1. 對於&#x200B;**名稱**，輸入&#x200B;**頁面範本**。
1. 對於&#x200B;**資料元素類型**，請選擇&#x200B;**自訂代碼**。
1. 按一下「**開啟編輯器**」，然後在自訂程式碼編輯器中輸入下列內容：

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
1. 前往&#x200B;**擴充功能** > **目錄**
1. 找到&#x200B;**Adobe Analytics**&#x200B;擴充功能，然後按一下&#x200B;**Install**

   ![Adobe Analytics擴充功能](assets/collect-data-analytics/analytics-catalog-install.png)

1. 在「**程式庫管理** > **報表套裝**」下方，輸入您要用於每個Launch環境的報表套裝ID。

   ![輸入報表套裝ID](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 在本教學課程中，您可以針對所有環境使用一個報表套裝，但在實際情況中，您會想要使用個別的報表套裝，如下圖所示

   >[!TIP]
   >
   >建議您使用&#x200B;*為我管理程式庫選項*&#x200B;作為「程式庫管理」設定，因為這可讓`AppMeasurement.js`程式庫更容易保持最新。

1. 核取方塊以啟用&#x200B;**使用Activity Map**。

   ![啟用使用Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. 在&#x200B;**一般** > **追蹤伺服器**&#x200B;下，輸入您的追蹤伺服器，例如`tmd.sc.omtrdc.net`。 如果您的網站支援`https://`，請輸入SSL追蹤伺服器

   ![輸入追蹤伺服器](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 按一下&#x200B;**儲存**&#x200B;以儲存變更。

## 新增條件至頁面載入規則

接下來，更新&#x200B;**Page Loaded**&#x200B;規則，以使用&#x200B;**元件資源類型**&#x200B;資料元素，確保只有在`cmp:show`事件為&#x200B;**Page**&#x200B;時才會觸發規則。 其他元件可以引發`cmp:show`事件，例如當投影片變更時，輪播元件就會引發。 因此，請務必為此規則新增條件。

1. 在Launch UI中，導覽至先前建立的&#x200B;**Page Loaded**&#x200B;規則。
1. 在&#x200B;**Conditions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Condition Configuration**&#x200B;精靈。
1. 對於&#x200B;**條件類型**，選擇&#x200B;**值比較**。
1. 將表單欄位中的第一個值設為`%Component Resource Type%`。 您可以使用資料元素圖示![data-element圖示](assets/collect-data-analytics/cylinder-icon.png)來選取&#x200B;**元件資源類型**&#x200B;資料元素。 將比較器設定為`Equals`。
1. 將第二個值設為`wknd/components/page`。

   ![頁面載入規則的條件設定](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 可以在自訂程式碼函式中新增此條件，該函式會監聽先前在教學課程中建立的`cmp:show`事件。 不過，在UI中新增該規則，可讓可能需要變更規則的其他使用者更清楚了解。 此外，我們還可以使用資料元素！

1. 儲存變更。

## 設定Analytics變數並觸發頁面檢視信標

目前，**Page Loaded**&#x200B;規則只會輸出主控台陳述式。 接下來，使用資料元素和Analytics擴充功能，將Analytics變數設為&#x200B;**Page Loaded**&#x200B;規則中的&#x200B;**action**。 我們也會設定其他動作，以觸發&#x200B;**頁面檢視信標**，並將收集的資料傳送至Adobe Analytics。

1. 在&#x200B;**Page Loaded**&#x200B;規則&#x200B;**remove**&#x200B;中， **Core - Custom Code**&#x200B;動作（主控台陳述式）:

   ![移除自訂程式碼動作](assets/collect-data-analytics/remove-console-statements.png)

1. 在「動作」底下，按一下&#x200B;**Add**&#x200B;以新增動作。
1. 將&#x200B;**Extension**&#x200B;類型設定為&#x200B;**Adobe Analytics**，並將&#x200B;**Action Type**&#x200B;設定為&#x200B;**Set Variables**

   ![將動作擴充功能設為Analytics設定變數](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 在主面板中，選取可用的&#x200B;**eVar**，並設為資料元素&#x200B;**頁面範本**&#x200B;的值。 使用資料元素表徵圖![資料元素表徵圖](assets/collect-data-analytics/cylinder-icon.png)選擇&#x200B;**頁面模板**&#x200B;元素。

   ![設定為eVar頁面範本](assets/collect-data-analytics/set-evar-page-template.png)

1. 向下捲動，在&#x200B;**Additional Settings**&#x200B;下，將&#x200B;**Page Name**&#x200B;設定為資料元素&#x200B;**Page Name**:

   ![頁面名稱環境變數集](assets/collect-data-analytics/page-name-env-variable-set.png)

   儲存變更。

1. 接下來，點選&#x200B;**加號**&#x200B;圖示，在&#x200B;**Adobe Analytics — 設定變數**&#x200B;的右側新增其他動作：

   ![新增其他Launch動作](assets/collect-data-analytics/add-additional-launch-action.png)

1. 將&#x200B;**Extension**&#x200B;類型設為&#x200B;**Adobe Analytics**，並將&#x200B;**Action Type**&#x200B;設為&#x200B;**Send Beacon**。 由於這被視為頁面檢視，請將預設追蹤設為&#x200B;**`s.t()`**。

   ![傳送信標Adobe Analytics動作](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 儲存變更。**Page Loaded**&#x200B;規則現在應具備下列設定：

   ![最終啟動設定](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 聽取事 `cmp:show` 件。
   * **2.** 檢查頁面是否觸發事件。
   * **3.** 為頁面名稱和 **頁面** 範本設 **定Analytics變數**
   * **4.** 傳送Analytics頁面檢視信標
1. 儲存所有變更並建置您的Launch程式庫，並提升至適當的環境。

## 驗證頁面檢視信標和Analytics呼叫

現在，**頁面載入**&#x200B;規則會傳送Analytics信標，您應該可以使用Experience Platform偵錯器查看Analytics追蹤變數。

1. 在瀏覽器中開啟[WKND網站](https://wknd.site/us/en.html)。
1. 按一下Debugger圖示![Experience Platform Debugger圖示](assets/collect-data-analytics/experience-cloud-debugger.png)以開啟Experience PlatformDebugger。
1. 如先前所述，確認Debugger將Launch屬性對應至&#x200B;*您的*&#x200B;開發環境，且已勾選&#x200B;**主控台記錄**。
1. 開啟Analytics功能表，並確認報表套裝已設為&#x200B;*您的*&#x200B;報表套裝。 「頁面名稱」也應填入：

   ![Analytics標籤除錯程式](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 向下捲動並展開&#x200B;**網路請求**。 您應該能夠找到為&#x200B;**頁面範本**&#x200B;設定的&#x200B;**evar**:

   ![Evar和頁面名稱集](assets/collect-data-analytics/evar-page-name-set.png)

1. 返回瀏覽器，開啟開發人員主控台。 按一下頁面頂端的&#x200B;**轉盤**。

   ![點進轉盤頁面](assets/collect-data-analytics/click-carousel-page.png)

1. 在瀏覽器主控台中觀察主控台陳述式：

   ![不符合條件](assets/collect-data-analytics/condition-not-met.png)

   這是因為轉盤確實會觸發`cmp:show`事件&#x200B;*，但*，因為我們檢查了&#x200B;**元件資源類型**，所以沒有觸發任何事件。

   >[!NOTE]
   >
   > 如果您沒有看見任何主控台記錄，請確定已在Experience Platform偵錯器的&#x200B;**Launch**&#x200B;下勾選&#x200B;**主控台記錄**。

1. 導覽至[Western Australia](https://wknd.site/us/en/magazine/western-australia.html)之類的文章頁面。 觀察「頁面名稱」和「範本類型」變更。

## 恭喜！

您只是使用事件導向的Adobe用戶端資料層和Experience Platform Launch，從AEM網站收集資料頁面資料，並將其傳送至Adobe Analytics。

### 後續步驟

查看下列教學課程，了解如何使用事件導向的Adobe用戶端資料層[追蹤Adobe Experience Manager網站上特定元件的點按](track-clicked-component.md)。
