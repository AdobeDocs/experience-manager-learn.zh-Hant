---
title: 使用Watched資料夾產生列印渠道檔案
seo-title: 使用Watched資料夾產生列印渠道檔案
description: 這是多步驟教學課程的第10部分，可協助您建立適用於列印頻道的第一個互動式通訊檔案。 在本部分，我們將使用受監視的資料夾機制產生列印渠道檔案。
seo-description: 這是多步驟教學課程的第10部分，可協助您建立適用於列印頻道的第一個互動式通訊檔案。 在本部分，我們將使用受監視的資料夾機制產生列印渠道檔案。
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: 互動式通訊
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: 開發
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 1%

---


# 使用Watched資料夾產生列印渠道檔案

在本部分，我們將使用受監視的資料夾機制產生列印渠道檔案。

在建立並測試您的列印渠道檔案後，我們需要一種機制來以批次模式或隨選方式產生這些檔案。 通常，這些類型的檔案會以批次模式產生，最常見的機制是使用受監視的資料夾。

在中配置監視資料夾時，AEM將關聯在檔案放入監視資料夾時執行的ECMA指令碼或Java代碼。 在本文中，我們將重點介紹ECMA指令碼，它將生成打印通道文檔並將其保存到檔案系統。

監視的資料夾配置和ECMA指令碼是您在本教程[開頭導入的資產的一部分。](introduction.md)

拖放至監看資料夾的輸入檔案具有下列結構。 ECMA指令碼讀取帳戶編號並為每個帳戶生成打印渠道文檔。

有關生成文檔的ECMA指令碼的詳細資訊，請參閱本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)[

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

若要使用監看的資料夾機制產生列印渠道檔案，請遵循下列步驟：

* [依照本檔案中提及的步驟進行](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登入crx並導覽至/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 請確定interactiveCommunicationsDocument的路徑指向您要列印的正確檔案。（第1行）
* 記下saveLocation（第2行）。您可以根據需要進行更改。
* 請確定表單資料模型的輸入參數已系結至「請求屬性」，其系結值已設為「accountnumber」。 請參閱下面的螢幕擷取。
   ![請求](assets/requestattributeprintchannel.gif)

* 使用下列內容建立accountnumbers.xml檔案

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* 將xml檔案拖放至C:\RenderPrintChannel\input

* 如ECMA指令碼中所指定，在儲存位置檢查pdf檔案。




