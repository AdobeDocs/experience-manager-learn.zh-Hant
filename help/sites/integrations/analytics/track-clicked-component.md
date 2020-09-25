---
title: 使用Adobe Analytics追蹤點按的元件
description: 使用事件導向的Adobe客戶資料層來追蹤Adobe Experience Manager網站上特定元件的點按次數。 瞭解如何使用Experience Platform Launch中的規則來監聽這些事件，並將資料傳送至具有追蹤連結信標的Adobe Analytics。
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
translation-type: tm+mt
source-git-commit: 97fe98c8c62f5472f7771bbc803b2a47dc97044d
workflow-type: tm+mt
source-wordcount: '1773'
ht-degree: 1%

---


# 使用Adobe Analytics追蹤點按的元件

搭配AEM核心元件使用事件導向 [的Adobe客戶資料層](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html) ，以追蹤Adobe Experience Manager網站上特定元件的點按次數。 瞭解如何使用Experience Platform Launch中的規則來監聽點按事件、依元件篩選資料，以及將資料傳送至具有追蹤連結信標的Adobe Analytics。

## 您將建立的

WKND行銷團隊想瞭解哪些「行動呼籲(CTA)」按鈕在首頁上表現最佳。 在本教學課程中，我們將在Experience Platform Launch中新增規則，監聽來自 `cmp:click`**Teaser** 和 **Button** 元件的事件，並將元件ID和新事件與追蹤連結信標一起傳送至Adobe Analytics。

![您將建立的追蹤點按次數](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目標 {#objective}

1. 根據事件在啟動中建立事件導向 `cmp:click` 規則。
1. 依元件資源類型篩選不同事件。
1. 設定已點按的元件ID，並傳送具有追蹤連結信標的事件Adobe Analytics。

## 必備條件

本教學課程是「使用Adobe Analytics [收集頁面資料」的延續](./collect-data-analytics.md) ，並假設您有：

* 啟 **用Adobe Analytics擴充功能** 的 [Launch屬性](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)
* **Adobe Analytics** test/dev報表套裝ID和追蹤伺服器。 請參閱下列檔案以 [建立新報表套裝](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。
* [Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) browser extension configured with your Launch property loaded on [https://wknd.site/us/en.html](https://wknd.site/us/en.html) or an AEM site with the Adobe Data Layer.

## 檢查按鈕和摘要結構

在啟動中建立規則之前，請先檢視Button和Teaser的 [架構](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item) ，並在資料層實作中加以檢查。

1. 導覽至 [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 開啟瀏覽器的開發人員工具並導覽至 **Console**。 運行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   這會傳回Adobe用戶端資料層的目前狀態。

   ![透過瀏覽器主控台的資料層狀態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 展開回應並尋找前置詞和項目 `button-` 的 `teaser-xyz-cta` 項目。 您應看到如下的資料結構：

   按鈕結構：

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   摘要架構：

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "nt:unstructured"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   這些是以元件／容 [器項目結構為基礎](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)。 我們將在Launch中建立的規則將使用此架構。

## 建立CTA點按規則

Adobe用戶端資料層是事件 **導向** 的資料層。 當任何核心元件被點按時， `cmp:click` 會透過資料層傳送事件。 接著建立規則以監聽該事 `cmp:click` 件。

1. 導覽至Experience Platform Launch，並進入與AEM網站整合的Web屬性。
1. 導覽至「啟 **動** 」UI中的「規則」區段，然後按一下「新 **增規則」**。
1. 為「已點按的 **規則CTA」命名**。
1. 按一 **下「事件** >新增 **」以開啟「****** 事件設定」精靈。
1. 在「事 **件類型** 」下 **選擇「自訂代碼」**。

   ![將規則命名為「已點按的CTA」並新增自訂代碼事件](assets/track-clicked-component/custom-code-event.png)

1. 按一 **下主面板中的** 「開啟編輯器」，然後輸入下列程式碼片段：

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   上述程式碼片段會將函式推送至資 [料層，以新增事件](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 偵聽器。 觸發事 `cmp:click` 件時，會呼 `componentClickedHandler` 叫函式。 在此函式中，會新增一些例行性檢查，並 `event` 且會針對觸發事件的元件，以資 [料層的最新狀態來建立新物件](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 。

   之後就 `trigger(event)` 叫了。 `trigger()` 是Launch中的保留名稱，將「觸發」啟動規則。 我們將物 `event` 件傳遞為參數，而此參數會以Launch中另一個名稱的保留名稱公開 `event`。 Launch中的資料元素現在可以參照各種屬性，例如： `event.component['someKey']`.

1. 儲存變更。
1. 下一步，在「 **動作** 」(Actions **)下，按一** 下「新增」(Add **)以開啟「動** 作設定」精靈。
1. 在「操 **作類型** 」下 **選擇「自定義代碼」**。

   ![自訂代碼動作類型](assets/track-clicked-component/action-custom-code.png)

1. 按一 **下主面板中的** 「開啟編輯器」，然後輸入下列程式碼片段：

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   物件 `event` 會從自訂事件中 `trigger()` 呼叫的方法傳遞。 `component` 是從觸發點按的資料層衍生的元件 `getState` 目前狀態。

1. 儲存變更並在Launch中執 [行建置](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) ，將程式碼提升至 [AEM網站上使用的環](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) 境。

   >[!NOTE]
   >
   > 使用 [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) ，將內嵌程式碼切換至開發環 **境非常有用** 。

1. 導覽至 [WKND網站](https://wknd.site/us/en.html) ，並開啟開發人員工具以檢視主控台。 選擇 **保留日誌**。

1. 按一下「摘要 **」** 或「 **按鈕** CTA」按鈕之一，以導覽至另一頁。

   ![CTA按鈕，以按一下](assets/track-clicked-component/cta-button-to-click.png)

1. 在開發人員主控台中， **觀察CTA已點按規則** :

   ![已點按CTA按鈕](assets/track-clicked-component/cta-button-clicked-log.png)

## 建立資料元素

接著建立資料元素，以擷取已點按的元件ID和標題。 回想一下，在上一次的演習中，產 `event.path` 品的輸出與 `component.button-b6562c963d` 之類似， `event.component['dc:title']` 其價值與「檢視行程」類似。

### 元件ID

1. 導覽至Experience Platform Launch，並進入與AEM網站整合的Web屬性。
1. 導覽至「資 **料元素」區段** ，然後按一 **下「新增資料元素」**。
1. 在「名 **稱** 」中 **輸入元件ID**。
1. 對於「 **資料元素類型** 」，請 **選取「自訂代碼」**。

   ![元件ID資料元素表單](assets/track-clicked-component/component-id-data-element.png)

1. 按一 **下「開啟編輯器** 」，然後在自訂代碼編輯器中輸入下列項目：

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   儲存變更。

   >[!NOTE]
   >
   > 請記得， `event` 物件已設為可用且根據觸發Launch規則的事件 **進行** 「範圍」。 在規則內參考資料元素之前，資料元素的 *值* 才設定。 因此，在規則內使用此資料元素是安全的，就像在上一練習中建立的 **CTA Clicked** Rule ** ，但在其他上下文中使用則不安全。

### 元件標題

1. 導覽至「資 **料元素」區段** ，然後按一 **下「新增資料元素」**。
1. 在「名 **稱** 」中 **輸入元件標題**。
1. 對於「 **資料元素類型** 」，請 **選取「自訂代碼」**。
1. 按一 **下「開啟編輯器** 」，然後在自訂代碼編輯器中輸入下列項目：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   儲存變更。

## 新增條件至CTA點按規則

接著，更新 **CTA Clicked** (點選 `cmp:click` )規則，以確保只有在針對 **Teaser** 或Button觸發事件時才觸發規則 ****。 由於摘要的CTA在資料層中被視為個別物件，因此務必檢查父項以確認摘要是否來自摘要。

1. 在啟動UI中，導覽至先前建 **立的頁面** 載入規則。
1. 在「條 **件** 」(Conditions **)下，單** 擊「添加」(Add **)以開啟「條** 件配置」(Condition Configuration)嚮導。
1. 對於「 **條件類型** 」，選 **擇「自定義代碼」**。

   ![CTA點按條件自訂代碼](assets/track-clicked-component/custom-code-condition.png)

1. 按一 **下「開啟編輯器** 」，然後在自訂代碼編輯器中輸入下列項目：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       //Check for Button Type
       if(event.component['@type'] === 'wknd/components/button') {
           return true;
       } else if (event.component['@type'] == 'nt:unstructured') {
           // Check for CTA inside a Teaser
           var parentComponentId = event.component['parentId'];
           var parentComponent = window.adobeDataLayer.getState('component.' + parentComponentId);
   
           if(parentComponent['@type'] === 'wknd/components/teaser') {
               return true;
           }
       }
   }
   
   return false;
   ```

   上述程式碼會先檢查資源類型是否來自 **Button** ，然後檢查資源類型是否來自 **Teaser中的CTA**。

1. 儲存變更。

## 設定Analytics變數並觸發追蹤連結信標

目前， **** CTA點按規則只會輸出控制台陳述式。 接著，使用資料元素和Analytics擴充功能，將Analytics變數設為動 **作**。 我們也會設定其他動作來觸發 **追蹤連結** ，並將收集的資料傳送至Adobe Analytics。

1. 在「載入 **頁面** 」規 **則中，移** 除「核心——自訂程式碼 **** 」動作（控制台陳述式）:

   ![移除自訂程式碼動作](assets/track-clicked-component/remove-console-statements.png)

1. 在「動作」(Actions)下， **按一下** 「新增」(Add)以新增動作。
1. 將「延 **伸功能** 」類型設定為 **Adobe Analytics** ，並將「動作類型」 **設定為「設******&#x200B;定變數」。

1. 為eVar、 **Props**&#x200B;和Events設 **定下列**&#x200B;值 ****:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8` - `CTA Clicked`

   ![設定eVar Prop和事件](assets/track-clicked-component/set-evar-prop-event.png)

1. 接著，點選加號圖示，在 **Adobe Analytics —— 設定變數右側新增其** 他動作 **** :

   ![新增其他啟動動作](assets/track-clicked-component/add-additional-launch-action.png)

1. 將「延 **伸功能** 」類型設 **定為Adobe Analytics** ，並將「 **動作類型** 」設定為「傳送 ****&#x200B;信標」。
1. 在「 **追蹤** 」下方，將選項按鈕設為 **`s.tl()`**。
1. 對於「 **連結類型** 」，請選擇「 **自定義連結** 」，並為「連結名稱 **」將值設定為「資料要素元件******&#x200B;標題：

   ![傳送連結信標的設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

1. 儲存變更。「 **CTA點按規則** 」現在應具備下列設定：

   ![最終啟動設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 聽聽活 `cmp:click` 動。
   * **2.** 檢查事件是否由「按鈕」或「摘 **要** 」 **觸發**。
   * **3.** 將Analytics變數設定為以 **eVar** 、 **prop****、eventImp**&#x200B;追蹤元 ****&#x200B;件ID。
   * **4.** 傳送Analytics追蹤連結信標( **且** 不將其視為頁面檢視)。

1. 儲存所有變更並建立您的啟動程式庫，並升級至適當的環境。

## 驗證追蹤連結信標和分析呼叫

現在， **** CTA已點按規則會傳送Analytics信標，您應該可以使用Experience Platform除錯程式來查看Analytics追蹤變數。

1. 在您的 [瀏覽器中開啟](https://wknd.site/us/en.html) WKND網站。
1. 按一下「除錯程式」圖 ![示「Experience平台除錯程式」圖示](assets/track-clicked-component/experience-cloud-debugger.png) ，以開啟「Experience Platform Debugger」。
1. 請確定除錯程式正將Launch屬性對應至您的 *Development* environment，如先前所述，且已勾 **選「Console記錄** 」。
1. 開啟「Analytics」功能表，並確認報表套裝已設 *定至* 報表套裝。

   ![Analytics標籤除錯程式](assets/track-clicked-component/analytics-tab-debugger.png)

1. 在瀏覽器中，按一下「摘要 **」** 或「 **按鈕** CTA」按鈕，以導覽至另一頁。

   ![CTA按鈕，以按一下](assets/track-clicked-component/cta-button-to-click.png)

1. 返回Experience Platform除錯程式並向下捲動並展開「網 **路請求** > *您的報表套裝」*。 您應該可以找到 **eVar**、 **prop**&#x200B;和 **event** set。

   ![點按時追蹤的Analytics事件、evar和prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 返回瀏覽器並開啟開發人員主控台。 導覽至網站的頁尾，然後按一下其中一個導覽連結：

   ![按一下頁尾中的「導覽」連結](assets/track-clicked-component/click-navigation-link-footer.png)

1. 在瀏覽器主控台中，未 *符合規則「已點按CTA」的「自訂代碼」訊息*。

   這是因為導覽元件會觸發 `cmp:click` 事 *件* ，但由於我們針對資源類型檢查，因此不會執行任何動作。

   >[!NOTE]
   >
   > 如果您未看到任何主控台記錄檔，請確 **定已勾選「Experience Platform Debugger」中的「** Launch **** 」（啟動）下的「Console Logging」（控制台記錄）。

## 恭喜！

您剛才使用事件導向的Adobe Client Data Layer和Experience Platform Launch來追蹤Adobe Experience Manager網站上特定元件的點按次數。