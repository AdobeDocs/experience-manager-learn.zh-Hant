---
title: 在本機部署資產
description: 在您的本機AEM執行個體上部署教學課程資產
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: ddef90067d3ae4a3c6a705b5e109e474bab34f6d
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# 在您的系統上部署

請依照下列步驟操作，讓此使用案例在您的本機AEM執行個體上運作。

* [部署DevelopingWithServiceUser套件組合](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) 包含在zip檔案中。

* 在Apache Sling Service使用者對應程式服務中新增下列項目 **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** 使用 [configMgr](http://localhost:4502/system/console/configMgr).

* [部署電子報套件組合](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). 此套件包含的程式碼會列出資料夾內容，並組合選取的電子報。

* [使用套件管理器匯入套件](assets/newsletter.zip). 此套件包含用來測試解決方案的用戶端程式庫和範例pdf檔案。

* [匯入範例最適化表單](assets/sample-adaptive-form.zip). 此表單將列出可選擇的電子報。

* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
選擇要下載的電子報。所選電子報將合併為一個pdf並返回給您。




