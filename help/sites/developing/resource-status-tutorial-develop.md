---
title: 在AEM Sites中開發資源狀態
description: 'Adobe Experience Manager的Resource Status API''s是可插拔的架構，可在AEM的各種編輯器網頁UI中顯示狀態訊息。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# 開發資源狀態 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager的Resource Status API&#39;s是可插拔的架構，可在AEM的各種編輯器網頁UI中顯示狀態訊息。

## 概覽 {#overview}

Resource Status for Editors架構提供伺服器端和用戶端API，以標準且一致的方式顯示及與編輯器狀態互動。

AEM的「頁面」、「體驗片段」和「範本」編輯器中，原生可使用編輯器狀態列。

自定義資源狀態提供器的示例使用案例包括：

* 在計畫啟動後2小時內通知作者頁面
* 通知作者頁面已在過去15分鐘內啟動
* 通知作者頁面在最近5分鐘內被編輯，以及由誰編輯

![AEM編輯器資源狀態概觀](assets/sample-editor-resource-status-screenshot.png)

## 資源狀態提供者框架 {#resource-status-provider-framework}

在開發自定義資源狀態時，開發工作包括：

1. ResourceStatusProvider實施，負責確定是否需要狀態，以及有關狀態的基本資訊：標題、訊息、優先順序、變體、圖示和可用動作。
2. （可選）實施任何可用動作功能的GraniteUI JavaScript。

   ![資源狀態體系](assets/sample-editor-resource-status-application-architecture.png)

3. 作為「頁面」、「體驗片段」和「範本」編輯器一部分提供的狀態資源，會透過資源「[!DNL statusType]」屬性提供類型。

   * 頁面編輯器： `editor`
   * Experience Fragment editor: `editor`
   * 範本編輯器: `template-editor`

4. 狀態資源的與已注 `statusType` 冊的OSGi配置 `CompositeStatusType` 屬性相 `name` 匹配。

   對於所有匹配項， `CompositeStatusType's` 會收集這些類型，並用於收集具 `ResourceStatusProvider` 有此類型的實施，方法為 `ResourceStatusProvider.getType()`。

5. 匹配項 `ResourceStatusProvider` 在編輯器 `resource` 中傳遞，並確定是否 `resource` 有要顯示的狀態。 如果需要狀態，此實作負責建立0或許多要傳回的 `ResourceStatuses` 項目，每個項目代表要顯示的狀態。

   通常，每 `ResourceStatusProvider` 個返回0 `ResourceStatus` 或1 `resource`。

6. ResourceStatus是可由客戶實施的介面，或者可用 `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` 於構建狀態。 狀態包括：

   * 標題
   * 訊息
   * 圖示
   * 變數
   * 優先順序
   * 動作
   * 資料

7. 或者，如果 `Actions` 為對象提供 `ResourceStatus` 了，則需要支援客戶端將功能綁定到狀態欄中的操作連結。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 任何支援JavaScript或CSS以支援這些動作的支援，都必須透過每個編輯器的個別用戶端程式庫來編寫，以確保編輯器中有前端程式碼可供使用。

   * 頁面編輯器類別： `cq.authoring.editor.sites.page`
   * 體驗片段編輯器類別： `cq.authoring.editor.sites.page`
   * 範本編輯器類別： `cq.authoring.editor.sites.template`

## 檢視程式碼 {#view-the-code}

[請參閱GitHub上的程式碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 其他資源 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
