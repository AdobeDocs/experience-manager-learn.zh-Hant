---
title: 在HTM5表單提交上觸發AEM工作流程
seo-title: 在HTML5表單提交上觸發AEM工作流程
description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---


# 讓此使用案例在您的系統上運作

>[!NOTE]
>
>若要讓範例資產在您的系統上運作，假設您的AEM製作和發佈執行個體分別在連接埠4502和4503上執行。 此外也假設AEM作者可透過`admin`/`admin`存取。 如果連接埠號或管理員密碼已變更，這些範例資產將無法運作。 您必須使用提供的范常式式碼建立自己的資產。

若要讓此使用案例在您的本機系統上運作，請執行下列步驟：

* 在連接埠4502上安裝AEM製作執行個體，在連接埠4503上安裝AEM發佈執行個體
* [遵循在AEM Forms中與服務使用者一起開發中指定的指示](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)。請務必建立服務使用者，並在您的AEM製作和發佈執行個體上部署套件組合。
* [開啟osgi設 ](http://localhost:4503/system/console/configMgr)定。
* 搜尋&#x200B;**Apache Sling Referrer Filter**。 確定已選取「允許空白」核取方塊。
* [部署自訂AEMFormDocumentService套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。此套件組合需要部署在您的AEM Publish執行個體上。此套件包含從行動表單產生互動式PDF的程式碼。
* [下載並解壓縮與本文相關的資產。](assets/offline-pdf-submission-assets.zip) 您會獲得下列內容
   * **offline-submission-profile.zip**  — 此AEM套件包含可讓您將互動式pdf下載至本機檔案系統的自訂設定檔。將此套件部署在您的AEM Publish執行個體上。
   * **xdp-form-and-workflow.zip**  — 此AEM套件包含XDP、範例工作流程，以及在節點內容/pdfsubmissions上設定的啟動器。在您的AEM製作和發佈執行個體上部署此套件。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  — 這是執行大部分工作的AEM套件組合。此捆綁包包含裝載在`/bin/startworkflow`上的servlet。 此Servlet會將提交的表單資料儲存在AEM存放庫的`/content/pdfsubmissions`節點下。 在您的AEM製作和發佈執行個體上部署此套件組合。
* [預覽行動表單](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 填寫數個欄位，然後按一下工具列上的按鈕以下載互動式PDF。
* 使用Acrobat填入下載的PDF，然後按一下提交按鈕。
* 您應該會收到成功訊息
* 以管理員身分登入AEM Author例項
* [檢查AEM收件匣](http://localhost:4502/aem/inbox)
* 您應該有工作項目可審核提交的PDF

>[!NOTE]
>
>有些客戶並未將PDF提交至發佈執行個體上執行的servlet，而是在Servlet容器（例如Tomcat）中部署了servlet。 這完全取決於客戶熟悉的拓撲。在本教學課程中，我們將使用發佈執行個體上部署的servlet來處理pdf提交內容。

