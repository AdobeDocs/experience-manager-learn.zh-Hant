---
title: 在AEM 6.5工作流程中使用表單資料模型服務的步驟
description: AEM Forms 6.5匯入了在AEM工作流程中建立變數的功能。 透過這項新功能，使用AEM工作流程中的「叫用表單資料模型服務」已變得非常容易。 以下影片將逐步說明在AEM工作流程中使用叫用表單資料模型服務所涉及的步驟。
feature: Workflow
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
duration: 239
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---

# 在AEM 6.5工作流程中使用表單資料模型服務的步驟 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms 6.4開始，我們現在能使用表單資料模型服務做為AEM工作流程的一部分。 以下影片會逐步介紹在AEM工作流程中設定表單資料模型步驟所需的步驟

>此影片中示範的功能需要AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

若要在您的伺服器上測試此功能，請遵循下列指示

* 使用SampleRest.war檔案設定tomcat，如[此處](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)所述。部署在Tomcat中的war檔案具有傳回申請人信用分數的代碼。信用分數是介於200到800之間的隨機數

* [使用封裝管理員將資產匯入AEM](assets/aem65-loanapplication.zip)
* 此套件包含下列專案：

   * 使用FDM步驟的工作流程模型。
   * 用於FDM步驟的表單資料模型。
   * 最適化表單在提交時觸發工作流程。
* 開啟[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填寫詳細資料並提交。 在表單提交時，會觸發[借出應用程式工作流程](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![工作流程](assets/invokefdm651.PNG)。
如果信用分數超過500，工作流程會利用「或分割」元件，將應用程式路由給管理員。 如果信用分數小於500，則會將應用程式傳送至cavery。
