---
title: 在AEM 6.5工作流程中使用表單資料模型服務做為步驟
seo-title: 在AEM 6.5工作流程中使用表單資料模型服務做為步驟
description: AEM Forms 6.5引入了在AEM工作流程中建立變數的功能。 有了這項新功能，在AEM Workflow中使用「叫用表單資料模型服務」變得十分簡單。 以下影片將引導您瞭解在AEM工作流程中使用「叫用表單資料模型服務」所涉及的步驟。
seo-description: AEM Forms 6.5引入了在AEM工作流程中建立變數的功能。 有了這項新功能，在AEM Workflow中使用「叫用表單資料模型服務」變得十分簡單。 以下影片將引導您瞭解在AEM工作流程中使用「叫用表單資料模型服務」所涉及的步驟。
feature: workflow.
topics: workflow.
audience: developer.
doc-type: technical video.
activity: setup.
version: 6.5.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---


# 在AEM 6.5工作流程中使用表單資料模型服務做為步驟 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms 6.4開始，我們現在可以將表單資料模型服務當做AEM工作流程的一部分。 以下視訊逐步說明在AEM工作流程中設定「表單資料模型」步驟所需的步驟

>!![NOTE]此視訊中展示的功能需要AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

要在伺服器上測試此功能，請遵循以下說明

* 如下所述，使用SampleRest.war檔案設定 [tomcat](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)。Tomcat中部署的war檔案具有返回申請人信用分數的代碼。信用分數是200到800之間的隨機數

* [ 使用套件管理員將資產匯入AEM](assets/aem65-loanapplication.zip)
* 此套件包含下列項目：

   * 使用FDM步驟的工作流模型。
   * 用於FDM步驟的表單資料模型。
   * 最適化表單，可在提交時觸發工作流程。
* 開啟MortgageApplicationForm [](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填寫詳細資訊並送出。 在提交表單時，會觸 [發出借申請的](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 工作流程。

![ 工作流程 ](assets/invokefdm651.PNG).
如果信用分數超過500，工作流程會利用「或分割」元件將應用程式傳送給管理員。 如果信用分數少於500，則會將申請路由至付費。
