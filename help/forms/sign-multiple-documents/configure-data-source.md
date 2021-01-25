---
title: 設定AEM資料來源
description: 配置MySQL支援的資料源以儲存和檢索表單資料
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 2%

---

# 設定資料來源

AEM有許多方式可讓您與外部資料庫整合。 整合資料庫的最常見方式之一，是透過[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection Pooled DataSource組態屬性。
第一步是在AEM中下載並部署適當的[MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
建立Apache Sling Connection Pooled DataSource並提供如下螢幕擷取畫面中指定的屬性。 本教學課程資產會提供資料庫架構給您。

![資料源](assets/data-source.PNG)

資料庫有一個名為formdata的表格，其中3欄如下方的螢幕擷取畫面所示。

![資料庫](assets/data-base.PNG)


>[!NOTE]
>請確定您的資料來源&#x200B;**aemformstutorial**。 示例代碼使用名稱連接到資料庫。

| 屬性名稱 | 值 |
------------------------|---------------------------------------
| 資料來源名稱 | SaveAndContinue |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接uri | jdbc:mysql://localhost:3306/aemformstutorial |

## 資產

建立模式的sql檔案可從此處[下載。 ](assets/sign-multiple-forms.sql)您需要使用MySql工作台導入此檔案以建立方案和表。


