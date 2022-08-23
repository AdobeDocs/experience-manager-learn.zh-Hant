---
title: 在6.5工作流中將表單資料模型服務AEM用作步驟
description: AEM Forms6.5引入了在工作流中建立變數的AEM能力。 使用工作流中的「調用表單資料模型服務」這一新功AEM能變得非常容易。 以下視頻將引導您完成在工作流中使用調用表單資料模型服務所涉AEM及的步驟。
feature: Workflow
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1f13d82e-c1d0-4c8c-8468-b4a4c5897c71
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# 在6.5工作流中將表單資料模型服務AEM用作步驟 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms6.4開始，我們現在能夠將表單資料模型服務用作工作流的一AEM部分。 以下視頻將介紹在工作流中配置表單資料模型步驟所需的AEM步驟

>!![NOTE]此視頻中演示的功能需要AEM Forms6.5.1


>[!VIDEO](https://video.tv.adobe.com/v/28145?quality=9&learn=on)

要在伺服器上test此功能，請遵循以下說明

* 使用SampleRest.war檔案設定tomcat（如所述） [這裡](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html).Tomcat中部署的戰爭檔案具有返回申請人信用評分的代碼。信用評分是200到800之間的隨機數

* [ 使用包管理器將AEM資產導入](assets/aem65-loanapplication.zip)
* 該包包含以下內容：

   * 使用FDM步驟的工作流模型。
   * 在FDM步驟中使用的窗體資料模型。
   * 用於在提交時觸發工作流的自適應表單。
* 開啟 [抵押申請表](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled)。 填寫詳細資訊並提交。 在表格上 [Loan Application Work（外部應用程式工作流）](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 按鈕。

![ 工作流程 ](assets/invokefdm651.PNG).
如果信用分數超過500，則工作流將使用或拆分元件將應用程式路由到管理員。 如果信用評分小於500，則將應用程式路由到cavery。
