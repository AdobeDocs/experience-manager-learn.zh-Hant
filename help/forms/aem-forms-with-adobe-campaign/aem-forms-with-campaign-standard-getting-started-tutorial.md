---
title: 開始使用AEM Forms和Adobe Campaign Standard
seo-title: 開始使用AEM Forms和Adobe Campaign Standard
description: 使用AEM Forms表單資料模型將AEM Forms與Adobe Campaign Standard整合，以擷取ACS促銷活動描述檔資訊等。
seo-description: 使用AEM Forms表單資料模型將AEM Forms與Adobe Campaign Standard整合，以擷取ACS促銷活動描述檔資訊等。
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: 「適應性Forms，表單資料模型」
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 0%

---


# 開始使用AEM Forms和Adobe Campaign Standard{#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formandcampaign](assets/helpx-cards-forms.png)

本教學課程將列出將AEM Forms與Adobe Campaign Standard(ACS)整合的各種使用案例。

ACS有一套豐富的API，可讓ACS與我們選擇的技術相互連接。 在本教程中，我們將重點介紹AEM Forms與ACS的介面。

要將AEM Forms與ACS整合，您需要執行以下步驟：

* [在ACS實例上設定API訪問。](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* 建立JSON網頁Token。
* 將JSON網路Token與AdobeIdentity Management服務交換為存取Token。
* 在授權HTTP標題中加入此存取Token，並在對ACS例項的每個要求中加入X-API-Key。

若要開始使用，請依照下列指示進行

* [下載並解壓縮與本教學課程相關的資產。](assets/aem-forms-and-acs-bundles.zip)
* 使用[Felix網頁主控台](http://localhost:4502/system/console/bundles)部署套件
* 在Felix OSGI Configuration中提供適當的Adobe Campaign設定。
* [如本文所述建立服務使用者](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。請確定部署與文章相關聯的OSGi套件。
* 將ACS私鑰儲存在etc/key/campaign/private.key中。 您必須在etc/key下建立名為campaign的資料夾。
* [提供對服務使用者「資料」之促銷活動資料夾的讀取存取權。](http://localhost:4502/useradmin)
