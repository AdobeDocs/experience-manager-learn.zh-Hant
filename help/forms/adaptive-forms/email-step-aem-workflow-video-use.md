---
title: 使用發送電子郵件步驟Forms Workflow
description: 在AEM Forms6.4中引入了「發送電子郵件」步驟。使用此步驟，我們可以構建業務流程或工作流，以便您發送帶有或不帶附件的電子郵件。 以下視頻介紹了配置發送電子郵件元件的步驟
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# 使用發送電子郵件步驟Forms Workflow {#using-send-email-step-of-forms-workflow}

在AEM Forms6.4中引入了「發送電子郵件」步驟。使用此步驟，我們可以構建業務流程或工作流，以便您發送帶有或不帶附件的電子郵件。 以下視頻將介紹配置發送電子郵件元件的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

作為本文的一部分，我們將介紹以下使用案例：

1. 用戶填寫「暫停請求」表單
1. 在提交表單時，AEM將觸發工作流
1. 工作AEM流利用「發送電子郵件」元件以DoR作為附件發送電子郵件

在使用「發送電子郵件」步驟之前，請確保從 [configMgr](http://localhost:4502/system/console/configMgr)。 提供特定於您的環境的值

![配置日CQ郵件服務](assets/mailservice.png)

作為與本文關聯的資產的一部分，您將獲得以下

1. 在提交時觸發工作流的自適應表單
1. 將以DOR作為附件發送電子郵件的示例工作流
1. 建立元資料屬性的OSGi捆綁

要獲取在系統上運行的示例，請執行以下操作：

1. [部署Developingwithserviceuser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下載和安裝setvalue包](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)此捆綁包包含用於建立元資料屬性的代碼，作為工作流進程步驟的一部分。
1. [配置日CQ郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [使用包管理器將與本文關聯的資產導入並安裝到CRX](assets/emaildoraemformskt.zip)
1. 啟動 [自適應形式](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。 填寫必填欄位並提交。
1. 您應收到一封DocumentOfRecord作為附件的電子郵件

瀏覽 [工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

查看工作流的流程步驟。 與流程步驟關聯的自定義代碼將建立元資料屬性名稱，並根據提交的資料設定其值。這些值隨後由發送電子郵件元件使用。

>[!NOTE]
>
>在AEM Forms6.5及更高版本中，您不需要此自定義代碼來建立元資料屬性。 請在工作流中使用變數AEM功能

確保根據下面的螢幕抓圖配置「發送電子郵件」元件的「附件」頁籤
![「發送電子郵件附件」頁籤](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」值必須與在自適應表單的提交選項中指定的「記錄路徑文檔」中指定的值匹配。
