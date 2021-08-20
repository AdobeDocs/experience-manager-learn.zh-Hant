---
title: 開始使用AEM Forms和Adobe Campaign Standard
description: 使用AEM Forms表單資料模型將AEM Forms與Adobe Campaign Standard整合，以擷取ACS促銷活動設定檔資訊等。
feature: 適用性Forms，表單資料模型
version: 6.3,6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# 開始使用AEM Forms和Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

本教學課程將列出整合AEM Forms與Adobe Campaign Standard(ACS)的各種使用案例。

ACS有一套豐富的API，可讓ACS與我們選擇的技術交互。 在本教學課程中，我們將著重於將AEM Forms與ACS結合。

要將AEM Forms與ACS整合，您需要執行以下步驟：

* [在ACS實例上設定API訪問。](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* 建立JSON網頁代號。
* 將JSON網頁代號與AdobeIdentity Management服務交換以取得存取代號。
* 在授權HTTP標題中加入此存取權杖，並在向ACS執行個體發出的每個請求中加入X-API-Key。

若要開始，請遵循下列指示

* [下載並解壓縮本教學課程相關的資產。](assets/aem-forms-and-acs-bundles.zip)
* 使用[Felix Web控制台](http://localhost:4502/system/console/bundles)部署套件組合
* 在Felix OSGI設定中提供Adobe Campaign的適當設定。
* [建立本文所述的服務使用者](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。請務必部署與文章相關聯的OSGi套件組合。
* 將ACS私密金鑰儲存在etc/key/campaign/private.key中。 您必須在etc/key下建立名為campaign的資料夾。
* [提供對服務使用者「資料」之促銷活動資料夾的讀取存取權。](http://localhost:4502/useradmin)
