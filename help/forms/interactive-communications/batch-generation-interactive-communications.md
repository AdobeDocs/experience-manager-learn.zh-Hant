---
title: 使用批處理API生成互動式通信文檔
description: 使用批次API產生列印管道檔案的範例資產
feature: 互動式通訊
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 3%

---


# 批次API

您可以使用批次API從範本產生多個互動式通訊。 範本是互動式通訊，不含任何資料。 批次API將資料與範本結合，以產生互動式通訊。 API在大量生產互動式通訊中很有用。 例如，電話單、多個客戶的信用卡對帳單。

[進一步了解批次產生API](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供使用批次API產生互動式通訊檔案的範例資產。

## 使用監看資料夾批次產生

* 將[互動式通訊範本](assets/Beneficiaries-confirmation.zip)匯入AEM Forms伺服器。
* 匯入[watched資料夾設定](assets/batch-generation-api.zip)。 這將在C驅動器中建立名為`batchAPI`的資料夾。

**如果您在非windows作業系統上執行AEM Forms，請遵循下列3個步驟：**

1. [開啟已觀看的資料夾](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 選擇BatchAPIWatchedFolder，然後按一下編輯。
3. 變更路徑以符合您的作業系統。

![路徑](assets/watched-folder-batch-api-basic.PNG)

* 下載並解壓縮[zip檔案](assets/jsonfile.zip)的內容。 zip檔案包含名為`jsonfile`的資料夾，該資料夾包含`beneficiaries.json`檔案。 此檔案具有生成3個文檔的資料。

* 將`jsonfile`資料夾拖曳至已監看資料夾的輸入資料夾。
* 擷取資料夾以進行處理後，檢查已監看資料夾的結果資料夾。 您應會看到產生3個PDF檔案

## 使用REST請求生成批

您可以透過REST請求叫用[批次API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html)。 您可以公開其他應用程式的REST端點，以叫用API來產生檔案。
提供的範例資產會公開REST端點，以產生互動式通訊檔案。 Servlet接受以下參數：

* fileName — 資料檔案在檔案系統上的位置。
* templatePath - IC模板路徑
* saveLocation — 在檔案系統上保存生成文檔的位置
* channelType — 打印、Web或兩者
* recordId — 設定互動式通訊名稱之元素的JSON路徑

以下螢幕截圖顯示參數及其值
![範例要求](assets/generate-ic-batch-servlet.PNG)

## 在伺服器上部署範例資產

* 使用[封裝管理器](http://localhost:4502/crx/packmgr/index.jsp)導入[ICTemplate](assets/ICTemplate.zip)
* 使用[套件管理器](http://localhost:4502/crx/packmgr/index.jsp)匯入[自訂提交處理常式](assets/BatchAPICustomSubmit.zip)
* 使用[Forms和檔案介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[適用性表單](assets/BatchGenerationAPIAF.zip)
* 使用[Felix Web控制台](http://localhost:4502/system/console/bundles)部署並啟動[自訂OSGI套件組合](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)
* [提交表單以觸發批生成](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
