---
title: AEM Forms和Adobe Campaign Standard快速入門
seo-title: AEM Forms和Adobe Campaign Standard快速入門
description: 使用AEM Forms Data Model將AEM Forms與Adobe Campaign Standard整合，以擷取ACS促銷活動設定檔資訊等。
seo-description: 使用AEM Forms Data Model將AEM Forms與Adobe Campaign Standard整合，以擷取ACS促銷活動設定檔資訊等。
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
translation-type: tm+mt
source-git-commit: 3b44a9e2341ce23f737e6ef75c67fadd9870d2ac
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# AEM Forms和Adobe Campaign Standard快速入門{#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formandcampaign](assets/helpx-cards-forms.png)

本教學課程將列出整合AEM Forms與Adobe Campaign Standard(ACS)的各種使用案例。

ACS有一套豐富的API，可讓ACS與我們選擇的技術相互連接。 在本教學課程中，我們將著重於將AEM Forms與ACS連接。

若要將AEM Forms與ACS整合，您必須遵循下列步驟：

* [在ACS實例上設定API訪問。](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* 建立JSON網頁Token。
* 將JSON網頁Token與Adobe Identity Management Service交換為存取Token。
* 在授權HTTP標題中加入此存取Token，並在對ACS例項的每個要求中加入X-API-Key。

若要開始使用，請依照下列指示進行

* [下載並解壓縮與本教學課程相關的資產。](assets/aem-forms-and-acs-bundles.zip)
* 使用[Felix網頁主控台](http://localhost:4502/system/console/bundles)部署套件
* 在「Felix OSGI設定」中提供適當的Adobe Campaign設定。
* [如本文所述建立服務使用者](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。請確定部署與文章相關聯的OSGi套件。
* 將ACS私鑰儲存在etc/key/campaign/private.key中。 您必須在etc/key下建立名為campaign的資料夾。
* [提供對服務使用者「資料」之促銷活動資料夾的讀取存取權。](http://localhost:4502/useradmin)
