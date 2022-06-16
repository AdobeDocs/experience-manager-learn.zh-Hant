---
title: AEM Forms和Adobe Campaign Standard入門
description: 使用AEM Forms表單資料模型將AEM Forms與Adobe Campaign Standard整合，以獲取ACS活動簡介資訊等。
feature: Adaptive Forms, Form Data Model
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
source-git-commit: 5c53919dd038c0992e1fe5dd85053f26c03c5111
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# AEM Forms和Adobe Campaign Standard入門 {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![形式化](assets/helpx-cards-forms.png)

本教程將列出將AEM Forms與Adobe Campaign Standard(ACS)整合的各種使用案例。

ACS擁有豐富的API，使ACS能夠與我們選擇的技術進行交互。 在本教程中，我們將重點介紹AEM Forms與ACS的介面。

要將AEM Forms與ACS整合，您需要執行以下步驟：

* [設定ACS實例的API訪問權限。](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* 建立JSON Web令牌。
* 將JSON Web令牌與AdobeIdentity Management服務交換為訪問令牌。
* 在授權HTTP標頭中包括此訪問令牌，並在每次向ACS實例請求時都包括X-API-Key。

要開始，請按照以下說明操作

* [下載並解壓縮與本教程相關的資產。](assets/aem-forms-and-acs-bundles.zip)
* 使用 [Felix Web控制台](http://localhost:4502/system/console/bundles)
* 在Felix OSGI配置中為Adobe Campaign提供適當設定。
* [建立本文中提到的服務用戶](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。 確保部署與項目關聯的OSGi捆綁包。
* 將ACS私鑰儲存在etc/key/campaign/private.key中。 您必須在etc/key下建立名為campaign的資料夾。
* [為服務用戶「data」提供對市場活動資料夾的讀取權限。](http://localhost:4502/useradmin)
