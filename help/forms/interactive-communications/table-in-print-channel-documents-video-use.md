---
title: 在AEM Forms Print Channel檔案中使用表格元件
description: 以下影片會逐步解說在Interactive Communications中為列印管道檔案使用表格元件所需的步驟。
feature: Interactive Communication
doc-type: technical video
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
duration: 277
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# 在AEM Forms Print Channel檔案中使用表格元件 {#using-table-component-in-aem-forms-print-channel-document}

以下影片會逐步解說在Interactive Communications中為列印管道檔案使用表格元件所需的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

表格是用來以表格方式顯示資料。 表格中的列需要根據資料來源傳回的資料而增加或縮小。 若要在列印管道檔案中使用表格，我們需要使用AEM Forms Designer建立版面檔案（xdp檔案）。 在此版面配置檔案中，我們新增具有所需欄數的表格。 根據您的要求，確定欄欄位物件型別是TextField或Numeric Field。 對於每個欄，欄位會確定資料繫結已設定為「使用名稱」。

>[!NOTE]
>
>若要讓表格成為動態表格，請確定您已將此列標示為重複。

**在您自己的伺服器上試用**

* [將資產檔案下載並解壓縮至您的硬碟](assets/usingtablesinprintchannel.zip)

* 使用封裝管理員將兩個zip檔案匯入AEM

* 與本文相關的資產包括以下專案：

   * 布局片段

   * 表單資料模組

   * 互動式通訊檔案
   * sampleretirementaccountdata.json

* 以[編輯模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)開啟互動式通訊檔案。

* 將TableDemo版面片段新增至貢獻區段。
* 將表格儲存格繫結至適當的表單資料模型元素，如影片所示

* 使用提供給您的範例json資料檔案預覽互動式通訊檔案
