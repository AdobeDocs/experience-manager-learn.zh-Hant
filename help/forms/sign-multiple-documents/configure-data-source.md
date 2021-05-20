---
title: 設定AEM資料來源
description: 配置MySQL備份資料源以儲存和檢索表單資料
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 3%

---

# 配置資料源

AEM可透過許多方式啟用與外部資料庫的整合。 整合資料庫最常見的方法之一，是透過[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection Pooled DataSource設定屬性。
第一步是在AEM中下載並部署適當的[MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
建立Apache Sling Connection Pooled DataSource ，並依照下方螢幕擷取畫面中的指定提供屬性。 本教學課程資產會提供您資料庫結構。

![資料來源](assets/data-source.PNG)

資料庫有一個名為formdata的表，其中有3列，如下面螢幕抓圖所示。

![資料庫](assets/data-base.PNG)


>[!NOTE]
>請確定您為資料源命名&#x200B;**aemformstutorial**。 范常式式碼會使用名稱連線至資料庫。

| 屬性名稱 | 值 |
------------------------|---------------------------------------
| 資料源名稱 | SaveAndContinue |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接uri | jdbc:mysql://localhost:3306/aemformstutorial |

## 資產

建立架構的SQL檔案可從此處[下載。 ](assets/sign-multiple-forms.sql)您需要使用MySql Workbench導入此檔案，以建立架構和表。


