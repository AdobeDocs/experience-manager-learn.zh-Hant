---
title: 在最適化Forms Workflow中擷取工作流程註解
seo-title: 在最適化Forms Workflow中擷取工作流程註解
description: 在工作流程中擷取工作流程AEM註解
seo-description: 在工作流程中擷取工作流程AEM註解
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: Workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# 在最適化Forms Workflow中捕獲工作流注釋{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[僅適用於AEM Forms6.4。在AEM Forms6.5中，請使用變數功能來達成此使用案例]

常見的請求是將任務審閱者輸入的注釋包含在電子郵件中。 在AEM Forms6.4中，沒有現成的機制可捕獲用戶輸入的注釋並將這些注釋包含在電子郵件中。

為符合此要求，提供範例OSGi套件，可用來擷取注釋並將這些注釋儲存為工作流程中繼資料屬性。

下列螢幕擷取畫面顯示如何使用[AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)中的處理步驟來擷取注釋，並將其儲存為中繼資料屬性。 &quot;Capture Workflow Comments&quot;是流程步驟中需要使用的java類的名稱。 您必須傳遞包含注釋的中繼資料屬性名稱。 在下方的螢幕擷取中，managerComments是儲存注釋的中繼資料屬性。

![workflowcomments1](assets/workflowcomments1.gif)

要在系統上測試此功能，請遵循以下步驟：
* [確保將工作流中的流程步驟配置為使用「捕獲工作流注釋」](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [部署使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。此套件包含范常式式碼，可擷取注釋並將其儲存為中繼資料屬性

* [將與本文相關的資產下載並解壓縮至您的檔案系統](assets/capturecomments.zip) 資產包含工作流程模型和範例最適化表單。

* 使用套件管理器將2個ZipAEM檔案匯入

* [瀏覽至此URL以預覽表格](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填寫表格欄位並提交表格

* [檢查收件AEM匣](http://localhost:4502/aem/inbox)

* 從收件箱中開啟任務並提交表單。 請在出現提示時輸入一些注釋。

注釋將儲存在crx中名為managerComments的中繼資料屬性中。 若要以管理員身分，檢查注釋登入crx。 工作流實例儲存在以下路徑中

/var/workflow/instances/server0

選擇適當的工作流實例，並在元資料節點中檢查屬性managerComments。

