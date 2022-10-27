---
title: 在AEM Forms打印通道文檔中使用表元件
seo-title: Using Table Component in AEM Forms Print Channel Document
description: 以下影片會逐步說明在互動式通訊中使用表格元件以列印管道檔案所需的步驟。
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# 在AEM Forms打印通道文檔中使用表元件 {#using-table-component-in-aem-forms-print-channel-document}

以下影片會逐步說明在互動式通訊中使用表格元件以列印管道檔案所需的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

表格用於以表格方式顯示資料。 表格中的列需要根據資料來源傳回的資料而增長或縮小。 若要在列印管道檔案中使用表格，我們需要使用AEM Forms Designer建立版面檔案（xdp檔案）。 在此佈局檔案中，我們添加包含所需列數的表。 根據您的需求，確定欄位對象類型為TextField或Numeric Field。 對於每列，欄位確保資料綁定設定為「使用名稱」。

>[!NOTE]
>
>若要讓表格動態，請確定您已將列標示為重複。

**在您自己的伺服器上試用**

* [將資產檔案下載並解壓縮至硬碟](assets/usingtablesinprintchannel.zip)

* 使用封裝管理程式將兩個zip檔案匯入AEM

* 與本文相關的資產包括：

   * 布局片段

   * 表單資料強制回應視窗

   * 互動式通信文檔
   * sampleretirementaccountdata.json

* 在中開啟互動式通訊檔案 [編輯模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* 將TableDemo版面片段新增至貢獻區段。
* 將表格儲存格系結至適當的「表單資料模型」元素，如影片所示

* 預覽互動式通訊檔案，其中包含提供給您的範例json資料檔案
