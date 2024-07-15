---
title: 在AEM Sites中開發資源狀態
description: Adobe Experience Manager的資源狀態API是可插入的架構，可在AEM的各種編輯器網頁UI中公開狀態訊息。
doc-type: Tutorial
version: 6.4, 6.5
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 2%

---


# 開發資源狀態 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager的資源狀態API是可插入的架構，可在AEM的各種編輯器網頁UI中公開狀態訊息。

## 概觀 {#overview}

編輯器的「資源狀態」架構提供伺服器端和使用者端API，以標準且統一的方式顯示編輯器狀態並與編輯器狀態互動。

編輯器狀態列原本可在AEM的頁面、體驗片段和範本編輯器中使用。

自訂資源狀態提供者的範例使用案例如下：

* 在頁面處於已排程啟動的2小時內時通知作者
* 通知作者某個頁面在過去15分鐘內啟動
* 通知作者某個頁面在過去5分鐘內經過編輯，以及編輯者

![AEM編輯器資源狀態總覽](assets/sample-editor-resource-status-screenshot.png)

## 資源狀態提供者架構 {#resource-status-provider-framework}

開發自訂資源狀態時，開發工作包括：

1. ResourceStatusProvider實作，負責判斷是否需要狀態，以及有關狀態的基本資訊：標題、訊息、優先順序、變體、圖示和可用動作。
2. 可選擇實施任何可用動作功能的GraniteUI JavaScript。

   ![資源狀態架構](assets/sample-editor-resource-status-application-architecture.png)

3. 作為頁面、體驗片段和範本編輯器的一部分提供的狀態資源會透過資源&quot;[!DNL statusType]&quot;屬性獲得型別。

   * 頁面編輯器： `editor`
   * 體驗片段編輯器： `editor`
   * 範本編輯器： `template-editor`

4. 狀態資源的`statusType`與已登入的`CompositeStatusType` OSGi設定的`name`屬性相符。

   針對所有相符專案，會收集`CompositeStatusType's`型別，並用來透過`ResourceStatusProvider.getType()`收集具有此型別的`ResourceStatusProvider`實作。

5. 相符的`ResourceStatusProvider`在編輯器中傳遞到`resource`，並判斷`resource`是否具有要顯示的狀態。 如果需要狀態，則此實作負責建置0或多個要傳回的`ResourceStatuses`，每個都代表要顯示的狀態。

   通常，`ResourceStatusProvider`會傳回每個`resource`的0或1 `ResourceStatus`。

6. ResourceStatus是可由客戶實作的介面，或是有用的`com.day.cq.wcm.commons.status.EditorResourceStatus.Builder`可用來建構狀態。 狀態包含：

   * 標題
   * 訊息
   * 圖示
   * 變數
   * 優先順序
   * 動作
   * 資料

7. 或者，如果為`ResourceStatus`物件提供了`Actions`，則需要支援clientlibs以將功能繫結到狀態列中的動作連結。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 任何支援動作的JavaScript或CSS必須透過每個編輯器的個別使用者端程式庫進行代理，以確保編輯器中可使用前端程式碼。

   * 頁面編輯器類別： `cq.authoring.editor.sites.page`
   * 體驗片段編輯器類別： `cq.authoring.editor.sites.page`
   * 範本編輯器類別： `cq.authoring.editor.sites.template`

## 檢視程式碼 {#view-the-code}

[檢視GitHub上的程式碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 其他資源 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
