---
title: 在工作流程中使用表單資料模型服務作為步驟
description: 從AEM Forms 6.4開始，我們現在能使用表單資料模型做為AEM Workflow的一部分。 以下影片會逐步介紹在AEM Workflow中設定表單資料模型步驟所需的步驟。
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
duration: 220
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# 在工作流程中使用表單資料模型服務作為步驟 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms 6.4開始，我們現在能使用表單資料模型做為AEM Workflow的一部分。 以下影片會逐步介紹在AEM Workflow中設定表單資料模型步驟所需的步驟


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

若要在您的伺服器上測試此功能，請遵循下列指示
* [下載並部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 這是設定中繼資料屬性的自訂OSGI套件組合。
>在AEM Forms 6.5及更高版本中，此功能可立即用於 [請在此說明](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 使用SampleRest.war檔案設定tomcat，如所述 [此處](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).在Tomcat中部署的war檔案具有傳回申請人信用評分的代碼。 信用分數是介於200到800之間的隨機數字

* [使用封裝管理員將資產匯入AEM](assets/invoke-fdm-as-service-step.zip)此套件包含下列專案：

   * 使用FDM步驟的工作流程模型。
   * 用於FDM步驟的表單資料模型。
   * 最適化表單在提交時觸發工作流程。
* 開啟 [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 填寫詳細資料並提交。 在提交表單時 [借出應用程式工作流程](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 已觸發。

![ 工作流程 ](assets/fdm-as-service-step-workflow.PNG).
如果信用分數超過500，工作流程會利用「或分割」元件，將應用程式路由給管理員。 如果信用分數小於500，則會將應用程式傳送至cavery
