---
title: 在AEM Forms打印渠道文檔中使用表元件
seo-title: 在AEM Forms打印渠道文檔中使用表元件
description: 以下視訊將逐步介紹在Interactive Communications中使用表格元件以列印頻道檔案的步驟。
feature: 互動式通訊
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 2%

---


# 在AEM Forms打印渠道文檔{#using-table-component-in-aem-forms-print-channel-document}中使用表元件

以下視訊將逐步介紹在Interactive Communications中使用表格元件以列印頻道檔案的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

表格用於以表格形式顯示資料。 表格中的列需要根據資料源返回的資料增長或縮小。 若要在列印頻道檔案中使用表格，我們需要使用AEM Forms設計人員來建立版面檔（xdp檔案）。 在此版面檔案中，我們新增具有所需欄數的表格。 請根據您的需求，確定欄位物件類型為TextField或Numeric Field。 對於每個欄，欄位會確定資料系結已設為「使用名稱」。

>[!NOTE]
>
>要使表動態，請確保已將行標籤為重複。

**在您自己的伺服器上試用**

* [下載資產檔案並解壓縮至您的硬碟](assets/usingtablesinprintchannel.zip)

* 使用套件管理器將兩個ZipAEM檔案匯入

* 與本文相關的資產包含下列項目：

   * 布局片段

   * 表單資料模式

   * 互動式通訊檔案
   * sampleretirementaccountdata.json

* 在[編輯模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)中開啟交互通信文檔。

* 將TableDemo版面片段新增至貢獻區段。
* 將表格儲存格系結至視訊中顯示的適當表單資料模型元素

* 使用提供給您的範例json資料檔案預覽互動式通訊檔案

