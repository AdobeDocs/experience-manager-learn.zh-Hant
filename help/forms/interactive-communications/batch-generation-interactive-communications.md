---
title: 使用批處理API生成互動式通信文檔
description: 使用批次API產生列印管道檔案的範例資產
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 2cdf37e6-42ad-469a-a6e4-a693ab2ca908
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 2%

---

# 批次API

您可以使用批次API從範本產生多個互動式通訊。 範本是互動式通訊，不含任何資料。 批次API將資料與範本結合，以產生互動式通訊。 API在大量生產互動式通訊中很有用。 例如，電話單、多個客戶的信用卡對帳單。

[進一步了解批次產生API](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供使用批次API產生互動式通訊檔案的範例資產。

## 使用監看資料夾批次產生

* 匯入 [互動式通訊範本](assets/Beneficiaries-confirmation.zip) 進入您的AEM Forms伺服器。
* 匯入 [監視資料夾配置](assets/batch-generation-api.zip). 這會建立名為 `batchAPI` 在C驅動器中。

**如果您在非windows作業系統上執行AEM Forms，請遵循下列3個步驟：**

1. [開啟已觀看的資料夾](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 選擇BatchAPIWatchedFolder，然後按一下編輯。
3. 變更路徑以符合您的作業系統。

![路徑](assets/watched-folder-batch-api-basic.PNG)

* 下載並擷取 [zip檔案](assets/jsonfile.zip). zip檔案包含名為的資料夾 `jsonfile` 包含 `beneficiaries.json` 檔案。 此檔案具有生成3個文檔的資料。

* 放置 `jsonfile` 資料夾放入監看資料夾的輸入資料夾中。
* 擷取資料夾以進行處理後，檢查已監看資料夾的結果資料夾。 您應會看到3個PDF檔案產生

## 使用REST請求生成批

您可以叫用 [批次API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) 通過REST請求。 您可以公開其他應用程式的REST端點，以叫用API來產生檔案。
提供的範例資產會公開REST端點，以產生互動式通訊檔案。 Servlet接受以下參數：

* fileName — 資料檔案在檔案系統上的位置。
* templatePath - IC模板路徑
* saveLocation — 在檔案系統上保存生成文檔的位置
* channelType — 打印、Web或兩者
* recordId — 設定互動式通訊名稱之元素的JSON路徑

以下螢幕截圖顯示參數及其值
![範例要求](assets/generate-ic-batch-servlet.PNG)

## 在伺服器上部署範例資產

* 匯入 [ICT模板](assets/ICTemplate.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 匯入 [自訂提交處理常式](assets/BatchAPICustomSubmit.zip) 使用 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 匯入 [適用性表單](assets/BatchGenerationAPIAF.zip) 使用 [Forms和檔案介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 部署並啟動 [自訂OSGI套件組合](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) 使用 [Felix Web Console](http://localhost:4502/system/console/bundles)
* [提交表單以觸發批生成](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
