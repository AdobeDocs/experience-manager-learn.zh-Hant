---
title: 在本機部署資產
description: 在本機AEM執行個體上部署教學課程資產
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# 在您的系統上部署

請依照下列步驟，將此使用案例用於本機AEM執行個體。

* [部署Zip檔案中包含的DevelopingWithServiceUser組合](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)。

* 在Apache Sling服務使用者對應程式服務&#x200B;**DevelopingWithServiceUser.core：getformsresourceresolver=fd-service**&#x200B;中，使用[configMgr](http://localhost:4502/system/console/configMgr)新增下列專案。

* [部署電子報組合](assets/Newsletters.core-1.0.0-SNAPSHOT.jar)。 此套件包含列出資料夾內容與組合所選電子報的程式碼。

* [使用封裝管理員](assets/newsletter.zip)匯入封裝。 此套件包含使用者端程式庫和測試解決方案的範例pdf檔案。

* [匯入範例最適化表單](assets/sample-adaptive-form.zip)。 此表單將列出可選取的電子報。

* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled)。
選取要下載的電子報。選取的電子報將合併為一個pdf並傳回給您。
