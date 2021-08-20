---
title: 在工作流程中使用表單資料模型服務
description: 從AEM Forms 6.4開始，現在起，我們可以在AEM工作流程中使用表單資料模型。 以下影片會逐步說明在AEM工作流程中設定「表單資料模型」步驟所需的步驟。
feature: 工作流程
type: Tutorial
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# 在工作流程中使用表單資料模型服務 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms 6.4開始，現在起，我們可以在AEM工作流程中使用表單資料模型。 以下影片逐步說明在AEM工作流程中設定「表單資料模型」步驟所需的步驟


>[!VIDEO](https://video.tv.adobe.com/v/21719/?quality=9&learn=on)

若要在您的伺服器上測試此功能，請依照下列指示操作
* [下載並部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。這是設定中繼資料屬性的自訂OSGI套件組合。
>!![NOTE]在AEM Forms 6.5及以上版本中，此功能可立即使用，如 [下所述](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 按照[此處](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)的說明，使用SampleRest.war檔案設定tomcat。部署在Tomcat中的war檔案具有返回申請人信用分數的代碼。 評分是200到800之間的隨機數字

* [使用套件管理程式將資產匯入AEM](assets/invoke-fdm-as-service-step.zip)。套件包含下列項目：

   * 使用FDM步驟的工作流模型。
   * 用於FDM步驟的表單資料模型。
   * 適用性表單，可在提交時觸發工作流程。
* 開啟[MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填寫詳細資訊並提交。 在表單提交上，會觸發[loanapplication workflow](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html)。

![ 工作流程 ](assets/fdm-as-service-step-workflow.PNG).
如果信用分數超過500分，工作流程會使用「或分割」元件將應用程式傳送至管理員。 如果信用積分小於500，系統會將申請轉送至
