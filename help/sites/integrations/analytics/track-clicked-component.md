---
title: 使用Adobe Analytics追蹤已點按的元件
description: 使用事件導向的Adobe用戶端資料層來追蹤Adobe Experience Manager網站上特定元件的點按次數。 了解如何使用Experience Platform Launch中的規則來監聽這些事件，並透過追蹤連結信標將資料傳送至Adobe Analytics。
version: cloud-service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1810'
ht-degree: 1%

---


# 使用Adobe Analytics追蹤已點按的元件

使用事件導向的[Adobe用戶端資料層搭配AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)來追蹤Adobe Experience Manager網站上特定元件的點按次數。 了解如何使用Experience Platform Launch中的規則來監聽點按事件、依元件篩選資料，以及透過追蹤連結信標將資料傳送至Adobe Analytics。

## 您將建置的

WKND行銷團隊想要了解哪個動作呼叫(CTA)按鈕在首頁上表現最佳。 在本教學課程中，我們將在Experience Platform Launch中新增規則，監聽來自&#x200B;**Teaser**&#x200B;和&#x200B;**Button**&#x200B;元件的`cmp:click`事件，並將元件ID和新事件連同追蹤連結信標一併傳送至Adobe Analytics。

![建立追蹤點按次數的項目](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目標 {#objective}

1. 根據`cmp:click`事件在Launch中建立事件導向規則。
1. 依元件資源類型篩選不同的事件。
1. 設定已點按的元件id，並傳送具有追蹤連結信標的事件Adobe Analytics。

## 必備條件

本教學課程是[使用Adobe Analytics](./collect-data-analytics.md)收集頁面資料的延續課程，並假設您擁有：

* 啟用[Adobe Analytics擴充功能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html)的&#x200B;**Launch屬性**
* **Adobe** Analyticstest/dev報表套裝ID和追蹤伺服器。請參閱下列檔案，了解如何建立新的報表套裝](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。[
* [Experience Platform](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Debuggerbrowser擴充功能已設定您的Launch屬性，載 [入https://wknd.site/us/en.](https://wknd.site/us/en.html) html或AEM網站，並啟用Adobe資料層。

## Inspect按鈕和預告結構

在Launch中建立規則之前，請先檢閱按鈕和Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item)的[架構，並在資料層實作中檢查它們。

1. 導覽至[https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 開啟瀏覽器的開發人員工具，並導覽至&#x200B;**主控台**。 執行下列命令：

   ```js
   adobeDataLayer.getState();
   ```

   這會傳回Adobe用戶端資料層的目前狀態。

   ![透過瀏覽器主控台的資料層狀態](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 展開回應，並尋找以`button-`和`teaser-xyz-cta`項目為首碼的項目。 您應會看到下列資料結構：

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

   這些項目以[元件/容器項目架構](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item)為基礎。 我們將在Launch中建立的規則將使用此結構。

## 建立CTA點按規則

Adobe客戶端資料層是&#x200B;**event**&#x200B;驅動的資料層。 按一下任何核心元件時， `cmp:click`事件會透過資料層發送。 接下來，建立規則以監聽`cmp:click`事件。

1. 導覽至Experience Platform Launch，並導覽至與AEM網站整合的Web屬性。
1. 導覽至Launch UI中的&#x200B;**Rules**&#x200B;區段，然後按一下&#x200B;**Add Rule**。
1. 將規則命名為&#x200B;**CTA Clicked**。
1. 按一下「**事件** > **新增**」以開啟「**事件配置**」精靈。
1. 在&#x200B;**事件類型**&#x200B;下，選擇&#x200B;**自訂代碼**。

   ![將規則命名為「CTA已點按」並新增自訂程式碼事件](assets/track-clicked-component/custom-code-event.png)

1. 按一下主面板中的&#x200B;**開啟編輯器** ，然後輸入下列程式碼片段：

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

   上述程式碼片段會透過[推送函式](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)至資料層來新增事件監聽器。 觸發`cmp:click`事件時，會呼叫`componentClickedHandler`函式。 在此函式中，會新增一些健全性檢查，並針對觸發事件的元件，使用資料層](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)的最新[狀態來建構新的`event`物件。

   之後呼叫`trigger(event)`。 `trigger()` 是Launch中的保留名稱，且將「觸發」Launch規則。我們會以參數形式傳遞`event`物件，而此參數將會由Launch中名為`event`的其他保留名稱公開。 Launch中的資料元素現在可以參照各種屬性，如下所示：`event.component['someKey']`。

1. 儲存變更。
1. 下一步在&#x200B;**Actions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Action Configuration**&#x200B;精靈。
1. 在「**動作類型**」下，選擇「**自訂程式碼**」。

   ![自訂程式碼動作類型](assets/track-clicked-component/action-custom-code.png)

1. 按一下主面板中的&#x200B;**開啟編輯器** ，然後輸入下列程式碼片段：

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event`物件是從自訂事件中呼叫的`trigger()`方法傳遞。 `component` 是從觸發點按的資料層衍生之元件 `getState` 的目前狀態。

1. 儲存變更並在Launch中執行[build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html)，將程式碼推廣至AEM網站上使用的[environment](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html)。

   >[!NOTE]
   >
   > 使用[Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html)將內嵌程式碼切換至&#x200B;**開發**&#x200B;環境，會相當實用。

1. 導覽至[WKND Site](https://wknd.site/us/en.html)，並開啟開發人員工具以檢視主控台。 選擇&#x200B;**保留日誌**。

1. 按一下&#x200B;**Teaser**&#x200B;或&#x200B;**Button** CTA按鈕之一以導覽至其他頁面。

   ![要點按的CTA按鈕](assets/track-clicked-component/cta-button-to-click.png)

1. 在開發人員主控台中，觀察已引發&#x200B;**CTA Clicked**&#x200B;規則：

   ![已點按CTA按鈕](assets/track-clicked-component/cta-button-clicked-log.png)

## 建立資料元素

接著建立資料元素，以擷取已點按的元件ID和標題。 回想一下，在上一次練習中，`event.path`的輸出與`component.button-b6562c963d`類似，而`event.component['dc:title']`的值與「檢視行程」類似。

### 元件ID

1. 導覽至Experience Platform Launch，並導覽至與AEM網站整合的Web屬性。
1. 導覽至&#x200B;**資料元素**&#x200B;區段，然後按一下&#x200B;**新增資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**元件ID**。
1. 對於&#x200B;**資料元素類型**，請選擇&#x200B;**自訂代碼**。

   ![元件ID資料元素表單](assets/track-clicked-component/component-id-data-element.png)

1. 按一下「**開啟編輯器**」，然後在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   儲存變更。

   >[!NOTE]
   >
   > 回想一下，`event`物件已可供使用，且範圍是根據Launch中觸發&#x200B;**Rule**&#x200B;的事件而定。 在規則內的資料元素為&#x200B;*referenced*&#x200B;之前，資料元素的值不會設定。 因此，在先前練習&#x200B;*中建立的&#x200B;**CTA Clicked**規則內使用此資料元素是安全的，但在其他內容中使用*&#x200B;則不安全。

### 元件標題

1. 導覽至&#x200B;**資料元素**&#x200B;區段，然後按一下&#x200B;**新增資料元素**。
1. 對於&#x200B;**名稱**，輸入&#x200B;**元件標題**。
1. 對於&#x200B;**資料元素類型**，請選擇&#x200B;**自訂代碼**。
1. 按一下「**開啟編輯器**」，然後在自訂程式碼編輯器中輸入下列內容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   儲存變更。

## 新增條件至CTA已點按規則

接下來，更新&#x200B;**CTA Clicked**&#x200B;規則，以確保只有在為&#x200B;**Teaser**&#x200B;或&#x200B;**Button**&#x200B;觸發`cmp:click`事件時，才會觸發規則。 由於預告的CTA在資料層中被視為個別物件，因此請務必檢查父項，確認其是否來自預告。

1. 在Launch UI中，導覽至先前建立的&#x200B;**CTA已點按**&#x200B;規則。
1. 在&#x200B;**Conditions**&#x200B;下，按一下&#x200B;**Add**&#x200B;以開啟&#x200B;**Condition Configuration**&#x200B;精靈。
1. 對於&#x200B;**條件類型**，選擇&#x200B;**自訂代碼**。

   ![CTA點按條件自訂程式碼](assets/track-clicked-component/custom-code-condition.png)

1. 按一下「**開啟編輯器**」，然後在自訂程式碼編輯器中輸入下列內容：

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

   上述程式碼會先檢查資源類型是否來自&#x200B;**Button**，接著檢查資源類型是否來自&#x200B;**Teaser**&#x200B;內的CTA。

1. 儲存變更。

## 設定Analytics變數並觸發追蹤連結信標

目前，**CTA Clicked**&#x200B;規則只會輸出主控台陳述式。 接下來，使用資料元素和Analytics擴充功能，將Analytics變數設為&#x200B;**action**。 我們也會設定其他動作，以觸發&#x200B;**追蹤連結**，並將收集的資料傳送至Adobe Analytics。

1. 在&#x200B;**CTA Clicked**&#x200B;規則&#x200B;**remove**&#x200B;中， **Core - Custom Code**&#x200B;動作（主控台陳述式）:

   ![移除自訂程式碼動作](assets/track-clicked-component/remove-console-statements.png)

1. 在「動作」底下，按一下&#x200B;**Add**&#x200B;以新增動作。
1. 將&#x200B;**Extension**&#x200B;類型設定為&#x200B;**Adobe Analytics**，並將&#x200B;**Action Type**&#x200B;設定為&#x200B;**Set Variables**。

1. 為&#x200B;**eVars**、**Props**&#x200B;和&#x200B;**Events**&#x200B;設定下列值：

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![設定eVarProp和事件](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 此處使用`%Component ID%`，因為它會取得所點按CTA的唯一識別碼。 使用`%Component ID%`的潛在缺點是Analytics報表將包含`button-2e6d32893a`之類的值。 使用`%Component Title%`可提供更人性化的名稱，但值可能不是唯一的。

1. 接下來，點選&#x200B;**加號**&#x200B;圖示，在&#x200B;**Adobe Analytics — 設定變數**&#x200B;的右側新增其他動作：

   ![新增其他Launch動作](assets/track-clicked-component/add-additional-launch-action.png)

1. 將&#x200B;**Extension**&#x200B;類型設為&#x200B;**Adobe Analytics**，並將&#x200B;**Action Type**&#x200B;設為&#x200B;**Send Beacon**。
1. 在&#x200B;**Tracking**&#x200B;下，將選項按鈕設為&#x200B;**`s.tl()`**。
1. 對於&#x200B;**連結類型**，選擇&#x200B;**自訂連結**，對於&#x200B;**連結名稱**，將值設定為：**`%Component Title%: CTA Clicked`**:

   ![傳送連結信標的設定](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   這會結合資料元素&#x200B;**元件標題**&#x200B;中的動態變數，以及靜態字串&#x200B;**CTA Clicked**。

1. 儲存變更。**CTA Clicked**&#x200B;規則現在應具備下列設定：

   ![最終啟動設定](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 聽取事 `cmp:click` 件。
   * **2.** 檢查事件是否由按鈕或預 **** 告 **觸發**。
   * **3.** 將的Analytics變數設為，以 **便** 將元件ID設 **為eVar** **、prop**&#x200B;和 **事件**。
   * **4.** 傳送Analytics追蹤連結信標(且請勿 **** 將其視為頁面檢視)。

1. 儲存所有變更並建置您的Launch程式庫，並提升至適當的環境。

## 驗證追蹤連結信標和Analytics呼叫

現在，**CTA Clicked**&#x200B;規則傳送了Analytics信標，您應該可以使用Analytics Debugger查看Analytics追蹤變數。

1. 在瀏覽器中開啟[WKND網站](https://wknd.site/us/en.html)。
1. 按一下Debugger圖示![Experience Platform Debugger圖示](assets/track-clicked-component/experience-cloud-debugger.png)以開啟Experience PlatformDebugger。
1. 如先前所述，確認Debugger將Launch屬性對應至&#x200B;*您的*&#x200B;開發環境，且已勾選&#x200B;**主控台記錄**。
1. 開啟Analytics功能表，並確認報表套裝已設為&#x200B;*您的*&#x200B;報表套裝。

   ![Analytics標籤除錯程式](assets/track-clicked-component/analytics-tab-debugger.png)

1. 在瀏覽器中，按一下&#x200B;**Teaser**&#x200B;或&#x200B;**Button** CTA按鈕之一以導覽至其他頁面。

   ![要點按的CTA按鈕](assets/track-clicked-component/cta-button-to-click.png)

1. 返回Experience Platform偵錯器，向下捲動並展開&#x200B;**網路請求** > *您的報表套裝*。 您應該能夠找到&#x200B;**eVar**、**prop**&#x200B;和&#x200B;**event**&#x200B;集。

   ![點按時追蹤的Analytics事件、eVar和Prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 返回瀏覽器，開啟開發人員主控台。 導覽至網站頁尾，然後按一下其中一個導覽連結：

   ![按一下頁尾中的導覽連結](assets/track-clicked-component/click-navigation-link-footer.png)

1. 在瀏覽器主控台中，觀察規則「CTA已點按」的訊息&#x200B;*「自訂程式碼」不符合*。

   這是因為導覽元件確實會觸發`cmp:click`事件&#x200B;*但*，因為我們會根據資源類型檢查，不會採取任何動作。

   >[!NOTE]
   >
   > 如果您沒有看見任何主控台記錄，請確定已在Experience Platform偵錯器的&#x200B;**Launch**&#x200B;下勾選&#x200B;**主控台記錄**。

## 恭喜！

您只是使用事件導向的Adobe用戶端資料層和Experience Platform Launch，來追蹤Adobe Experience Manager網站上特定元件的點按次數。