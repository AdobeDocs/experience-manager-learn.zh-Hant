---
title: 使用傳送電子郵件步驟進行Forms Workflow
description: AEM Forms 6.4導入了「傳送電子郵件」步驟。使用此步驟，我們可以建立業務流程或工作流程，讓您可以傳送包含或不含附件的電子郵件。 以下影片會逐步說明設定傳送電子郵件元件的步驟
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# 使用傳送電子郵件步驟進行Forms Workflow {#using-send-email-step-of-forms-workflow}

AEM Forms 6.4導入了「傳送電子郵件」步驟。使用此步驟，我們可以建立業務流程或工作流程，讓您可以傳送包含或不含附件的電子郵件。 以下影片會逐步說明設定傳送電子郵件元件的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

在本文中，我們會逐步引導您了解下列使用案例：

1. 用戶填寫請求表
1. 提交表單時會觸發AEM工作流程
1. AEM工作流程利用「傳送電子郵件」元件，以DoR作為附件傳送電子郵件

使用「傳送電子郵件」步驟之前，請務必從 [configMgr](http://localhost:4502/system/console/configMgr). 提供您環境的特定值

![設定Day CQ Mail Service](assets/mailservice.png)

隨著本文相關資產的一部分，您會獲得下列資訊

1. 可在提交時觸發工作流程的適用性表單
1. 以DOR作為附件傳送電子郵件的範例工作流程
1. 建立中繼資料屬性的OSGi套件組合

要使示例在您的系統上運行，請執行以下操作：

1. [部署Developmentwithserviceuser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下載和安裝setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)此套件包含在工作流程的處理步驟中，用於建立中繼資料屬性的程式碼。
1. [設定Day CQ Mail Service](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [使用套件管理器將與本文相關聯的資產匯入並安裝至CRX](assets/emaildoraemformskt.zip)
1. 啟動 [適用性表單](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). 填寫必填欄位並提交。
1. 您應該會收到一封包含DocumentOfRecord作為附件的電子郵件

探索 [工作流模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

檢視工作流程的處理步驟。 與流程步驟相關聯的自定義代碼將建立元資料屬性名稱，並從提交的資料中設定其值。然後，發送電子郵件元件將使用這些值。

>[!NOTE]
>
>在AEM Forms 6.5及更新版本中，您不需要此自訂程式碼來建立中繼資料屬性。 請在AEM Workflow（工作流程）中使用變數功能

根據下方螢幕擷取畫面，確認「傳送電子郵件」元件的「附件」索引標籤已設定
![「發送電子郵件附件」頁簽](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」值必須符合在最適化表單的提交選項中指定的「記錄路徑檔案」中指定的值。
