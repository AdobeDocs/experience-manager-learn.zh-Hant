---
title: 觸發AEMHTM5表單提交上的工作流 — 使用案例
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 以離線模式繼續填寫移動表單並提交移動表單以觸發工AEM作流
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# 使此使用案例在您的系統上工作

>[!NOTE]
>
>對於要在系統上工作的示例資產，假定AEM Author和Publish實例分別運行在埠4502和4503上。 還假定AEM作者可通過 `admin`/`admin`。 如果更改了埠號或管理員密碼，則這些示例資產將無法工作。 您必須使用提供的示例代碼建立自己的資產。

要使此使用案例在本地系統上工作，請執行以下步驟：

* 在埠4502上安裝AEM Author實例，在埠4503上安裝AEM Publish實例
* [按照在AEM Forms與服務用戶一起開發時指定的說明操作](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)。 請確保建立服務用戶並在您的AEM作者和發佈實例上部署捆綁包。
* [開啟osgi配置 ](http://localhost:4503/system/console/configMgr)。
* 搜索  **Apache Sling引用篩選器**。 確保選中「允許空」複選框。
* [部署自定義AEMFormDocumentService包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).此捆綁包需要部署在AEM發佈實例上。 此捆綁包具有從移動表單生成互動式PDF的代碼。
* [下載並解壓縮與本文相關的資產。](assets/offline-pdf-submission-assets.zip) 您將獲得以下
   * **離線提交 — profile.zip**  — 此包AEM含自定義配置檔案，允許您將互動式pdf下載到本地檔案系統。 在AEM發佈實例上部署此包。
   * **xdp-form-and-workflow-zip**  — 此包包AEM含在節點內容/pdf提交上配置的XDP、示例工作流、啟動程式。 在您的AEM作者和發佈實例上部署此包。
   * **HandlePDFubmission.HandlePDFubmission.core-1.0-SNAPSHOT.jar**  — 這是AEM完成大部分工作的捆綁。 此捆綁包包含裝在 `/bin/startworkflow`。 此servlet將提交的表單資料保存在 `/content/pdfsubmissions` 儲存庫中AEM的節點。 在AEM作者和發佈實例上部署此捆綁包。
* [預覽移動表單](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 填入幾個欄位，然後按一下工具欄上的按鈕以下載互動式PDF。
* 使用Acrobat填寫下載的PDF並按一下「提交」按鈕。
* 您應收到成功消息
* 以管理員身份登錄到AEM Author實例
* [檢查收件箱AEM](http://localhost:4502/aem/inbox)
* 您應該讓工作項目複查已提交的PDF

>[!NOTE]
>
>有些客戶沒有將PDF提交到發佈實例上運行的Servlet，而是在Servlet容器（如Tomcat）中部署了Servlet。 這完全取決於客戶熟悉的拓撲。為了在本教程中的目的，我們將使用發佈實例上部署的servlet來處理pdf提交。
