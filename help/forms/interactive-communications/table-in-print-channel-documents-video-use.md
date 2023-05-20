---
title: 在AEM Forms打印渠道文檔中使用表元件
seo-title: Using Table Component in AEM Forms Print Channel Document
description: 以下視頻將介紹在Interactive Communications中使用表元件打印渠道文檔所需的步驟。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# 在AEM Forms打印渠道文檔中使用表元件 {#using-table-component-in-aem-forms-print-channel-document}

以下視頻將介紹在Interactive Communications中使用表元件打印渠道文檔所需的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

表用於以表格方式顯示資料。 表中的行需要根據資料源返回的資料增長或收縮。 要使用打印通道文檔中的表，需要使用AEM Forms設計器建立佈局檔案（xdp檔案）。 在此佈局檔案中，我們將添加具有所需列數的表。 根據您的要求，確保列欄位對象類型為「文本欄位」或「數字欄位」。 對於每列，欄位確保資料綁定設定為「使用名稱」。

>[!NOTE]
>
>要使表動態，請確保已將行標籤為重複。

**在您自己的伺服器上嘗試**

* [將資產檔案下載並解壓縮到硬碟](assets/usingtablesinprintchannel.zip)

* 使用包管理器將兩個ZIP文AEM件導入

* 與本條相關的資產包括：

   * 布局片段

   * 表單資料模式

   * 互動式通信文檔
   * sampleretirementaccountdata.json

* 在中開啟互動式通信文檔 [編輯模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)。

* 將TableDemo佈局片段添加到「貢獻」部分。
* 將表單元格綁定到視頻中顯示的相應表單資料模型元素

* 預覽提供給您的示例json資料檔案的互動式通信文檔
