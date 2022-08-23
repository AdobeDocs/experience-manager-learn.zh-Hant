---
title: 發展AEM Sites的資源地位
description: 'Adobe Experience Manager的資源狀態API是一個可插入框架，用於在各種編輯器Web UI中AEM顯示狀態消息。 '
topics: development
audience: developer
doc-type: tutorial
activity: develop
version: 6.4, 6.5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# 開發資源狀態 {#developing-resource-statuses-in-aem-sites}

Adobe Experience Manager的資源狀態API是一個可插入框架，用於在各種編輯器Web UI中AEM顯示狀態消息。

## 概觀 {#overview}

編輯器資源狀態框架提供了伺服器端和客戶端API，用於以標準和統一的方式顯示和與編輯器狀態交互。

編輯器狀態欄在的「頁面」、「體驗片段」和「模板」編輯器中本機可AEM用。

自定義資源狀態提供程式的示例使用案例有：

* 在計畫激活後2小時內通知作者
* 通知作者過去15分鐘內已激活頁面
* 通知作者在過去5分鐘內編輯了頁面，由誰編輯

![AEM編輯器資源狀態概述](assets/sample-editor-resource-status-screenshot.png)

## 資源狀態提供程式框架 {#resource-status-provider-framework}

在開發自定義資源狀態時，開發工作包括：

1. ResourceStatusProvider實現，負責確定是否需要狀態，以及有關狀態的基本資訊：標題、消息、優先順序、變型、表徵圖和可用操作。
2. （可選）實現任何可用操作的功能的GraniteUI JavaScript。

   ![資源狀態體系](assets/sample-editor-resource-status-application-architecture.png)

3. 作為「頁面」、「體驗片段」和「模板」編輯器的一部分提供的狀態資源通過資源「」指定類型[!DNL statusType]」屬性。

   * 頁面編輯器： `editor`
   * 體驗片段編輯器： `editor`
   * 範本編輯器: `template-editor`

4. 狀態資源 `statusType` 與註冊匹配 `CompositeStatusType` 已配置OSGi `name` 屬性。

   對於所有匹配， `CompositeStatusType's` 類型被收集，並用於收集 `ResourceStatusProvider` 具有此類型的實現，通過 `ResourceStatusProvider.getType()`。

5. 匹配 `ResourceStatusProvider` 已通過 `resource` ，並確定 `resource` 具有要顯示的狀態。 如果需要狀態，則此實施將負責構建0或許多 `ResourceStatuses` 返回，每個都表示要顯示的狀態。

   通常， `ResourceStatusProvider` 返回0或1 `ResourceStatus` 每 `resource`。

6. ResourceStatus是可由客戶實現的介面，或幫助 `com.day.cq.wcm.commons.status.EditorResourceStatus.Builder` 可用於構造狀態。 狀態包括：

   * 標題
   * 訊息
   * 圖示
   * 變數
   * 優先順序
   * 動作
   * 資料

7. （可選）如果 `Actions` 為 `ResourceStatus` 對象，支援客戶端需要將功能綁定到狀態欄中的操作連結。

   ```js
   (function(jQuery, document) {
       'use strict';
   
       $(document).on('click', '.editor-StatusBar-action[data-status-action-id="do-something"]', function () {
           // Do something on the click of the resource status action
   
       });
   })(jQuery, document);
   ```

8. 任何支援這些操作的JavaScript或CSS都必須通過每個編輯器各自的客戶端庫進行代理，以確保前端代碼在編輯器中可用。

   * 頁面編輯器類別： `cq.authoring.editor.sites.page`
   * 體驗片段編輯器類別： `cq.authoring.editor.sites.page`
   * 模板編輯器類別： `cq.authoring.editor.sites.template`

## 查看代碼 {#view-the-code}

[查看GitHub上的代碼](https://github.com/Adobe-Consulting-Services/acs-aem-samples/tree/master/bundle/src/main/java/com/adobe/acs/samples/resourcestatus/impl/SampleEditorResourceStatusProvider.java)

## 其他資源 {#additional-resources}

* [`com.adobe.granite.resourcestatus` Java文檔](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/resourcestatus/package-summary.html)
* [`com.day.cq.wcm.commons.status.EditorResourceStatus` Java文檔](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/commons/status/EditorResourceStatus.html)
