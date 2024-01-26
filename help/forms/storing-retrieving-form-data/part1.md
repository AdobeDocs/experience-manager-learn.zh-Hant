---
title: 從MySQL資料庫儲存及擷取表單資料 — 設定資料來源
description: 多部分教學課程，逐步引導您完成儲存和擷取表單資料的相關步驟
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---

# 設定資料來源

AEM有許多方式可啟用與外部資料庫的整合。 資料庫整合最常見和標準的作法之一，就是透過使用Apache Sling Connection Pooled DataSource [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下載並部署適當的 [MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 在AEM中。
建立Apache Sling Connection Pooled DataSource，並提供以下熒幕擷取畫面中指定的屬性。 資料庫結構描述會作為本教學課程資產的一部分提供給您。

![data-source](assets/save-continue.PNG)

資料庫有一個名為formdata的表格，包含3個資料欄，如下面的熒幕擷取畫面所示。

![資料庫](assets/data-base-tables.PNG)

建立結構描述的sql檔案可以是 [已從此處下載](assets/form-data-db.sql). 您必須使用MySql Workbench匯入此檔案，才能建立結構描述和表格。

>[!NOTE]
>請務必為您的資料來源命名 **SaveAndContinue**. 範常式式碼使用名稱來連線至資料庫。

| 屬性名稱 | 值 |
| ------------------------|---------------------------------------|
| 資料來源名稱 | SaveAndContinue |
| JDBC驅動程式類別 | com.mysql.cj.jdbc.Driver |
| JDBC連線URI | jdbc:mysql://localhost：3306/aemformstutorial |
