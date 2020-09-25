---
title: 在AEM Forms列印頻道檔案中使用表格元件
seo-title: 在AEM Forms列印頻道檔案中使用表格元件
description: 以下視訊將逐步介紹在Interactive Communications中使用表格元件以列印頻道檔案的步驟。
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# 在AEM Forms列印頻道檔案中使用表格元件 {#using-table-component-in-aem-forms-print-channel-document}

以下視訊將逐步介紹在Interactive Communications中使用表格元件以列印頻道檔案的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

表格用於以表格形式顯示資料。 表格中的列需要根據資料源返回的資料增長或縮小。 若要在列印頻道檔案中使用表格，我們需要使用AEM Forms Designer建立版面檔案（xdp檔案）。 在此版面檔案中，我們新增具有所需欄數的表格。 請根據您的需求，確定欄位物件類型為TextField或Numeric Field。 對於每個欄，欄位會確定資料系結已設為「使用名稱」。

>[!NOTE]
要使表動態，請確保已將行標籤為重複。

**在您自己的伺服器上試用**

* [下載資產檔案並解壓縮至您的硬碟](assets/usingtablesinprintchannel.zip)

* 使用套件管理器將兩個Zip檔案匯入AEM

* 與本文相關的資產包含下列項目：

   * 布局片段

   * 表單資料模式

   * 互動式通訊檔案
   * sampleretirementaccountdata.json

* 以編輯模式開啟「互動式通訊 [檔案」](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)。

* 將TableDemo版面片段新增至貢獻區段。
* 將表格儲存格系結至視訊中顯示的適當表單資料模型元素

* 使用提供給您的範例json資料檔案預覽互動式通訊檔案

