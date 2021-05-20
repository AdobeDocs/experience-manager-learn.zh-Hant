---
title: 在最適化Forms Workflow中擷取工作流程註解
seo-title: 在最適化Forms Workflow中擷取工作流程註解
description: 在AEM Workflow中擷取工作流程註解
seo-description: 在AEM Workflow中擷取工作流程註解
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: 工作流程
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---


# 在適用性Forms Workflow中擷取工作流程備注{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[僅適用於AEM Forms 6.4。在AEM Forms 6.5中，請使用變數功能來達成此使用案例]

常見的請求是將任務審閱者輸入的注釋包含在電子郵件中。 在AEM Forms 6.4中，沒有立即可用的機制可擷取使用者輸入的留言，並將這些留言納入電子郵件中。

為滿足此需求，提供了範例OSGi套件組合，可用來擷取註解，並將這些註解儲存為工作流程中繼資料屬性。

以下螢幕截圖顯示如何使用[AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)中的進程步驟來捕獲注釋，並將其作為元資料屬性儲存。 「捕獲工作流注釋」是需要在流程步驟中使用的java類的名稱。 您必須傳遞包含註解的中繼資料屬性名稱。 在下面的螢幕截圖中，managerComments是將儲存注釋的元資料屬性。

![workflowcomments1](assets/workflowcomments1.gif)

要在您的系統上測試此功能，請執行以下步驟：
* [確保將工作流中的進程步驟配置為使用捕獲工作流注釋](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [部署Developmentwithserviceuser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。此套件包含擷取註解的范常式式碼，並將其儲存為中繼資料屬性

* [將與本文相關的資產下載並解壓縮至您的檔案系統。資產包](assets/capturecomments.zip) 含工作流程模型和範例適用性表單。

* 使用封裝管理程式將2個zip檔案匯入AEM

* [瀏覽至此URL以預覽表單](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填寫表單欄位並提交表單

* [檢查AEM收件匣](http://localhost:4502/aem/inbox)

* 從收件箱中開啟任務並提交表單。 請在出現提示時輸入一些注釋。

註解會儲存在crx中名為managerComments的中繼資料屬性中。 若要以管理員身分檢查註解登入crx。 工作流實例儲存在以下路徑中

/var/workflow/instances/server0

選取適當的工作流程例項，並在中繼資料節點中檢查屬性managerComments。

