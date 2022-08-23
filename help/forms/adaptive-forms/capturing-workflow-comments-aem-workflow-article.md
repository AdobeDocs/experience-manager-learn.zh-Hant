---
title: 在自適應Forms Workflow中捕獲工作流注釋
description: 在工作流中捕獲工作流AEM注釋
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# 在自適應Forms Workflow中捕獲工作流注釋{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[僅適用於AEM Forms6.4。在AEM Forms6.5中，請使用變數功能實現此使用情形]

常見請求是將任務審閱者輸入的注釋包含在電子郵件中。 在AEM Forms6.4中，沒有現成的機制來捕獲用戶輸入的注釋並將這些注釋包含在電子郵件中。

為滿足此要求，提供了一個示例OSGi捆綁包，可用於捕獲注釋並將這些注釋儲存為工作流元資料屬性。

以下螢幕快照顯示了如何在 [工AEM作流](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) 以捕獲注釋並將其儲存為元資料屬性。 「捕獲工作流注釋」是需要在進程步驟中使用的java類的名稱。 您需要傳遞將保存注釋的元資料屬性名稱。 在下面的螢幕快照中，managerComments是用於儲存注釋的元資料屬性。

![工作流注釋1](assets/workflowcomments1.gif)

要在系統上test此功能，請執行以下步驟：
* [確保將工作流中的流程步驟配置為使用「捕獲工作流注釋」](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [部署Developingwithserviceuser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 此捆綁包包含用於捕獲注釋並將其儲存為元資料屬性的示例代碼

* [將與本文相關的資產下載並解壓縮到您的檔案系統](assets/capturecomments.zip) 這些資產包含工作流模型和示例自適應表單。

* 使用包管理器將2個ZIPAEM檔案導入

* [通過瀏覽到此URL預覽表單](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填寫表單域並提交表單

* [檢查收件箱AEM](http://localhost:4502/aem/inbox)

* 從收件箱開啟任務並提交表單。 請在出現提示時輸入一些注釋。

注釋將儲存在元資料屬性crx中名為managerComments。 檢查注釋以管理員身份登錄到crx。 工作流實例儲存在以下路徑中

/var/workflow/instances/server0

選擇相應的工作流實例並檢查元資料節點中的屬性managerComments。
