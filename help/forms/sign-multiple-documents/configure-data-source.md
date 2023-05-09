---
title: 設定AEM資料來源
description: 配置MySQL備份資料源以儲存和檢索表單資料
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 2%

---

# 配置資料源

AEM可透過許多方式啟用與外部資料庫的整合。 整合資料庫最常見的方式之一，是透過 [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下載並部署適當的 [MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 在AEM中。
建立Apache Sling Connection Pooled DataSource ，並依照下方螢幕擷取畫面中的指定提供屬性。 本教學課程資產會提供您資料庫結構。

![資料來源](assets/data-source.PNG)

資料庫有一個名為formdata的表，其中有3列，如下面螢幕抓圖所示。

![資料庫](assets/data-base.PNG)


>[!NOTE]
>請確定您的資料源名稱 **修飾**. 范常式式碼會使用名稱連線至資料庫。

| 屬性名稱 | 值 |
| ------------------------|--------------------------------------- |
| 資料源名稱 | SaveAndContinue |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接uri | jdbc:mysql://localhost:3306/aemformation |

## Assets

建立架構的SQL檔案可以是 [從此處下載](assets/sign-multiple-forms.sql). 您需要使用MySql Workbench導入此檔案，以建立架構和表。

## 後續步驟

[建立OSGi服務以在資料庫中儲存和擷取資料](./create-osgi-service.md)
