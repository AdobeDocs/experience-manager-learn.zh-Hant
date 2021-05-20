---
title: 使用觀看的資料夾產生列印管道檔案
seo-title: 使用觀看的資料夾產生列印管道檔案
description: 這是建立打印管道第一個互動式通訊檔案的多步驟教學課程的10部分。 在本部分，我們將使用觀看的資料夾機制生成打印渠道文檔。
seo-description: 這是建立打印管道第一個互動式通訊檔案的多步驟教學課程的10部分。 在本部分，我們將使用觀看的資料夾機制生成打印渠道文檔。
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
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# 使用觀看的資料夾產生列印管道檔案

在本部分，我們將使用觀看的資料夾機制生成打印渠道文檔。

在建立和測試打印渠道文檔後，我們需要一種機制以批處理模式或按需生成這些文檔。 通常，這些類型的檔案會以批次模式產生，最常見的機制是使用監看的資料夾。

在AEM中設定觀看資料夾時，會關聯將檔案放入觀看資料夾時執行的ECMA指令碼或java程式碼。 本文將重點介紹ECMA指令碼，該指令碼將生成打印通道文檔並將其保存到檔案系統中。

觀看的資料夾設定和ECMA指令碼是您在本教學課程[開頭匯入的資產的一部分](introduction.md)

拖放至監看資料夾的輸入檔案具有下列結構。 ECMA指令碼讀取帳號，並為每個帳戶生成打印渠道文檔。

有關生成文檔的ECMA指令碼的詳細資訊，請[參閱本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

要使用觀看的資料夾機制生成打印通道文檔，請執行以下步驟：

* [請遵循本檔案中提及的步驟](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登入crx並導覽至/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 確保interactiveCommunicationsDocument的路徑指向要打印的正確文檔。（第1行）
* 記下saveLocation（第2行）。您可以根據需要進行更改。
* 請確定表單資料模型的輸入參數已系結至「請求屬性」，且其系結值設為「accountnumber」。 請參閱下方的螢幕截圖。
   ![請求](assets/requestattributeprintchannel.gif)

* 建立accountnumbers.xml檔案，其內容如下

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

* 將xml檔案拖放到C:\RenderPrintChannel\input中

* 按照ECMA指令碼中的指定，在保存位置檢查pdf檔案。




