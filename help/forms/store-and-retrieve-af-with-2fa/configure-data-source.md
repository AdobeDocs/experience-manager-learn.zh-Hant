---
title: 配置資料源
description: 建立指向MySQL資料庫的DataSource
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---


# 配置資料源

AEM可透過許多方式與外部資料庫整合。 資料庫整合最常見的標準做法之一，就是透過 [configMgr使用Apache Sling Connection Pooled DataSource組態屬性](http://localhost:4502/system/console/configMgr)。
第一步是下載並部署適當的 [MySQL驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 至AEM。
然後設定您資料庫專屬的Sling Connection Pooled DataSource屬性。 以下螢幕擷取顯示本教學課程所使用的設定。 本教學課程資產會提供資料庫架構給您。

![資料源](assets/data-source.JPG)


* JDBC驅動程式類： `com.mysql.cj.jdbc.Driver`
* JDBC連接URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>請務必將資料來源命 `StoreAndRetrieveAfData` 名為OSGi服務中使用的名稱。


## 建立資料庫


以下資料庫用於此使用案例。 資料庫有一個表格， `formdatawithattachments` 名為4欄，如下面的螢幕擷取畫面所示。
![資料庫](assets/table-schema.JPG)

* 欄afdata **將保** 存最適化表單資料。
* 列attachmentsInfo **** 將保存有關表單附件的資訊。
* 欄位 **telephoneNumber** 將保留填寫表單之人員的行動號碼。

請使用MySQL工作台導入數 [據庫](assets/data-base-schema.sql)架構以建立資料庫。

## 建立表單資料模型

建立表單資料模型，並以上一步驟中建立的資料來源為基礎。
如下 **方螢幕擷取** ，設定此表單資料模型的get服務。
請確定您未在get服務中返回 **陣列** 。

此 **get服務** ，用來擷取與應用程式ID相關的電話號碼。

![get-service](assets/get-service.JPG)

然後，此表單資料模型將用於 **MyAccountForm** ，以擷取與應用程式ID相關聯的電話號碼。
