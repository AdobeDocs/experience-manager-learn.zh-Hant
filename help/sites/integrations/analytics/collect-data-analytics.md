---
title: 使用Adobe Analytics收集頁面資料
description: 使用事件導向的Adobe用戶端資料層，收集使用Adobe Experience Manager建立之網站的使用者活動資料。 瞭解如何使用Experience Platform Launch中的規則來監聽這些事件，並將資料傳送至Adobe Analytics報表套裝。
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
translation-type: tm+mt
source-git-commit: 64c167ec1d625fdd8be1bc56f7f5e59460b8fed3
workflow-type: tm+mt
source-wordcount: '2415'
ht-degree: 1%

---


# 使用Adobe Analytics收集頁面資料

瞭解如何搭配使用[Adobe用戶端資料層的內建功能與AEM核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html)來收集Adobe Experience Manager Sites中某頁面的相關資料。 [Experience Platform ](https://www.adobe.com/experience-platform/launch.html) Launch和 [Adobe Analytics擴](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) 充功能將用來建立規則，以傳送頁面資料至Adobe Analytics。

## 您將建立的

![頁面資料追蹤](assets/collect-data-analytics/analytics-page-data-tracking.png)

在本教學課程中，您將根據Adobe用戶端資料層的事件觸發啟動規則、新增規則觸發的條件，並將AEM頁面的&#x200B;**頁面名稱**&#x200B;和&#x200B;**頁面範本**&#x200B;傳送至Adobe Analytics。

### 目標{#objective}

1. 根據資料層的變更，在啟動中建立事件導向規則
1. 在Launch中將頁面資料層屬性對應至資料元素
1. 收集頁面資料，並使用頁面檢視信標傳送至Adobe Analytics

## 必備條件

以下為必要項目：

* **Experience Platform** LaunchProperty
* **Adobe** Analyticst/dev報表套裝ID和追蹤伺服器。請參閱以下檔案，瞭解如何建立新的報表套裝[。](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)
* [Experience Platform ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Debuggerbrowser擴充功能。本教學課程中的螢幕擷取自Chrome瀏覽器。
* （選用）啟用[Adobe用戶端資料層](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)的AEM網站。 本教學課程將使用公開對應網站[https://wknd.site/us/en.html](https://wknd.site/us/en.html)，但歡迎您使用您自己的網站。

>[!NOTE]
>
> 需要整合Launch和AEM網站的協助嗎？ [請參閱此影片系列](../experience-platform-launch/overview.md)。

## WKND站點的交換機啟動環境

[https://wknd.sites是以開放原始碼專案為基礎，](https://wknd.site) 為AEM實作 [提供參考和教](https://github.com/adobe/aem-guides-wknd)  [](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) 學課程的公開對開網站。

您不必設定AEM環境並安裝WKND程式碼基底，而是可以使用Experience Platform除錯程式來&#x200B;**switch**&#x200B;即時[https://wknd.site/](https://wknd.site/)至&#x200B;*您的*&#x200B;啟動屬性。 當然，如果您自己的AEM網站已啟用[Adobe用戶端資料層](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)，您可以使用它

1. 登入Experience Platform Launch並[建立Launch屬性](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html)（如果您尚未）。
1. 確保已建立初始的啟動[庫](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library)並升級到啟動[環境](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)。
1. 從您的資料庫已發佈至的環境中複製啟動內嵌代碼。

   ![複製啟動內嵌代碼](assets/collect-data-analytics/launch-environment-copy.png)

1. 在瀏覽器中開啟新標籤，並導覽至[https://wknd.site/](https://wknd.site/)
1. 開啟Experience Platform Debugger瀏覽器擴充功能

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 導覽至&#x200B;**Launch** > **Configuration**，並在&#x200B;**Incleted Embed Codes**&#x200B;下方，將現有的Launch內嵌代碼取代為從步驟3複製的&#x200B;*您的*&#x200B;內嵌代碼。

   ![取代內嵌代碼](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 在WKND頁籤上啟用&#x200B;**控制台日誌**&#x200B;和&#x200B;**鎖定調試器。**

   ![控制台記錄](assets/collect-data-analytics/console-logging-lock-debugger.png)

## 驗證WKND網站上的Adobe用戶端資料層

[WKND參考專案](https://github.com/adobe/aem-guides-wknd)是使用AEM核心元件建立，預設啟用[Adobe用戶端資料層](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。 接著，確認Adobe用戶端資料層已啟用。

1. 導覽至[https://wknd.site](https://wknd.site)。
1. 開啟瀏覽器的開發人員工具，並導覽至&#x200B;**Console**。 運行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   這會傳回Adobe用戶端資料層的目前狀態。

   ![Adobe資料層狀態](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 展開響應並檢查`page`條目。 您應看到如下的資料結構：

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: "WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world."
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   我們將使用從資料層的[頁面架構](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)、`dc:title`、`xdm:language`和`xdm:template`衍生的標準屬性，將頁面資料傳送至Adobe Analytics。

   >[!NOTE]
   >
   > 看不到`adobeDataLayer` javascript物件？ 請確定您的網站上已啟用[Adobe用戶端資料層](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 建立載入頁面的規則

Adobe用戶端資料層是&#x200B;**event**&#x200B;驅動的資料層。 載入AEM **Page**&#x200B;資料層時，會觸發事件`cmp:show`。 建立將根據`cmp:show`事件觸發的規則。

1. 導覽至Experience Platform Launch，並進入與AEM網站整合的Web屬性。
1. 導覽至「啟動UI」中的「**規則**」區段，然後按一下「建立新規則&#x200B;**」。**

   ![建立規則](assets/collect-data-analytics/analytics-create-rule.png)

1. 將規則命名為&#x200B;**已載入頁面**。
1. 按一下&#x200B;**事件** **添加**&#x200B;以開啟&#x200B;**事件配置**&#x200B;嚮導。
1. 在&#x200B;**事件類型**&#x200B;下，選擇&#x200B;**自訂代碼**。

   ![命名規則並新增自訂代碼事件](assets/collect-data-analytics/custom-code-event.png)

1. 按一下主面板中的&#x200B;**開啟編輯器**，然後輸入下列程式碼片段：

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

   上述程式碼片段將透過[將函式](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)推送至資料層來新增事件偵聽器。 觸發`cmp:show`事件時，會呼叫`pageShownEventHandler`函式。 在此函式中，添加一些例行性檢查，並使用觸發事件的元件的資料層[的最新狀態構造新的`event`。](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)

   在呼叫`trigger(event)`之後。 `trigger()` 是Launch中的保留名稱，將「觸發」啟動規則。我們將`event`物件傳遞為參數，而此參數將會在名為`event`的Launch中以另一個保留名稱公開。 Launch中的資料元素現在可以參照各種屬性，例如：`event.component['someKey']`。

1. 儲存變更。
1. 下一步，在&#x200B;**Actions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Action Configuration**&#x200B;嚮導。
1. 在「**動作類型**」下，選擇「自訂代碼&#x200B;**」。**

   ![自訂代碼動作類型](assets/collect-data-analytics/action-custom-code.png)

1. 按一下主面板中的&#x200B;**開啟編輯器**，然後輸入下列程式碼片段：

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   `event`物件是從自訂事件中呼叫的`trigger()`方法傳遞。 `component` 是自訂事件中資料層衍 `getState` 生的目前頁面。從之前的資料層公開的[頁面架構](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)中重新調用，以便看到各種鍵在包裝盒外公開。

1. 儲存變更並在Launch中執行[build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)，將程式碼提升至AEM網站上使用的[environment](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)。

   >[!NOTE]
   >
   > 使用[Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)將內嵌代碼切換至&#x200B;**Development**&#x200B;環境，會非常有用。

1. 導覽至您的AEM網站並開啟開發人員工具以檢視主控台。 重新整理頁面，您應該會看到主控台訊息已記錄：

   ![頁面載入的主控台訊息](assets/collect-data-analytics/page-show-event-console.png)

## 建立資料元素

接著，建立數個資料元素，從Adobe用戶端資料層擷取不同的值。 如前一練習中所述，我們可以直接透過自訂程式碼存取資料層的屬性。 使用資料元素的好處是，可跨啟動規則重複使用這些元素。

從資料層公開的[頁面架構](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)之前叫出：

資料元素將映射至`@type`、`dc:title`和`xdm:template`屬性。

### 元件資源類型

1. 導覽至Experience Platform Launch，並進入與AEM網站整合的Web屬性。
1. 導覽至&#x200B;**資料元素**&#x200B;區段，然後按一下&#x200B;**建立新資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**元件資源類型**。
1. 對於&#x200B;**資料元素類型**，選擇&#x200B;**自定義代碼**。

   ![元件資源類型](assets/collect-data-analytics/component-resource-type-form.png)

1. 按一下&#x200B;**開啟編輯器**，然後在自定義代碼編輯器中輸入以下內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   儲存變更。

   >[!NOTE]
   >
   > 請記住，`event`物件已設為可用，且範圍以啟動中觸發&#x200B;**Rule**&#x200B;的事件為基礎。 在規則中「資料元素」是&#x200B;*referenced*&#x200B;之前，不會設定「資料元素」的值。 因此，在規則內使用此資料元素是安全的，例如在前一步驟&#x200B;*中建立的&#x200B;**頁面載入**規則，但*&#x200B;在其他上下文中使用則不安全。

### 頁面名稱

1. 按一下&#x200B;**添加資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**頁面名稱**。
1. 對於&#x200B;**資料元素類型**，選擇&#x200B;**自定義代碼**。
1. 按一下&#x200B;**開啟編輯器**，然後在自定義代碼編輯器中輸入以下內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   儲存變更。

### 頁面範本

1. 按一下&#x200B;**添加資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**頁面模板**。
1. 對於&#x200B;**資料元素類型**，選擇&#x200B;**自定義代碼**。
1. 按一下&#x200B;**開啟編輯器**，然後在自定義代碼編輯器中輸入以下內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   儲存變更。

1. 您現在應該有三個資料元素作為規則的一部分：

   ![規則中的資料元素](assets/collect-data-analytics/data-elements-page-rule.png)

## 新增Analytics擴充功能

接著，將Analytics擴充功能新增至您的Launch屬性。 我們得把這些資料傳送到某處！

1. 導覽至Experience Platform Launch，並進入與AEM網站整合的Web屬性。
1. 前往&#x200B;**Extensions** > **Catalog**
1. 找到&#x200B;**Adobe Analytics**&#x200B;擴充功能，然後按一下&#x200B;**Install**

   ![Adobe Analytics Extension](assets/collect-data-analytics/analytics-catalog-install.png)

1. 在&#x200B;**資料庫管理** > **報表套裝**&#x200B;下，輸入您要用於每個啟動環境的報表套裝ID。

   ![輸入報表套裝ID](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 在本教學課程中，您可以針對所有環境使用一個報表套裝，但在實際生活中，您會想要使用個別的報表套裝，如下圖所示

   >[!TIP]
   >
   >我們建議使用&#x200B;*「管理我的程式庫」選項*&#x200B;作為「程式庫管理」設定，因為這可讓`AppMeasurement.js`程式庫保持最新狀態。

1. 選中此框以啟用&#x200B;**使用Activity Map**。

   ![啟用使用Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. 在「**一般** > **追蹤伺服器**」下，輸入您的追蹤伺服器，例如`tmd.sc.omtrdc.net`。 如果您的網站支援`https://`，請輸入您的SSL追蹤伺服器

   ![輸入追蹤伺服器](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 按一下&#x200B;**保存**&#x200B;保存更改。

## 新增條件至「載入頁面」規則

接著，更新&#x200B;**頁面載入**&#x200B;規則，以使用&#x200B;**元件資源類型**&#x200B;資料元素，確保只有當`cmp:show`事件用於&#x200B;**Page**&#x200B;時才會觸發規則。 其他元件可以觸發`cmp:show`事件，例如，當投影片變更時，轉盤元件會觸發它。 因此，為此規則新增條件很重要。

1. 在啟動UI中，導覽至先前建立的&#x200B;**頁面載入**&#x200B;規則。
1. 在&#x200B;**Conditions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Condition Configuration**&#x200B;嚮導。
1. 對於&#x200B;**條件類型**，選擇&#x200B;**值比較**。
1. 將表單欄位中的第一個值設為`%Component Resource Type%`。 您可以使用「資料元素」表徵圖![data-element表徵圖](assets/collect-data-analytics/cylinder-icon.png)來選擇「元件資源類型」資料元素。 ****&#x200B;將比較器設定為`Equals`。
1. 將第二個值設定為`wknd/components/page`。

   ![頁面載入規則的條件設定](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 您可在自訂程式碼函式中新增此條件，以監聽在教學課程之前建立的`cmp:show`事件。 不過，在UI中新增它可讓需要變更規則的其他使用者看到更多內容。 此外，我們還可使用我們的資料元素！

1. 儲存變更。

## 設定Analytics變數並觸發頁面檢視信標

目前，**Page Loaded**&#x200B;規則只會輸出控制台陳述式。 接著，使用資料元素和Analytics擴充功能，在&#x200B;**載入頁面**&#x200B;規則中，將Analytics變數設為&#x200B;**action**。 我們也會設定其他動作來觸發&#x200B;**頁面檢視信標**，並將收集的資料傳送至Adobe Analytics。

1. 在&#x200B;**Page Loaded**&#x200B;規則&#x200B;**remove**&#x200B;中&#x200B;**Core - Custom Code**&#x200B;動作（控制台語句）:

   ![移除自訂程式碼動作](assets/collect-data-analytics/remove-console-statements.png)

1. 在「操作」(Actions)下，按一下「添加」(Add)**以添加新操作。**
1. 將&#x200B;**Extension**&#x200B;類型設為&#x200B;**Adobe Analytics**，並將&#x200B;**Action Type**&#x200B;設為&#x200B;**Set Variables**

   ![將動作擴充功能設為Analytics集變數](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 在主面板中，選取可用的&#x200B;**eVar**，並設為「資料元素&#x200B;**頁面範本**」的值。 使用「資料元素」圖示![「資料元素」圖示](assets/collect-data-analytics/cylinder-icon.png)來選取「頁面範本」元素。****

   ![設為eVar頁面範本](assets/collect-data-analytics/set-evar-page-template.png)

1. 向下捲動，在&#x200B;**Additional Settings**&#x200B;下，將&#x200B;**Page Name**&#x200B;設為資料元素&#x200B;**Page Name**:

   ![頁面名稱環境變數集](assets/collect-data-analytics/page-name-env-variable-set.png)

   儲存變更。

1. 接著，點選&#x200B;**plus**&#x200B;圖示，在&#x200B;**Adobe Analytics - Set Variables**&#x200B;右側新增其他動作：

   ![新增其他啟動動作](assets/collect-data-analytics/add-additional-launch-action.png)

1. 將&#x200B;**Extension**&#x200B;類型設為&#x200B;**Adobe Analytics**，並將&#x200B;**動作類型**&#x200B;設為&#x200B;**傳送信標**。 由於此視為頁面檢視，請將預設追蹤設定保留為&#x200B;**`s.t()`**。

   ![傳送信標Adobe Analytics動作](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 儲存變更。**載入的頁面**&#x200B;規則現在應具備下列設定：

   ![最終啟動設定](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 聽聽活 `cmp:show` 動。
   * **2.** 檢查事件是否由頁面觸發。
   * **3.** 設定頁面名稱和頁 **面范** 本的 **分析變數**
   * **4.** 傳送分析頁面檢視信標
1. 儲存所有變更並建立您的啟動程式庫，並升級至適當的環境。

## 驗證頁面檢視信標和分析呼叫

現在，**頁面載入**&#x200B;規則會傳送Analytics信標，您應該可以使用Experience Platform除錯程式來查看Analytics追蹤變數。

1. 在瀏覽器中開啟[WKND站點](https://wknd.site/us/en.html)。
1. 按一下「除錯程式」圖示![「Experience Platform Debugger」圖示](assets/collect-data-analytics/experience-cloud-debugger.png)以開啟「Experience Platform Debugger」。
1. 請確定除錯程式正將Launch屬性對應至&#x200B;*您的*&#x200B;開發環境，如先前所述，且已勾選&#x200B;**控制台記錄**。
1. 開啟「Analytics」功能表，並確認報表套裝已設為&#x200B;*您的*&#x200B;報表套裝。 頁面名稱也應填入：

   ![Analytics標籤除錯程式](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 向下滾動並展開&#x200B;**網路請求**。 您應該可以找到&#x200B;**evar**&#x200B;為&#x200B;**頁面範本**&#x200B;設定的&lt;a0/>:

   ![Evar和頁面名稱設定](assets/collect-data-analytics/evar-page-name-set.png)

1. 返回瀏覽器並開啟開發人員主控台。 按一下頁面頂端的&#x200B;**轉盤**。

   ![點進轉盤頁面](assets/collect-data-analytics/click-carousel-page.png)

1. 在瀏覽器控制台中觀察控制台語句：

   ![不符合條件](assets/collect-data-analytics/condition-not-met.png)

   這是因為轉盤確實會觸發`cmp:show`事件&#x200B;*，但*，因為我們檢查了&#x200B;**元件資源類型**，所以不會引發任何事件。

   >[!NOTE]
   >
   > 如果您未看到任何控制台記錄，請確定已在Experience Platform Debugger的&#x200B;**Launch**&#x200B;下勾選了&#x200B;**Console Logging**。

1. 導覽至[Western Australia](https://wknd.site/us/en/magazine/western-australia.html)之類的文章頁面。 觀察頁面名稱和範本類型的變更。

## 恭喜！

您剛才使用事件導向的Adobe Client Data Layer和Experience Platform Launch從AEM網站收集資料頁面資料，並將它傳送至Adobe Analytics。

### 後續步驟

請參閱下列教學課程，瞭解如何使用事件導向的Adobe Client資料層[追蹤Adobe Experience Manager網站上特定元件的點按次數](track-clicked-component.md)。
