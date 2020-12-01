---
title: 從MySQL資料庫儲存和檢索表單資料
description: 多部分教學課程，引導您逐步瞭解儲存和擷取表單資料的相關步驟
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 1%

---

# 配置資料源

AEM可透過許多方式與外部資料庫整合。 資料庫整合最常見的標準做法之一，是透過[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection Pooled DataSource組態屬性。
第一步是在AEM中下載並部署適當的[MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
建立Apache Sling Connection Pooled DataSource並提供如下螢幕擷取畫面中指定的屬性。 本教學課程資產會提供資料庫架構給您。

![資料源](assets/save-continue.PNG)

資料庫有一個名為formdata的表格，其中3欄如下方的螢幕擷取畫面所示。

![資料庫](assets/data-base-tables.PNG)

建立模式的sql檔案可從此處[下載。 ](assets/form-data-db.sql)您需要使用MySql工作台導入此檔案以建立方案和表。

>[!NOTE]
>請確定您的資料來源&#x200B;**SaveAndContinue**&#x200B;名稱。 示例代碼使用名稱連接到資料庫。

| 屬性名稱 | 值 |
------------------------|---------------------------------------
| 資料來源名稱 | SaveAndContinue |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接uri | jdbc:mysql://localhost:3306/aemformstutorial |


