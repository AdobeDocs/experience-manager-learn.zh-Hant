---
title: 使用批次API產生互動式通訊檔案
description: 使用批次API產生列印管道檔案的資產範例
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
duration: 77
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---

# 批次API

您可以使用批次API從範本產生多個互動式通訊。 範本是沒有任何資料的互動式通訊。 Batch API將資料與範本結合，以產生互動式通訊。 此API適合用於大量生產互動式通訊。 例如，電話帳單、多個客戶的信用卡對帳單。

[深入瞭解批次產生API](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供使用批次API產生互動式通訊檔案的資產範例。

## 使用Watched資料夾產生批次

* 將[互動式通訊範本](assets/Beneficiaries-confirmation.zip)匯入您的AEM Forms伺服器。
* 匯入[觀察資料夾組態](assets/batch-generation-api.zip)。 這會在您的C磁碟機中建立名為`batchAPI`的資料夾。

**如果您在非Windows作業系統上執行AEM Forms，請遵循下列3個步驟：**

1. [開啟watched資料夾](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 選取BatchAPIWatchedFolder，然後按一下編輯。
3. 變更路徑以符合您的作業系統。

![path](assets/watched-folder-batch-api-basic.PNG)

* 下載並解壓縮[zip檔案](assets/jsonfile.zip)的內容。 Zip檔案包含名為`jsonfile`的資料夾，其中包含`beneficiaries.json`檔案。 此檔案包含產生3份檔案的資料。

* 將`jsonfile`資料夾拖放到watched資料夾的輸入資料夾中。
* 擷取資料夾以進行處理之後，請檢查您的watched資料夾的結果資料夾。 您應該會看到已產生3個PDF檔案

## 使用REST請求產生批次

您可以透過REST要求叫用[批次API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html)。 您可以公開其他應用程式的REST端點，以叫用API來產生檔案。
提供的資產範例會公開REST端點，以用於產生互動式通訊檔案。 此servlet接受下列引數：

* fileName — 資料檔案在檔案系統上的位置。
* templatePath — 互動通訊範本路徑
* saveLocation — 在檔案系統上儲存產生檔案的位置
* channelType — 列印、網頁或兩者
* recordId — 要設定互動式通訊名稱的元素的JSON路徑

下列熒幕擷圖顯示引數及其值
![範例要求](assets/generate-ic-batch-servlet.PNG)

## 在您的伺服器上部署範例資產

* 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)匯入[ICTemplate](assets/ICTemplate.zip)
* 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)匯入[自訂提交處理常式](assets/BatchAPICustomSubmit.zip)
* 使用[Forms和檔案介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[最適化表單](assets/BatchGenerationAPIAF.zip)
* 使用[Felix Web主控台](http://localhost:4502/system/console/bundles)部署並啟動[自訂OSGI套件](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar)
* [透過提交表單](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)觸發批次產生
