---
title: 以最適化Forms Workflow擷取工作流程註解
description: 在AEM工作流程中擷取工作流程註解
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# 以最適化Forms Workflow擷取工作流程註解{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[僅適用於AEM Forms 6.4。在AEM Forms 6.5中，請使用變數功能來達成此使用案例]

常見的要求是能在電子郵件中加入任務檢閱者輸入的註解。 在AEM Forms 6.4中，沒有現成的機制可擷取使用者輸入的註解，並在電子郵件中包含這些註解。

為了滿足此需求，提供範例OSGi套件，其可用於擷取註解並將這些註解儲存為工作流程中繼資料屬性。

下列熒幕擷圖顯示如何使用[AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)中的程式步驟來擷取註解，並將註解儲存為中繼資料屬性。 「擷取工作流程註解」是需要用於程式步驟中的java類別名稱。 您必須傳遞內含註解的中繼資料屬性名稱。 在下方熒幕擷圖中，managerComments是將儲存註解的中繼資料屬性。

![workflowcomments1](assets/workflowcomments1.gif)

若要在您的系統上測試此功能，請遵循下列步驟：
* [請確定工作流程中的程式步驟設定為使用擷取工作流程註解](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [部署Developingwithserviceuser套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 此套件包含用來擷取註解的範常式式碼，並將其儲存為中繼資料屬性

* [將本文相關的資產下載並解壓縮至您的檔案系統](assets/capturecomments.zip)這些資產包含工作流程模型和範例最適化表單。

* 使用封裝管理員將2個zip檔案匯入AEM

* [瀏覽到此URL以預覽表單](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填寫表單欄位並提交表單

* [檢查您的AEM收件匣](http://localhost:4502/aem/inbox)

* 從收件匣開啟工作並提交表單。 出現提示時，請輸入一些註解。

註解儲存在AEM儲存庫中稱為`managerComments`的中繼資料屬性中。 若要檢查註解，請以管理員身分登入crx。 工作流程例項會儲存在下列路徑中：

`/var/workflow/instances/server0`

選取適當的工作流程例項，並檢查中繼資料節點中的屬性managerComments。
