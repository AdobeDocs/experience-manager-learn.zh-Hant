---
title: 在AEM Forms打印通道文檔中使用表元件
seo-title: 在AEM Forms打印通道文檔中使用表元件
description: 以下影片會逐步說明在互動式通訊中使用表格元件以列印管道檔案所需的步驟。
feature: 互動式通訊
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 1%

---


# 在AEM Forms打印通道文檔{#using-table-component-in-aem-forms-print-channel-document}中使用表元件

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

* 在[編輯模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)中開啟互動式通信文檔。

* 將TableDemo版面片段新增至貢獻區段。
* 將表格儲存格系結至適當的「表單資料模型」元素，如影片所示

* 預覽互動式通訊檔案，其中包含提供給您的範例json資料檔案

