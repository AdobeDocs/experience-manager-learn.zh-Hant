---
title: 使用批次API生成互動式通信文檔
description: 使用批次API產生列印渠道檔案的範例資產
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 3%

---


# 批次API

您可以使用Batch API，從範本產生多種互動式通訊。 範本是互動式通訊，不含任何資料。 批次API將資料與範本結合，以產生互動式通訊。 API在大量製作互動式通訊時很實用。 例如，電話帳單、多名客戶的信用卡帳單。

[進一步瞭解批次產生API](https://docs.adobe.com/content/help/en/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供使用批次API產生互動式通訊檔案的範例資產。

## 使用Watched資料夾產生批次

* 將[互動式通訊範本](assets/Beneficiaries-confirmation.zip)匯入您的AEM Forms伺服器。
* 導入[watched資料夾配置](assets/batch-generation-api.zip)。 這將在C驅動器中建立一個名為`batchAPI`的資料夾。

**如果您正在非Windows作業系統上執行AEM Forms，請遵循下列3個步驟：**

1. [開啟監看的資料夾](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 選擇「BatchAPIWpattedFolder」，然後按一下「編輯」。
3. 變更路徑以符合您的作業系統。

![路徑](assets/watched-folder-batch-api-basic.PNG)

* 下載並解壓縮[zip檔案](assets/jsonfile.zip)的內容。 zip檔案包含名為`jsonfile`的資料夾，其中包含`beneficiaries.json`檔案。 此檔案具有生成3個文檔的資料。

* 將`jsonfile`資料夾拖曳至您所監視資料夾的輸入資料夾。
* 在擷取資料夾以進行處理後，請檢查您所監視資料夾的結果資料夾。 您應該會看到產生3個PDF檔案

## 使用REST請求生成批

您可以通過REST請求調用[批處理API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html)。 您可以為其他應用程式公開REST端點，以叫用API來產生檔案。
提供的範例資產會公開REST端點，以產生互動式通訊檔案。 Servlet接受以下參數：

* fileName —檔案系統上資料檔案的位置。
* templatePath - IC模板路徑
* saveLocation —— 在檔案系統上保存生成文檔的位置
* channelType —— 列印、網頁或兩者
* recordId —— 元素的JSON路徑，用以設定互動式通訊的名稱

下列螢幕擷取顯示參數及其值
![sample request](assets/generate-ic-batch-servlet.PNG)

## 在伺服器上部署範例資產

* 使用[軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)導入[ICTemplate](assets/ICTemplate.zip)
* 使用[套件管理器](http://localhost:4502/crx/packmgr/index.jsp)匯入[自訂提交處理常式](assets/BatchAPICustomSubmit.zip)
* 使用[Forms和文檔介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)導入[最適化表單](assets/BatchGenerationAPIAF.zip)
* 使用[Felix網頁主控台](http://localhost:4502/system/console/bundles)部署並啟動[自訂OSGI套件](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)
* [提交表單以觸發批次產生](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
