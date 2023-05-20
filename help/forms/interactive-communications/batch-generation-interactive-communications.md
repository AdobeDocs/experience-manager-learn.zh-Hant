---
title: 使用批處理API生成交互通信文檔
description: 使用批API生成打印通道文檔的樣例資產
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

# 批處理API

可以使用批處理API從模板生成多個交互通信。 該模板是沒有任何資料的互動式通信。 批處理API將資料與模板組合以生成互動式通信。 該API在互動式通信的批量生產中是有用的。 例如，電話帳單、多個客戶的信用卡對帳單。

[瞭解有關批處理生成API的詳細資訊](https://experienceleague.adobe.com/docs/experience-manager-65/forms/interactive-communications/generate-multiple-interactive-communication-using-batch-api.html)

本文提供了使用Batch API生成Interactive Communications文檔的示例資產。

## 使用監視資料夾生成批

* 導入 [互動式通信模板](assets/Beneficiaries-confirmation.zip) 你的AEM Forms伺服器。
* 導入 [監視資料夾配置](assets/batch-generation-api.zip)。 這將建立名為 `batchAPI` 在C盤上。

**如果您在非Windows作業系統上運行AEM Forms，請執行以下三個步驟：**

1. [開啟受監視的資料夾](http://localhost:4502/libs/fd/core/WatchfolderUI/content/UI.html)
2. 選擇BatchAPIWatchedFolder，然後按一下編輯。
3. 更改路徑以匹配您的作業系統。

![路徑](assets/watched-folder-batch-api-basic.PNG)

* 下載並解壓縮 [zip檔案](assets/jsonfile.zip)。 zip檔案包含名為 `jsonfile` 包含 `beneficiaries.json` 的子菜單。 此檔案具有生成3個文檔的資料。

* 刪除 `jsonfile` 資料夾。
* 一旦拾取該資料夾進行處理，請檢查監視資料夾的結果資料夾。 您應看到生成的3個PDF檔案

## 使用REST請求生成批

您可以調用 [批處理API](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html) 通過REST請求。 您可以為其他應用程式公開REST端點以調用API來生成文檔。
提供的示例資產公開了用於生成Interactive Communication文檔的REST終結點。 Servlet接受以下參數：

* fileName — 檔案系統上資料檔案的位置。
* templatePath - IC模板路徑
* saveLocation — 用於在檔案系統上保存生成的文檔的位置
* channelType — 打印、Web或兩者
* recordId — 用於設定交互通信名稱的元素的JSON路徑

以下螢幕快照顯示參數及其值
![示例請求](assets/generate-ic-batch-servlet.PNG)

## 在伺服器上部署示例資產

* 導入 [ICT模板](assets/ICTemplate.zip) 使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 導入 [自定義提交處理程式](assets/BatchAPICustomSubmit.zip) 使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 導入 [自適應窗體](assets/BatchGenerationAPIAF.zip) 使用 [Forms和文檔介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 部署和啟動 [自定義OSGI捆綁包](assets/batchgenerationapi.batchgenerationapi.core-1.0-SNAPSHOT.jar) 使用 [Felix Web控制台](http://localhost:4502/system/console/bundles)
* [通過提交表單觸發批生成](http://localhost:4502/content/dam/formsanddocuments/batchgenerationapi/jcr:content?wcmmode=disabled)
