---
title: 使用Adobe Analytics追蹤已點按的元件
description: 使用事件導向的Adobe用戶端資料層來追蹤Adobe Experience Manager網站上特定元件的點按次數。 了解如何使用標籤規則來監聽這些事件，並使用追蹤連結信標將資料傳送至Adobe Analytics報表套裝。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '1885'
ht-degree: 1%

---

# 使用Adobe Analytics追蹤已點按的元件

>[!NOTE]
>
>Adobe Experience Platform Launch在Adobe Experience Platform中已重新命名為一套資料收集技術。 因此，產品檔案中已推出數個術語變更。 請參閱下列內容 [檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 以匯整術語變更的參考。

使用事件導向 [Adobe用戶端資料層與AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) 追蹤Adobe Experience Manager網站上特定元件的點按次數。 了解如何使用標籤屬性中的規則來監聽點按事件、依元件篩選資料，以及透過追蹤連結信標將資料傳送至Adobe Analytics。

## 您要建置的 {#what-build}

WKND行銷團隊有興趣了解 `Call to Action (CTA)` 按鈕在首頁上的執行效果最佳。 在本教學課程中，將規則新增至監聽的標籤屬性 `cmp:click` 來自 **Teaser** 和 **按鈕** 元件。 接著，將元件ID和新事件連同追蹤連結信標一併傳送至Adobe Analytics。

![建立追蹤點按次數的項目](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目標 {#objective}

1. 在擷取 `cmp:click` 事件。
1. 依元件資源類型篩選不同的事件。
1. 設定元件ID，並透過追蹤連結信標將事件傳送至Adobe Analytics。

## 必備條件

本教學課程是 [使用Adobe Analytics收集頁面資料](./collect-data-analytics.md) 並假設您擁有：

* A **標籤屬性** 和 [Adobe Analytics擴充功能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) 已啟用
* **Adobe Analytics** 測試/開發報表套裝ID和追蹤伺服器。 請參閱下列檔案，以了解 [建立報表套裝](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform偵錯器](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 瀏覽器擴充功能已在上載入您的標籤屬性 [WKND站點](https://wknd.site/us/en.html) 或啟用「Adobe資料層」的AEM網站。

## Inspect Button與Teaser結構

在標籤屬性中建立規則之前，請先檢閱 [按鈕和Teaser的結構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) 並在資料層實作中加以檢查。

1. 導覽至 [WKND首頁](https://wknd.site/us/en.html)
1. 開啟瀏覽器的開發人員工具，並導覽至 **主控台**. 執行下列命令：

   ```js
   adobeDataLayer.getState();
   ```

   上述程式碼會傳回Adobe用戶端資料層的目前狀態。

   ![透過瀏覽器主控台的資料層狀態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 展開回應，並尋找前置詞為的項目 `button-` 和  `teaser-xyz-cta` 的下界。 您應會看到下列資料結構：

   按鈕結構：

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   預告結構：

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   以上資料詳細資料以 [元件/容器項目結構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). 新標籤規則會使用此結構。

## 建立CTA點按規則

Adobe用戶端資料層是 **事件** 驅動資料層。 只要按一下任何核心元件， `cmp:click` 事件會透過資料層發送。 聽 `cmp:click` 事件，讓我們建立規則。

1. 導覽至Experience Platform，並導覽至與AEM網站整合的標籤屬性。
1. 導覽至 **規則** 區段，然後按一下 **新增規則**.
1. 為規則命名 **已點按CTA**.
1. 按一下 **事件** > **新增** 開啟 **事件設定** 嚮導。
1. 針對 **事件類型** 欄位，選擇 **自訂程式碼**.

   ![將規則命名為「CTA已點按」並新增自訂程式碼事件](assets/track-clicked-component/custom-code-event.png)

1. 按一下 **開啟編輯器** 在主面板中，輸入下列程式碼片段：

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
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

   上述程式碼片段透過 [推送函式](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 進入資料層。 每當 `cmp:click` 事件已觸發 `componentClickedHandler` 函式時，才會呼叫。 在此函式中，會新增一些健全性檢查，並新增 `event` 物件是使用最新的 [資料層的狀態](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 用於觸發事件的元件。

   最後 `trigger(event)` 函式時，才會呼叫。 此 `trigger()` 函式是標籤屬性中的保留名稱，且 **觸發** 規則。 此 `event` 物件會以參數的形式傳遞，而參數又會由標籤屬性中的其他保留名稱公開。 標籤屬性中的資料元素現在可以使用程式碼片段(例如 `event.component['someKey']`.

1. 儲存變更。
1. 下一個 **動作** 按一下 **新增** 開啟 **動作設定** 嚮導。
1. 針對 **動作類型** 欄位，選擇 **自訂程式碼**.

   ![自訂程式碼動作類型](assets/track-clicked-component/action-custom-code.png)

1. 按一下 **開啟編輯器** 在主面板中，輸入下列程式碼片段：

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   此 `event` 物件是從 `trigger()` 方法在自訂事件中呼叫。 此 `component` 對象是從資料層派生的元件的當前狀態 `getState()` 方法和是觸發點按的元素。

1. 儲存變更並執行 [建置](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 在標籤屬性中，將程式碼提升至 [環境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) 用於AEM網站。

   >[!NOTE]
   >
   > 使用 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 將內嵌程式碼切換為 **開發** 環境。

1. 導覽至 [WKND站點](https://wknd.site/us/en.html) 並開啟開發人員工具以檢視主控台。 此外，請選取 **保留日誌** 核取方塊。

1. 按一下 **Teaser** 或 **按鈕** CTA按鈕，可導覽至其他頁面。

   ![要點按的CTA按鈕](assets/track-clicked-component/cta-button-to-click.png)

1. 在開發人員主控台中，觀察 **已點按CTA** 規則已引發：

   ![已點按CTA按鈕](assets/track-clicked-component/cta-button-clicked-log.png)

## 建立資料元素

接著建立資料元素，以擷取已點按的元件ID和標題。 回顧上次練習中 `event.path` 與 `component.button-b6562c963d` 和 `event.component['dc:title']` 就像《觀景之旅》一樣。

### 元件ID

1. 導覽至Experience Platform，並導覽至與AEM網站整合的標籤屬性。
1. 導覽至 **資料元素** 區段，按一下 **新增資料元素**.
1. 針對 **名稱** 欄位，輸入 **元件ID**.
1. 針對 **資料元素類型** 欄位，選擇 **自訂程式碼**.

   ![元件ID資料元素表單](assets/track-clicked-component/component-id-data-element.png)

1. 按一下 **開啟編輯器** 按鈕，並在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. 儲存變更。

   >[!NOTE]
   >
   > 回想一下 `event` 物件可供使用，且範圍取決於觸發 **規則** 在標籤屬性中。 在資料元素為 *引用* 規則內。 因此，在規則內使用此資料元素是安全的，如 **已載入頁面** 在上一步中建立的規則 *但* 在其他情況下使用將不安全。


### 元件標題

1. 導覽至 **資料元素** 區段，按一下 **新增資料元素**.
1. 針對 **名稱** 欄位，輸入 **元件標題**.
1. 針對 **資料元素類型** 欄位，選擇 **自訂程式碼**.
1. 按一下 **開啟編輯器** 按鈕，並在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 儲存變更。

## 新增條件至CTA已點按規則

接下來，更新 **已點按CTA** 規則，確保規則只在 `cmp:click` 針對 **Teaser** 或 **按鈕**. 由於預告的CTA在資料層中被視為個別物件，因此務必檢查父項，以確認其是否來自預告。

1. 在標籤屬性UI中，導覽至 **已點按CTA** 規則。
1. 在 **條件** 按一下 **新增** 開啟 **條件設定** 嚮導。
1. 針對 **條件類型** 欄位，選擇 **自訂程式碼**.

   ![CTA點按條件自訂程式碼](assets/track-clicked-component/custom-code-condition.png)

1. 按一下 **開啟編輯器** 並在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   上述程式碼會先檢查資源類型是否來自 **按鈕** 或資源類型來自 **Teaser**.

1. 儲存變更。

## 設定Analytics變數並觸發追蹤連結信標

目前 **已點按CTA** 規則只會輸出console陳述式。 接下來，使用資料元素和Analytics擴充功能，將Analytics變數設為 **動作**. 我們也設定額外的動作來觸發 **追蹤連結** 並將收集的資料傳送至Adobe Analytics。

1. 在 **已點按CTA** 規則， **移除** the **核心 — 自訂程式碼** 動作（控制台陳述式）:

   ![移除自訂程式碼動作](assets/track-clicked-component/remove-console-statements.png)

1. 在「動作」底下，按一下 **新增** 來建立動作。
1. 設定 **擴充功能** 類型 **Adobe Analytics** 並設定 **動作類型** to  **設定變數**.

1. 為 **eVar**, **Prop**，和 **事件**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![設定eVarProp和事件](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 此處 `%Component ID%` 會使用，因為它保證所點按CTA的唯一識別碼。 使用的潛在缺點 `%Component ID%` 即Analytics報表包含 `button-2e6d32893a`. 使用 `%Component Title%` 會提供更人性化的名稱，但值可能並非唯一。

1. 接下來，在 **Adobe Analytics — 設定變數** 點選 **plus** 圖示：

   ![新增額外動作至標籤規則](assets/track-clicked-component/add-additional-launch-action.png)

1. 設定 **擴充功能** 類型 **Adobe Analytics** 並設定 **動作類型** to  **傳送信標**.
1. 在 **追蹤** 將單選按鈕設定為 **`s.tl()`**.
1. 針對 **連結類型** 欄位，選擇 **自訂連結** 和 **連結名稱** 將值設為： **`%Component Title%: CTA Clicked`**:

   ![傳送連結信標的設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   上述設定會結合資料元素中的動態變數 **元件標題** 和靜態字串 **已點按CTA**.

1. 儲存變更。此 **已點按CTA** 規則現在應具備下列設定：

   ![最終標籤規則設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 聽 `cmp:click` 事件。
   * **2.** 檢查事件是否由 **按鈕** 或 **Teaser**.
   * **3.** 設定Analytics變數以追蹤 **元件ID** as a **eVar**, **prop**，和 **事件**.
   * **4.** 傳送Analytics追蹤連結信標(並傳送 **not** 視為頁面檢視)。

1. 儲存所有變更並建置您的標籤程式庫，並提升至適當的環境。

## 驗證追蹤連結信標和Analytics呼叫

現在， **已點按CTA** 規則會傳送Analytics信標，則您應該可以使用Analytics Debugger查看Analytics追蹤變數。

1. 開啟 [WKND站點](https://wknd.site/us/en.html) 在瀏覽器中。
1. 按一下Debugger圖示 ![Experience Platform Debugger圖示](assets/track-clicked-component/experience-cloud-debugger.png) 以開啟Experience Platform偵錯器。
1. 確認Debugger將標籤屬性對應至 *您的* 開發環境，如先前所述及 **主控台記錄** 已勾選。
1. 開啟Analytics功能表，並確認報表套裝已設為 *您的* 報表套裝。

   ![Analytics標籤除錯程式](assets/track-clicked-component/analytics-tab-debugger.png)

1. 在瀏覽器中，按一下 **Teaser** 或 **按鈕** CTA按鈕，可導覽至其他頁面。

   ![要點按的CTA按鈕](assets/track-clicked-component/cta-button-to-click.png)

1. 返回Experience Platform偵錯器，向下捲動並展開 **網路請求** > *您的報表套裝*. 您應該能夠找到 **eVar**, **prop**，和 **事件** 設定。

   ![點按時追蹤的Analytics事件、eVar和Prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 返回瀏覽器，開啟開發人員主控台。 導覽至網站頁尾，然後按一下其中一個導覽連結：

   ![按一下頁尾中的導覽連結](assets/track-clicked-component/click-navigation-link-footer.png)

1. 在瀏覽器主控台中觀察訊息 *未符合規則「已點按CTA」的「自訂程式碼」*.

   上述訊息是因為導覽元件會觸發 `cmp:click` 事件 *但* 因為 [規則的條件](#add-a-condition-to-the-cta-clicked-rule) 會檢查資源類型，不會執行任何動作。

   >[!NOTE]
   >
   > 如果您沒有看見任何主控台記錄，請確定 **主控台記錄** 在下方勾選 **Experience Platform標籤** 在Experience Platform偵錯器中。

## 恭喜！

您剛才在Experience Platform中使用事件導向的Adobe用戶端資料層和標籤，來追蹤AEM網站上特定元件的點按次數。
