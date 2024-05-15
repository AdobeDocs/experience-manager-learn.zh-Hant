---
title: 使用Watched資料夾產生Print Channel檔案
description: 這是多步驟教學課程的第10部分，說明如何為列印管道建立您的第一份互動式通訊檔案。 在本部分中，我們將使用watched資料夾機制來產生列印管道檔案。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
duration: 70
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# 使用Watched資料夾產生Print Channel檔案

在本部分中，我們將使用watched資料夾機制來產生列印管道檔案。

建立及測試您的列印管道檔案後，我們需要一種機制，以批次模式或依需求產生這些檔案。 通常，這些型別的檔案會以批次模式產生，最常見的機制是使用watched資料夾。

當您在AEM中設定watched資料夾時，您會關聯ECMA指令碼或Java程式碼，這些程式碼會在檔案放入watched資料夾時執行。 在本文中，我們將重點介紹ECMA指令碼，它會產生列印通道檔案並將其儲存至檔案系統。

watched資料夾設定和ECMA指令碼是您於匯入的資產的一部分 [本教學課程的開頭](introduction.md)

放入watched資料夾的輸入檔案具有下列結構。 ECMA指令碼會讀取帳號，並產生每個帳號的列印管道檔案。

如需有關產生檔案的ECMA指令碼的詳細資訊， [請參閱本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

若要使用watched資料夾機制產生列印channel檔案，請遵循下列步驟：

* [請依照本檔案提及的步驟操作](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登入crx並導覽至/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 請確定interactiveCommunicationsDocument的路徑指向您要列印的正確檔案。（第1行）
* 記下saveLocation （第2行）。您可以視需要加以變更。
* 請確定表單資料模型的輸入引數已繫結至要求屬性，且其繫結值已設為「accountnumber」。 請參閱下方的熒幕擷圖。
  ![請求](assets/requestattributeprintchannel.gif)

* 建立含有以下內容的accountnumbers.xml檔案

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

* 依照ECMA指令碼中的指定，檢查儲存位置中的pdf檔案。

## 後續步驟

[在表單提交時開啟代理程式UI](./opening-agent-ui-on-form-submission.md)