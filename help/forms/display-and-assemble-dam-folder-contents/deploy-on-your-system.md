---
title: 選取並下載DAM資料夾內容
description: 在您的本機AEM執行個體上部署教學課程資產
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: a2bbb26751c9182056b4fe6d36eeeec964001df8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# 在您的系統上部署

請依照下列步驟操作，讓此使用案例在您的本機AEM執行個體上運作。

* [請依照本文所述步驟，設定為使用fd-service使用者](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en). 請確定您已部署DevelopingWithServiceUser套件組合。

* [部署電子報套件組合](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 此套件包含的程式碼會列出資料夾內容，並組合選取的電子報。

* [使用套件管理器匯入套件](assets/newsletter.zip). 此套件包含用來測試解決方案的用戶端程式庫和範例pdf檔案。

* [匯入範例最適化表單](assets/sample-adaptive-form.zip). 此表單將列出可選擇的電子報。

* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
選擇要下載的電子報。所選電子報將合併為一個pdf並返回給您。




