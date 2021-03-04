---
title: 在工作流中使用表單資料模型服務
seo-title: 在工作流中使用表單資料模型服務
description: 從AEM Forms6.4開始，我們現在可以在工作流程中使用表單資料AEM模型。 以下視訊逐步說明在「工作流程」中設定「表單資料模型」步驟所需AEM的步驟。
seo-description: 從AEM Forms6.4開始，我們現在可以在工作流程中使用表單資料AEM模型。 以下視訊逐步說明在「工作流程」中設定「表單資料模型」步驟所需AEM的步驟。
uuid: ecd5d5aa-01eb-48fb-872f-66c656ae14df.
feature: 工作流程
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: c442f439-1e5d-4f96-85df-b818c28389ff
topic: 開發
role: 開發人員
level: 中級
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 1%

---


# 在工作流{#using-form-data-model-service-as-step-in-workflow}中使用表單資料模型服務作為步驟

從AEM Forms6.4開始，我們現在可以在工作流程中使用表單資料AEM模型。 以下視訊將逐步介紹在「工作流程」中設定「表單資料模型」步驟所需AEM的步驟


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

要在伺服器上測試此功能，請遵循以下說明
* [下載並部署setvalue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。這是自訂OSGI套件，可設定中繼資料屬性。
>!![NOTE]在AEM Forms6.5和更高版本中，此功能現已推出，如 [下所述](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 如[此處](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)所述，使用SampleRest.war檔案設定tomcat。部署在Tomcat中的war檔案具有返回申請人信用分數的代碼。 信用分數是200到800之間的隨機數

* [使用套件管理AEM器將資產匯入。套件包含下列項目：](assets/invoke-fdm-as-service-step.zip)

   * 使用FDM步驟的工作流模型。
   * 用於FDM步驟的表單資料模型。
   * 最適化表單，可在提交時觸發工作流程。
* 開啟[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填寫詳細資訊並送出。 在提交表單時，會觸發[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![ 工作流程 ](assets/fdm-as-service-step-workflow.PNG).
如果信用分數超過500，工作流程會利用「或分割」元件將應用程式傳送給管理員。 如果信用分數少於500，則會將申請路由至
