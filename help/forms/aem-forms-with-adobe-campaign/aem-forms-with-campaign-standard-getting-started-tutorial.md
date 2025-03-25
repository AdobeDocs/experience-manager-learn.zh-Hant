---
title: 整合AEM Forms和Adobe Campaign Standard
description: 使用AEM Forms表單資料模型將AEM Forms與Adobe Campaign Standard整合，以擷取ACS促銷活動設定檔資訊等。
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 1%

---

# 整合AEM Forms和Adobe Campaign Standard

![formsandcampaign](assets/helpx-cards-forms.png)

瞭解如何將AEM Forms與Adobe Campaign Standard (ACS)整合。

ACS具有一組豐富的API，可讓ACS與我們選擇的技術連線。 在本教學課程中，我們將專注於將AEM Forms與ACS連線。

若要將AEM Forms與ACS整合，您需要遵循以下步驟：

* [在您的ACS執行個體上設定API存取。](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* 建立JSON Web權杖。
* 將JSON Web權杖與Adobe Identity Management服務交換為存取權杖。
* 在授權HTTP標頭中包含此存取Token，並在對ACS執行個體的每個請求中包含X-API-Key。

若要開始使用，請遵循下列指示

* [下載並解壓縮與本教學課程相關的資產。](assets/aem-forms-and-acs-bundles.zip)
* 使用[Felix Web主控台](http://localhost:4502/system/console/bundles)部署組合
* 在Felix OSGI設定中為Adobe Campaign提供適當的設定。
* [建立本文中提及的服務使用者](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。 請務必部署與文章相關聯的OSGi套件組合。
* 將ACS私密金鑰儲存在etc/key/campaign/private.key中。 您必須在etc/key下建立名為campaign的資料夾。
* [為服務使用者「資料」提供Campaign資料夾的讀取存取權。](http://localhost:4502/useradmin)

## 後續步驟

[產生JWT和存取權杖](partone.md)
