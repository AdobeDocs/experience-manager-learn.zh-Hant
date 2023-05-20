---
title: 配置AEM資料源
description: 配置MySQL支援的資料源以儲存和檢索表單資料
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

有多種方法可AEM以與外部資料庫整合。 整合資料庫的最常見方法之一是使用Apache Sling連接池化資料源配置屬性 [configMgr](http://localhost:4502/system/console/configMgr)。
第一步是下載並部署相應的 [MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 的上AEM界。
建立Apache Sling連接池化資料源，並提供如下螢幕抓圖中指定的屬性。 本教程資源中將提供資料庫模式。

![資料源](assets/data-source.PNG)

資料庫有一個名為formdata的表，其中有3列，如下面的螢幕抓圖所示。

![資料庫](assets/data-base.PNG)


>[!NOTE]
>請確保為資料源命名 **雄獅**。 示例代碼使用名稱連接到資料庫。

| 屬性名稱 | 值 |
| ------------------------|--------------------------------------- |
| 資料源名稱 | 保存並繼續 |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接URI | jdbc:mysql://localhost:3306/aemformational |

## Assets

要建立架構的SQL檔案可以是 [從此處下載](assets/sign-multiple-forms.sql)。 您需要使用MySql工作台導入此檔案以建立方案和表。

## 後續步驟

[建立OSGi服務以在資料庫中儲存和讀取資料](./create-osgi-service.md)
