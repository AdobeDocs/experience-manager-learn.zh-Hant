---
title: 在AEM Sites中開發資源狀態
description: 'Adobe Experience Manager的資源狀態API是可插拔的架構，用於在AEM各種編輯器Web UI中公開狀態訊息。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# 開發資源狀態 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager的資源狀態API是可插拔的架構，用於在AEM各種編輯器Web UI中公開狀態訊息。

## 概覽 {#overview}

Editors的資源狀態框架提供伺服器端和用戶端API，以標準且統一的方式顯示及與編輯器狀態互動。

AEM的「頁面」、「體驗片段」和「範本」編輯器原生可使用編輯器狀態列。

自訂資源狀態提供者的範例使用案例包括：

* 在排程啟動後2小時內通知作者
* 通知作者頁面已在過去15分鐘內啟動
* 通知作者過去5分鐘內已編輯頁面，以及編輯者

![AEM編輯器資源狀態概觀](assets/sample-editor-resource-status-screenshot.png)

## 資源狀態提供程式框架 {#resource-status-provider-framework}

開發自訂資源狀態時，開發工作包括：

1. ResourceStatusProvider實施，負責確定是否需要狀態，以及有關狀態的基本資訊：標題、訊息、優先順序、變體、圖示和可用動作。
2. （可選）實作任何可用動作功能的GraniteUI JavaScript。

   ![資源狀態體系結構](assets/sample-editor-resource-status-application-architecture.png)

3. 在頁面、體驗片段和範本編輯器中提供的狀態資源是透過資源「[!DNL statusType]」屬性來指定類型。

   * 頁面編輯器：`editor`
   * 體驗片段編輯器：`editor`
   * 範本編輯器: `template-editor`

4. 狀態資源的`statusType`與已註冊的`CompositeStatusType` OSGi配置的`name`屬性匹配。

   對於所有符合項目，會收集`CompositeStatusType's`類型，並透過`ResourceStatusProvider.getType()`用於收集具有此類型的`ResourceStatusProvider`實作。

5. 在編輯器中傳遞匹配的`ResourceStatusProvider`，並確定`resource`是否具有要顯示的狀態。 `resource`如果需要狀態，此實作會負責建置0或許多要傳回的`ResourceStatuses`，每個都代表要顯示的狀態。

   通常， `ResourceStatusProvider`會針對每個`resource`傳回0或1個`ResourceStatus`。

6. ResourceStatus是可由客戶實施的介面，或有用的`com.day.cq.wcm.commons.status.EditorResourceStatus.Builder`可用於構造狀態。 狀態由下列部分組成：

   * 標題
   * 訊息
   * 圖示
   * 變數
   * 優先順序
   * 動作
   * 資料

7. 或者，如果為`ResourceStatus`對象提供了`Actions` ，則需要支援的clientlibs將功能綁定到狀態欄中的操作連結。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 任何支援JavaScript或CSS以支援這些動作，都必須透過每個編輯器的個別用戶端程式庫來代理，以確保前端程式碼可在編輯器中使用。

   * 頁面編輯器類別：`cq.authoring.editor.sites.page`
   * 體驗片段編輯器類別：`cq.authoring.editor.sites.page`
   * 範本編輯器類別：`cq.authoring.editor.sites.template`

## 檢視程式碼 {#view-the-code}

[請參閱GitHub上的程式碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 其他資源 {#additional-resources}

* [`com.adobe.granite.resourcestatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` JavaDocs](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
