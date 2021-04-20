---
title: 使用傳送電子郵件步驟進行Forms Workflow
seo-title: 使用傳送電子郵件步驟進行Forms Workflow
description: 「傳送電子郵件」步驟已於AEM Forms6.4推出。使用此步驟，我們可以建立商業程式或工作流程，讓您傳送包含或不含附件的電子郵件。 以下視訊將逐步介紹設定傳送電子郵件元件的步驟
seo-description: 「傳送電子郵件」步驟已於AEM Forms6.4推出。使用此步驟，我們可以建立商業程式或工作流程，讓您傳送包含或不含附件的電子郵件。 以下視訊將逐步介紹設定傳送電子郵件元件的步驟
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: Workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---


# 使用Forms Workflow{#using-send-email-step-of-forms-workflow}的「發送電子郵件」步驟

「傳送電子郵件」步驟已於AEM Forms6.4推出。使用此步驟，我們可以建立商業程式或工作流程，讓您傳送包含或不含附件的電子郵件。 以下視訊將逐步介紹設定傳送電子郵件元件的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

在本文中，我們將引導您檢視下列使用案例：

1. 使用者填寫「請求時間表」
1. 提交表單時，AEM會觸發工作流程
1. 「工AEM作流程」利用「發送電子郵件」元件以DoR作為附件發送電子郵件

在使用「傳送電子郵件」步驟之前，請務必從[configMgr](http://localhost:4502/system/console/configMgr)配置Day CQ Mail服務。 提供您環境的特定值

![配置日CQ郵件服務](assets/mailservice.png)

作為與本文相關的資產的一部分，您將會取得下列

1. 最適化表單，會在提交時觸發工作流程
1. 以DOR為附件傳送電子郵件的範例工作流程
1. 建立中繼資料屬性的OSGi套件

要在系統上運行示例，請執行以下操作：

1. [部署使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下載和安裝setvalue包此](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)包包含用於在工作流的流程步驟中建立元資料屬性的代碼。
1. [配置日CQ郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [使用套件管理員將與本文相關的資產匯入並安裝至CRX](assets/emaildoraemformskt.zip)
1. 啟動[adaptive form](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。 填寫必填欄位並送出。
1. 您應該會收到電子郵件，其中DocumentOfRecord為附件

探索[工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

檢視工作流程的流程步驟。 與流程步驟關聯的自定義代碼將建立元資料屬性名稱，並從提交的資料中設定其值。然後，發送電子郵件元件將使用這些值。

>[!NOTE]
>
>在AEM Forms6.5和更高版本中，您不需要此自訂程式碼來建立中繼資料屬性。 請使用「工作流程」中的變AEM數功能

請確定「傳送電子郵件」元件的「附件」標籤已依下方螢幕擷取畫面設定
![「傳送電子郵件附件」標籤](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」值必須符合在最適化表單的提交選項中指定的「記錄檔案路徑」中指定的值。

