---
title: 在本機伺服器上部署範例
description: 多部分教學課程，逐步引導您完成查詢Azure入口網站中儲存之表單提交內容的步驟
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 44841a3c-85e0-447f-85e2-169a451d9c68
duration: 20
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# 在本機伺服器上部署範例

若要讓此使用案例在本機伺服器上運作，請遵循下列步驟。假設您的AEM執行個體在localhost 4502連線埠上執行。

* 使用封裝管理員[安裝封裝](assets/azuredemo.all-1.0.0-SNAPSHOT.zip)。

* 使用OSGi configMgr提供Azure入口網站認證
  ![azure-portal](assets/azure-portal-config.png)
確定儲存URI以斜線結尾，且SAS權杖以開頭？
* 瀏覽至[AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* 編輯下列3個資料來源的驗證設定，以符合您的環境
  ![資料來源](assets/fdm-data-sources.png)

* 預覽並提交[ContactUs表單](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [查詢您的表單提交](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)
