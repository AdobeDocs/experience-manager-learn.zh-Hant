---
title: 在HTM5表單提交時觸發AEM工作流程 — 讓使用案例發揮作用
description: 以離線模式繼續填寫行動表單並提交行動表單以觸發AEM工作流程
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 90
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---

# 讓此使用案例在您的系統上運作

>[!NOTE]
>
>為了讓範例資產在您的系統上運作，假設您有AEM Author和Publish執行個體分別執行於連線埠4502和4503。 也假設可以透過`admin`/`admin`存取AEM作者。 如果連線埠號碼或管理密碼已變更，這些範例資產將無法運作。 您必須使用提供的範常式式碼建立自己的資產。

若要讓此使用案例在本機系統中正常運作，請遵循下列步驟：

* 在連線埠4502上安裝AEM Author例項，並在連線埠4503上安裝AEM Publish例項
* [請依照在AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)中與服務使用者一起開發中指定的指示進行。 請務必建立服務使用者，並將套件組合部署在您的AEM作者和Publish執行個體上。
* [開啟osgi設定](http://localhost:4503/system/console/configMgr)。
* 搜尋&#x200B;**Apache Sling反向連結篩選器**。 請確定已選取「允許空白」核取方塊。
* [部署自訂AEMFormDocumentService組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。此組合必須部署在您的AEM Publish執行個體上。 此套件組合具有從行動表單產生互動式PDF的程式碼。
* [下載並解壓縮與本文相關的資產。](assets/offline-pdf-submission-assets.zip)您將取得下列專案
   * **offline-submission-profile.zip** — 此AEM套件包含自訂設定檔，可讓您將互動式pdf下載至本機檔案系統。 在您的AEM Publish執行個體上部署此套件。
   * **xdp-form-and-workflow.zip** — 此AEM封裝包含設定在節點content/pdfsubmissions上的XDP、範例工作流程、啟動器。 在您的AEM作者和Publish執行個體上部署此套件。
   * **HandlePDFubmission.HandlePDFubmission.core-1.0-SNAPSHOT.jar** — 這是執行大部分工作的AEM套件組合。 此組合包含掛接在`/bin/startworkflow`上的servlet。 此servlet會將提交的表單資料儲存在AEM存放庫的`/content/pdfsubmissions`節點下。 在您的AEM作者和Publish執行個體上部署此套件組合。
* [預覽行動表單](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 填寫數個欄位，然後按一下工具列上的按鈕以下載互動式PDF。
* 使用Acrobat填寫已下載PDF並按一下提交按鈕。
* 您應該會收到一則成功訊息
* 以管理員身分登入AEM作者執行個體
* [檢查AEM收件匣](http://localhost:4502/aem/inbox)
* 您應該要有工作專案來檢閱已提交的PDF

>[!NOTE]
>
>有些客戶不是將PDF提交到發佈執行個體上執行的servlet，而是將servlet部署在Tomcat之類的servlet容器中。 這完全取決於客戶熟悉的拓撲。在本教學課程中，我們將使用部署在發佈執行個體上的servlet來處理pdf提交。
