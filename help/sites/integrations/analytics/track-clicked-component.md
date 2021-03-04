---
title: 使用Adobe Analytics追蹤點按的元件
description: 使用事件導向的Adobe用戶端資料層來追蹤Adobe Experience Manager網站上特定元件的點按次數。 瞭解如何使用Experience Platform Launch中的規則來監聽這些事件，並將資料傳送至具有追蹤連結信標的Adobe Analytics。
feature: 分析
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
topic: Integrations
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 1%

---


# 使用Adobe Analytics追蹤點按的元件

使用事件驅動[Adobe客戶端資料層與核心元件&lt;a1/AEM>來跟蹤Adobe Experience Manager站點上特定元件的按一下。 ](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html)瞭解如何使用Experience Platform Launch中的規則來監聽點按事件、依元件篩選，以及使用追蹤連結信標將資料傳送至Adobe Analytics。

## 您將建立的

WKND行銷團隊想瞭解哪些「行動呼籲(CTA)」按鈕在首頁上表現最佳。 在本教學課程中，我們將在Experience Platform Launch中新增一個規則，監聽來自&#x200B;**Teaser**&#x200B;和&#x200B;**Button**&#x200B;元件的`cmp:click`事件，並將元件ID和新事件與追蹤連結信標一起傳送至Adobe Analytics。

![您將建立的追蹤點按次數](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目標{#objective}

1. 根據`cmp:click`事件在啟動中建立事件導向規則。
1. 依元件資源類型篩選不同事件。
1. 設定已點按的元件ID，並使用追蹤連結信標傳送事件Adobe Analytics。

## 必備條件

本教學課程是[使用Adobe Analytics](./collect-data-analytics.md)收集頁面資料的延續課程，並假設您有：

* 啟用[Adobe Analytics擴展](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)的&#x200B;**啟動屬性**
* **Adobe** Analyticst/dev報表套裝ID和追蹤伺服器。請參閱以下檔案，瞭解如何建立新的報表套裝](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。[
* [Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 除錯瀏覽器擴充功能已設定，您的Launch屬性已載入 [https://wknd.site/us/en.](https://wknd.site/us/en.html) html或啟AEM用Adobe資料層的網站。

## Inspect按鈕與摘要架構

在啟動中建立規則之前，請先檢閱按鈕和摘要](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)的[架構，然後在資料層實作中加以檢查。

1. 導覽至[https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 開啟瀏覽器的開發人員工具，並導覽至&#x200B;**Console**。 運行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   這會傳回Adobe用戶端資料層的目前狀態。

   ![透過瀏覽器主控台的資料層狀態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 展開響應並查找前置詞為`button-`和`teaser-xyz-cta`條目的條目。 您應看到如下的資料結構：

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
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   這些是以[元件／容器項目結構](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)為基礎。 我們將在Launch中建立的規則將使用此架構。

## 建立CTA點按規則

Adobe客戶端資料層是&#x200B;**事件**&#x200B;驅動的資料層。 當任何核心元件被點按時，`cmp:click`事件會透過資料層傳送。 接著建立規則以監聽`cmp:click`事件。

1. 導覽至Experience Platform Launch並進入與網站整合的WebAEM屬性。
1. 導覽至「啟動UI」中的「**規則**」區段，然後按一下「新增規則&#x200B;**」。**
1. 將規則命名為&#x200B;**CTA Clicked**。
1. 按一下&#x200B;**事件** > **添加**&#x200B;以開啟&#x200B;**事件配置**&#x200B;嚮導。
1. 在&#x200B;**事件類型**&#x200B;下，選擇&#x200B;**自訂代碼**。

   ![將規則命名為「已點按的CTA」並新增自訂代碼事件](assets/track-clicked-component/custom-code-event.png)

1. 按一下主面板中的&#x200B;**開啟編輯器**，然後輸入下列程式碼片段：

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

   上述程式碼片段將透過[將函式](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)推送至資料層來新增事件偵聽器。 觸發`cmp:click`事件時，會呼叫`componentClickedHandler`函式。 在此函式中，添加一些例行性檢查，並使用觸發事件的元件的資料層[的最新狀態構造新的`event`對象。](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)

   在呼叫`trigger(event)`之後。 `trigger()` 是Launch中的保留名稱，將「觸發」啟動規則。我們將`event`物件傳遞為參數，而此參數將會在名為`event`的Launch中以另一個保留名稱公開。 Launch中的資料元素現在可以參照各種屬性，例如：`event.component['someKey']`。

1. 儲存變更。
1. 下一步，在&#x200B;**Actions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Action Configuration**&#x200B;嚮導。
1. 在「**動作類型**」下，選擇「自訂代碼&#x200B;**」。**

   ![自訂代碼動作類型](assets/track-clicked-component/action-custom-code.png)

1. 按一下主面板中的&#x200B;**開啟編輯器**，然後輸入下列程式碼片段：

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event`物件是從自訂事件中呼叫的`trigger()`方法傳遞。 `component` 是從觸發點按的資料層衍生的元件 `getState` 目前狀態。

1. 儲存變更並在Launch中執行[build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)，將程式碼提升至您網站上使用的[環境&lt;a3/AEM>。](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)

   >[!NOTE]
   >
   > 使用[Adobe Experience Platform調試器](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)將嵌入代碼切換到&#x200B;**Development**&#x200B;環境非常有用。

1. 導覽至[WKND網站](https://wknd.site/us/en.html)，並開啟開發人員工具以檢視主控台。 選擇&#x200B;**保留日誌**。

1. 按一下&#x200B;**Teaser**&#x200B;或&#x200B;**Button** CTA按鈕之一，以導覽至另一頁。

   ![CTA按鈕，以按一下](assets/track-clicked-component/cta-button-to-click.png)

1. 在開發人員主控台中，觀察&#x200B;**CTA Clicked**&#x200B;規則已引發：

   ![已點按CTA按鈕](assets/track-clicked-component/cta-button-clicked-log.png)

## 建立資料元素

接著建立資料元素，以擷取已點按的元件ID和標題。 回想一下，在上一次練習中，`event.path`的輸出與`component.button-b6562c963d`類似，而`event.component['dc:title']`的值與「檢視行程」類似。

### 元件ID

1. 導覽至Experience Platform Launch並進入與網站整合的WebAEM屬性。
1. 導覽至&#x200B;**資料元素**&#x200B;區段，然後按一下&#x200B;**新增資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**元件ID**。
1. 對於&#x200B;**資料元素類型**，選擇&#x200B;**自定義代碼**。

   ![元件ID資料元素表單](assets/track-clicked-component/component-id-data-element.png)

1. 按一下&#x200B;**開啟編輯器**，然後在自定義代碼編輯器中輸入以下內容：

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   儲存變更。

   >[!NOTE]
   >
   > 請記住，`event`物件已設為可用，且範圍以啟動中觸發&#x200B;**Rule**&#x200B;的事件為基礎。 在規則中「資料元素」是&#x200B;*referenced*&#x200B;之前，不會設定「資料元素」的值。 因此，在規則內使用此「資料元素」是安全的，例如在上一練習&#x200B;*中建立的&#x200B;**CTA Clicked**規則，但*&#x200B;在其他上下文中使用則不安全。

### 元件標題

1. 導覽至&#x200B;**資料元素**&#x200B;區段，然後按一下&#x200B;**新增資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**元件標題**。
1. 對於&#x200B;**資料元素類型**，選擇&#x200B;**自定義代碼**。
1. 按一下&#x200B;**開啟編輯器**，然後在自定義代碼編輯器中輸入以下內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   儲存變更。

## 新增條件至CTA點按規則

接著，更新&#x200B;**CTA Clicked**&#x200B;規則，以確保只有在&#x200B;**Teaser**&#x200B;或&#x200B;**Button**&#x200B;引發`cmp:click`事件時才觸發規則。 由於摘要的CTA在資料層中被視為個別物件，因此務必檢查父項以確認摘要是否來自摘要。

1. 在啟動UI中，導覽至先前建立的&#x200B;**CTA Clicked**&#x200B;規則。
1. 在&#x200B;**Conditions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Condition Configuration**&#x200B;嚮導。
1. 對於&#x200B;**條件類型**，選擇&#x200B;**自定義代碼**。

   ![CTA點按條件自訂代碼](assets/track-clicked-component/custom-code-condition.png)

1. 按一下&#x200B;**開啟編輯器**，然後在自定義代碼編輯器中輸入以下內容：

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

   上述程式碼會先檢查資源類型是否來自&#x200B;**Button**，然後檢查資源類型是否來自&#x200B;**Teaser**&#x200B;內的CTA。

1. 儲存變更。

## 設定Analytics變數並觸發追蹤連結信標

目前，**CTA Clicked**&#x200B;規則只會輸出主控台陳述式。 接著，使用資料元素和Analytics擴充功能，將Analytics變數設為&#x200B;**action**。 我們也會設定其他動作來觸發&#x200B;**追蹤連結**，並將收集的資料傳送至Adobe Analytics。

1. 在&#x200B;**CTA Clicked**&#x200B;規則&#x200B;**remove**&#x200B;中&#x200B;**核心——自訂代碼**&#x200B;動作（控制台語句）:

   ![移除自訂程式碼動作](assets/track-clicked-component/remove-console-statements.png)

1. 在「操作」(Actions)下，按一下「添加」(Add)**以添加新操作。**
1. 將&#x200B;**擴展**&#x200B;類型設定為&#x200B;**Adobe Analytics**，並將&#x200B;**操作類型**&#x200B;設定為&#x200B;**設定變數**。

1. 為&#x200B;**eVars**、**Props**&#x200B;和&#x200B;**Events**&#x200B;設定下列值：

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![設定eVarProp和事件](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 此處使用`%Component ID%`，因為它會為所點按的CTA取得唯一識別碼。 使用`%Component ID%`的潛在缺點是，Analytics報表將包含`button-2e6d32893a`等值。 使用`%Component Title%`會提供更人性化的名稱，但值可能不是唯一的。

1. 接著，點選&#x200B;**plus**&#x200B;圖示，在&#x200B;**Adobe Analytics-設定變數**&#x200B;右側新增其他動作：

   ![新增其他啟動動作](assets/track-clicked-component/add-additional-launch-action.png)

1. 將&#x200B;**擴展**&#x200B;類型設定為&#x200B;**Adobe Analytics**，並將&#x200B;**操作類型**&#x200B;設定為&#x200B;**發送信標**。
1. 在&#x200B;**Tracking**&#x200B;下，將選項按鈕設為&#x200B;**`s.tl()`**。
1. 對於&#x200B;**連結類型**，選擇&#x200B;**自定義連結**，對於&#x200B;**連結名稱**，將值設定為：**`%Component Title%: CTA Clicked`**:

   ![傳送連結信標的設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   這會結合資料元素&#x200B;**元件標題**&#x200B;中的動態變數，以及靜態字串&#x200B;**CTA Clicked**。

1. 儲存變更。**CTA Clicked**&#x200B;規則現在應具備下列設定：

   ![最終啟動設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 聽聽活 `cmp:click` 動。
   * **2.** 檢查事件是否由「按鈕」或「摘 **** 要」 **觸發**。
   * **3.** 將Analytics變數設定為，以追蹤 **元** 件ID **為** **eVar**、 **prop**，以及事件Adobe。
   * **4.** 傳送Analytics追蹤連結信標(且不 **** 要將其視為頁面檢視)。

1. 儲存所有變更並建立您的啟動程式庫，並升級至適當的環境。

## 驗證追蹤連結信標和分析呼叫

現在，**CTA Clicked**&#x200B;規則會傳送Analytics信標，您應該可以使用Experience Platform偵錯器來查看Analytics追蹤變數。

1. 在瀏覽器中開啟[WKND站點](https://wknd.site/us/en.html)。
1. 按一下「除錯程式」圖示![「體驗平台除錯程式」圖示](assets/track-clicked-component/experience-cloud-debugger.png)以開啟「Experience Platform除錯程式」。
1. 請確定除錯程式正將Launch屬性對應至&#x200B;*您的*&#x200B;開發環境，如先前所述，且已勾選&#x200B;**控制台記錄**。
1. 開啟「Analytics」功能表，並確認報表套裝已設為&#x200B;*您的*&#x200B;報表套裝。

   ![Analytics標籤除錯程式](assets/track-clicked-component/analytics-tab-debugger.png)

1. 在瀏覽器中，按一下&#x200B;**Teaser**&#x200B;或&#x200B;**Button** CTA按鈕之一，以導覽至另一頁。

   ![CTA按鈕，以按一下](assets/track-clicked-component/cta-button-to-click.png)

1. 返回Experience Platform調試器，並向下滾動並展開&#x200B;**網路請求** > *您的報表套裝*。 您應該能夠找到&#x200B;**eVar**、**prop**&#x200B;和&#x200B;**事件**&#x200B;集。

   ![點按時追蹤的Analytics事件、evar和prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 返回瀏覽器並開啟開發人員主控台。 導覽至網站的頁尾，然後按一下其中一個導覽連結：

   ![按一下頁尾中的「導覽」連結](assets/track-clicked-component/click-navigation-link-footer.png)

1. 在瀏覽器主控台中，未符合規則&quot;CTA Clicked&quot;的訊息&#x200B;*&quot;Custom Code&quot;（自訂代碼）*。

   這是因為導覽元件確實會觸發`cmp:click`事件&#x200B;*但*，因為我們針對資源類型檢查了該事件，所以不會執行任何動作。

   >[!NOTE]
   >
   > 如果您未看到任何控制台日誌，請確定在Experience Platform調試器中的&#x200B;**Launch**&#x200B;下選中了&#x200B;**控制台日誌**。

## 恭喜！

您剛才使用事件導向的Adobe用戶端資料層和Experience Platform Launch來追蹤Adobe Experience Manager網站上特定元件的點按次數。