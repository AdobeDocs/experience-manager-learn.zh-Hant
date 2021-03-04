---
title: 在6.5工作流程中使用表單資料AEM模型服務
seo-title: 在6.5工作流程中使用表單資料AEM模型服務
description: AEM Forms6.5引進了在工作流程中建立變數的AEM功能。 有了這項新功能，在工作流程中使用「叫用表單資料模型服務」AEM變得十分簡單。 以下視訊將引導您瞭解在工作流程中使用「叫用表單資料模型服務」所涉AEM及的步驟。
seo-description: AEM Forms6.5引進了在工作流程中建立變數的AEM功能。 有了這項新功能，在工作流程中使用「叫用表單資料模型服務」AEM變得十分簡單。 以下視訊將引導您瞭解在工作流程中使用「叫用表單資料模型服務」所涉AEM及的步驟。
feature: 工作流程
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 1%

---


# 在6.5工作流{#using-form-data-model-service-as-step-in-workflow}中AEM將表單資料模型服務當做步驟

從AEM Forms6.4開始，我們現在可以將表單資料模型服務當成工作流程的一AEM部分。 以下視訊將逐步介紹在「工作流程」中設定「表單資料模型」步驟所需AEM的步驟

>!![NOTE]本視訊中展示的功能需要AEM Forms6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

要在伺服器上測試此功能，請遵循以下說明

* 如[此處](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)所述，使用SampleRest.war檔案設定tomcat。部署在Tomcat中的war檔案具有返回申請人信用分數的代碼。信用分數是200到800之間的隨機數

* [ 使用套件管理AEM器將資產匯入](assets/aem65-loanapplication.zip)
* 此套件包含下列項目：

   * 使用FDM步驟的工作流模型。
   * 用於FDM步驟的表單資料模型。
   * 最適化表單，可在提交時觸發工作流程。
* 開啟[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填寫詳細資訊並送出。 在提交表單時，會觸發[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![ 工作流程 ](assets/invokefdm651.PNG).
如果信用分數超過500，工作流程會利用「或分割」元件將應用程式傳送給管理員。 如果信用分數少於500，則會將申請路由至付費。
