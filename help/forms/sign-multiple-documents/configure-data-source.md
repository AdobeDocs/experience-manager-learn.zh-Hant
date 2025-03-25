---
title: 設定AEM Data Source
description: 設定MySQL支援的資料來源以儲存及擷取表單資料
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6899
thumbnail: 6899.jpg
exl-id: 2e851ae5-6caa-42e3-8af2-090766a6f36a
duration: 39
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 3%

---

# 設定資料來源

AEM有許多方式可用來與外部資料庫整合。 整合資料庫的最常見方式之一，是透過[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection Pooled DataSource組態屬性。
第一步是下載並部署AEM中適當的[MySql驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java)。
建立Apache Sling Connection Pooled DataSource，並提供以下熒幕擷取畫面中指定的屬性。 資料庫結構描述會作為本教學課程資產的一部分提供給您。

![資料來源](assets/data-source.PNG)

資料庫有一個名為formdata的表格，包含3個資料欄，如下面的熒幕擷取畫面所示。

![資料庫](assets/data-base.PNG)


>[!NOTE]
>請確定您為資料來源命名&#x200B;**aemformstutorial**。 範常式式碼使用名稱來連線至資料庫。

| 屬性名稱 | 值 |
| ------------------------|--------------------------------------- |
| 資料來源名稱 | `SaveAndContinue` |
| JDBC驅動程式類別 | `com.mysql.cj.jdbc.Driver` |
| JDBC連線URI | `jdbc:mysql://localhost:3306/aemformstutorial` |

## Assets

可從這裡](assets/sign-multiple-forms.sql)下載建立結構描述的SQL檔案[。 您必須使用MySql Workbench匯入此檔案，才能建立結構描述和表格。

## 後續步驟

[建立OSGi服務以儲存和擷取資料庫中的資料](./create-osgi-service.md)
