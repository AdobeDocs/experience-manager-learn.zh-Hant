---
title: 從MySQL資料庫儲存和檢索表單資料 — 配置資料源
description: 多部分教程，引導您完成儲存和檢索表單資料所涉及的步驟
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 1%

---

# 配置資料源

有多種方法可AEM以與外部資料庫整合。 資料庫整合最常見的標準做法之一是使用Apache Sling連接池化資料源配置屬性， [configMgr](http://localhost:4502/system/console/configMgr)。
第一步是下載並部署相應的 [MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 的上AEM界。
建立Apache Sling連接池化資料源，並提供如下螢幕抓圖中指定的屬性。 本教程資源中將提供資料庫模式。

![資料源](assets/save-continue.PNG)

資料庫有一個名為formdata的表，其中有3列，如下面的螢幕抓圖所示。

![資料庫](assets/data-base-tables.PNG)

要建立架構的sql檔案可以是 [從此處下載](assets/form-data-db.sql)。 您需要使用MySql工作台導入此檔案以建立方案和表。

>[!NOTE]
>請確保為資料源命名 **保存並繼續**。 示例代碼使用名稱連接到資料庫。

| 屬性名稱 | 值 |
| ------------------------|---------------------------------------|
| 資料源名稱 | 保存並繼續 |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接URI | jdbc:mysql://localhost:3306/aemformational |
