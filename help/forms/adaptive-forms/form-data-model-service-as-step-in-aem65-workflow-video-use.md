---
title: 在AEM 6.5工作流程中使用表單資料模型服務作為步驟
description: AEM Forms 6.5導入了在AEM工作流程中建立變數的功能。 有了這項新功能，在AEM Workflow中使用「叫用表單資料模型服務」變得非常輕鬆。 以下影片將逐步引導您完成在AEM工作流程中使用叫用表單資料模型服務的步驟。
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
last-substantial-update: 2021-02-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# 在AEM 6.5工作流程中使用表單資料模型服務作為步驟 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms 6.4開始，現在起，我們可以在AEM工作流程中使用表單資料模型服務。 以下影片逐步說明在AEM工作流程中設定「表單資料模型」步驟所需的步驟

>!![NOTE]此影片中展示的功能需有AEM Forms 6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=12&learn=on)

若要在您的伺服器上測試此功能，請依照下列指示操作

* 使用SampleRest.war檔案設定tomcat，如所述 [此處](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Tomcat中部署的戰爭檔案具有返回申請人信用評分的代碼。信用評分是200到800之間的隨機數

* [ 使用套件管理器將資產匯入AEM](assets/aem65-loanapplication.zip)
* 套件包含下列項目：

   * 使用FDM步驟的工作流模型。
   * 用於FDM步驟的表單資料模型。
   * 適用性表單，可在提交時觸發工作流程。
* 開啟 [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 填寫詳細資訊並提交。 在提交的表格上， [借出應用程式工作流程](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 已觸發。

![ 工作流程 ](assets/invokefdm651.PNG).
如果信用分數超過500分，工作流程會使用「或分割」元件將應用程式傳送至管理員。 如果信用積分小於500，系統會將申請轉送至收件者。
