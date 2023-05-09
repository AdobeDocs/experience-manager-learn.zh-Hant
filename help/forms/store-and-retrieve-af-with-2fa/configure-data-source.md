---
title: 配置資料源
description: 建立指向MySQL資料庫的DataSource
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 1%

---

# 配置資料源

AEM可透過許多方式啟用與外部資料庫的整合。 資料庫整合最常見的標準實務之一，就是透過 [configMgr](http://localhost:4502/system/console/configMgr).
第一步是下載並部署適當的 [MySQL驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 到AEM。
然後，設定資料庫專屬的Sling Connection Pooled DataSource屬性。 以下螢幕擷取畫面顯示本教學課程所使用的設定。 本教學課程資產會提供您資料庫結構。

![資料來源](assets/data-source.JPG)


* JDBC驅動程式類： `com.mysql.cj.jdbc.Driver`
* JDBC連接URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>請確定您的資料源名稱 `StoreAndRetrieveAfData` 因為這是OSGi服務中使用的名稱。


## 建立資料庫


以下資料庫用於此使用案例。 資料庫有一個名為 `formdatawithattachments` 4欄，如下方螢幕擷取所示。
![資料庫](assets/table-schema.JPG)

* 欄 **afdata** 會保留最適化表單資料。
* 欄 **attachmentsInfo** 將保存有關表單附件的資訊。
* 欄 **電話號碼** 會保留填寫表格的人的手機號碼。

請通過導入 [資料庫模式](assets/data-base-schema.sql)
使用MySQL Workbench。

## 建立表單資料模型

建立表單資料模型，並以前一步驟中建立的資料來源為基礎。
設定 **get** 此表單資料模型的服務，如下方螢幕擷取所示。
請確定您未在 **get** 服務。

其目的 **get** 服務是擷取與應用程式id相關聯的電話號碼。

![get-service](assets/get-service.JPG)

此表單資料模型將用於 **MyAccountForm** 以擷取與應用程式id相關聯的電話號碼。

## 後續步驟

[編寫代碼以保存表單附件](./store-form-attachments.md)
