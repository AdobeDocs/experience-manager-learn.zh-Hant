---
title: 在工作流中使用表單資料模型服務作為步驟
description: 從AEM Forms6.4開始，我們現在能夠將表單資料模型用作工作流的一AEM部分。 以下視頻將介紹在工作流中配置表單資料模型步驟所需AEM的步驟。
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

# 在工作流中使用表單資料模型服務作為步驟 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms6.4開始，我們現在能夠將表單資料模型用作工作流的一AEM部分。 以下視頻將介紹在工作流中配置表單資料模型步驟所需的AEM步驟


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

要在伺服器上test此功能，請遵循以下說明
* [下載並部署setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 這是設定元資料屬性的自定義OSGI捆綁包。
>!![NOTE]在AEM Forms6.5及更高版本中，此功能現成可用， [此處](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 使用SampleRest.war檔案設定tomcat（如所述） [這裡](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).Tomcat中部署的戰爭檔案具有返回申請人信用分數的代碼。 信用評分為200到800之間的隨機數

* [使用包管理器將AEM資產導入](assets/invoke-fdm-as-service-step.zip).該包包含以下內容：

   * 使用FDM步驟的工作流模型。
   * 在FDM步驟中使用的窗體資料模型。
   * 用於在提交時觸發工作流的自適應表單。
* 開啟 [抵押申請表](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填寫詳細資訊並提交。 在表格上 [Loan Application Work（外部應用程式工作流）](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 按鈕。

![ 工作流程 ](assets/fdm-as-service-step-workflow.PNG).
如果信用分數超過500，則工作流將使用或拆分元件將應用程式路由到管理員。 如果信用評分小於500，則將應用程式路由到
