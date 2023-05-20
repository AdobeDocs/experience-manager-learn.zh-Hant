---
title: 在本地部署資產
description: 在本地實例上部署教程資AEM產
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# 在系統上部署

請按照下面列出的步驟，使此用例在您的本地實例上AEM工作。

* [部署DevelopingWithServiceUser捆綁包](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) 包含在zip檔案中。

* 在Apache Sling服務用戶映射器服務中添加以下條目 **DevegingWithServiceUser.core:getformsresourceresolver=fd-service** 使用 [configMgr](http://localhost:4502/system/console/configMgr)。

* [部署新聞稿捆綁包](assets/Newsletters.core-1.0.0-SNAPSHOT.jar)。 此捆綁包包含用於列出資料夾內容和匯編所選新聞稿的代碼。

* [使用包管理器導入包](assets/newsletter.zip)。 此軟體包包含客戶端庫和示例pdf檔案以test解決方案。

* [導入示例自適應窗體](assets/sample-adaptive-form.zip)。 此表單將列出可選擇的新聞稿。

* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled)。
選擇要下載的幾份新聞稿。所選新聞稿將合併成一個pdf並返回給您。
