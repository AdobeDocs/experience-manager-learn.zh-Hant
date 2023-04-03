---
title: 在工作流程中使用表單資料模型服務
description: 從AEM Forms 6.4開始，現在起，我們可以在AEM工作流程中使用表單資料模型。 以下影片會逐步說明在AEM工作流程中設定「表單資料模型」步驟所需的步驟。
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# 在工作流程中使用表單資料模型服務 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms 6.4開始，現在起，我們可以在AEM工作流程中使用表單資料模型。 以下影片逐步說明在AEM工作流程中設定「表單資料模型」步驟所需的步驟


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

若要在您的伺服器上測試此功能，請依照下列指示操作
* [下載並部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 這是設定中繼資料屬性的自訂OSGI套件組合。
>!![NOTE]在AEM Forms 6.5及更新版本中，此功能可立即使用，如 [此處描述](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 使用SampleRest.war檔案設定tomcat，如所述 [此處](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).Tomcat中部署的戰爭檔案具有返回申請人信用評分的代碼。 評分是200到800之間的隨機數字

* [使用套件管理器將資產匯入AEM](assets/invoke-fdm-as-service-step.zip).該包包含以下內容：

   * 使用FDM步驟的工作流模型。
   * 用於FDM步驟的表單資料模型。
   * 適用性表單，可在提交時觸發工作流程。
* 開啟 [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 填寫詳細資訊並提交。 在提交的表格上， [借出應用程式工作流程](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 已觸發。

![ 工作流程 ](assets/fdm-as-service-step-workflow.PNG).
如果信用分數超過500分，工作流程會使用「或分割」元件將應用程式傳送至管理員。 如果信用積分小於500，系統會將申請轉送至
