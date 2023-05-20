---
title: 使用監視資料夾生成打印渠道文檔
seo-title: Generating Print Channel Documents Using Watched Folder
description: 這是為打印通道建立第一個互動式通信文檔的多步驟教程的第10部分。 在本部分，我們將使用監視資料夾機制生成打印渠道文檔。
seo-description: This is part 10 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will generate print channel documents using the watched folder mechanism.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# 使用監視資料夾生成打印渠道文檔

在本部分，我們將使用監視資料夾機制生成打印渠道文檔。

在建立和測試打印渠道文檔後，我們需要一種機制以批處理模式或按需生成這些文檔。 通常，這些類型的文檔是以批處理模式生成的，最常見的機制是使用監視資料夾。

在中配置監視資料夾時AEM，會關聯在檔案被放入監視資料夾時執行的ECMA指令碼或java代碼。 本文將重點介紹ECMA指令碼，該指令碼將生成打印通道文檔並將其保存到檔案系統。

監視的資料夾配置和ECMA指令碼是您在 [本教程開始](introduction.md)

掉到監視資料夾中的輸入檔案具有以下結構。 ECMA指令碼讀取帳戶號並為每個帳戶生成打印通道文檔。

有關生成文檔的ECMA指令碼的詳細資訊， [請參閱本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

要使用監視資料夾機制生成打印通道文檔，請執行以下步驟：

* [按照本文檔中提及的步驟操作](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登錄到crx並導航到/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 確保interactiveCommunicationsDocument的路徑指向要打印的正確文檔。（第1行）
* 記下saveLocation（第2行）。您可以根據需要更改它。
* 確保表單資料模型的輸入參數綁定到請求屬性，其綁定值設定為「accountnumber」。 請參閱下面的螢幕截圖。
   ![請求](assets/requestattributeprintchannel.gif)

* 建立具有以下內容的accountnumbers.xml檔案

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

* 將xml檔案放入C:\RenderPrintChannel\input

* 按照ECMA指令碼中指定的方式，在保存位置檢查pdf檔案。

## 後續步驟

[在提交表單時開啟代理UI](./opening-agent-ui-on-form-submission.md)