---
title: HTM5表AEM單提交的觸發工作流程
seo-title: HTML5表AEM單提交的觸發工作流程
description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
seo-description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---


# 讓此使用案例在您的系統上運作

>[!NOTE]
>
>對於要在系統上運作的範例資產，假設您的AEM Author和Publish執行個體分別在埠4502和4503上執行。 您也假設AEM作者可透過`admin`/`admin`存取。 如果埠號或管理員密碼已變更，則這些範例資產將無法運作。 您必須使用提供的范常式式碼建立自己的資產。

要使此使用案例在本地系統上工作，請執行以下步驟：

* 在埠4502上安裝AEM Author例項，在埠4503上安裝AEM Publish例項
* [請依照AEM Forms與服務使用者一起開發時的指示](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)。請務必建立服務使用者，並在您的AEM Author和Publish執行個體上部署套件。
* [開啟osgi設定 ](http://localhost:4503/system/console/configMgr)。
* 搜尋&#x200B;**Apache Sling Referrer Filter**。 請確定已選中「允許空白」複選框。
* [部署自訂AEMFormDocumentService Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。此套件必須部署在您的AEM Publish執行個體上。此套件包含從行動表單產生互動式PDF的程式碼。
* [下載並解壓縮與本文相關的資產。](assets/offline-pdf-submission-assets.zip) 您將取得下列
   * **offline-submission-profile.zip**  —— 此套AEM件包含自訂描述檔，可讓您將互動式pdf下載至本機檔案系統。將此套件部署在您的AEM Publish實例上。
   * **xdp-form-and-workflow.zip** -此套件包AEM含XDP、範例工作流程、節點內容/pdf提交上設定的啟動程式。將此套件部署在您的AEM Author和Publish執行個體上。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** -這是AEM執行大部分工作的套件。此捆綁包包含裝載在`/bin/startworkflow`上的servlet。 此servlet將提交的表單資料保存在儲存庫的`/content/pdfsubmissions`節AEM點下。 在您的AEM Author和Publish執行個體上部署此套件。
* [預覽行動表單](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 填寫數個欄位，然後按一下工具列上的按鈕以下載互動式PDF。
* 使用Acrobat填寫下載的PDF，然後按一下提交按鈕。
* 您應該收到成功訊息
* 以管理員身分登入AEM Author例項
* [檢查收件AEM匣](http://localhost:4502/aem/inbox)
* 您應該有工作項目來審核提交的PDF

>[!NOTE]
>
>有些客戶並未將PDF送出至在發佈例項上執行的servlet，而是將servlet部署在servlet容器（例如Tomcat）中。 這一切取決於客戶所熟悉的拓撲。在本教程中，我們將使用部署在發佈實例上的servlet來處理pdf提交。

