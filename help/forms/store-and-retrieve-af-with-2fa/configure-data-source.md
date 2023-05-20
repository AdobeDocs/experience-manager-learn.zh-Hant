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

支援與外部資料AEM庫整合的方法有很多。 資料庫整合最常見的標準做法之一是使用Apache Sling連接池化資料源配置屬性， [configMgr](http://localhost:4502/system/console/configMgr)。
第一步是下載並部署相應的 [MySQL驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 到AEM。
然後，設定特定於資料庫的Sling連接池化資料源屬性。 以下螢幕快照顯示了本教程使用的設定。 本教程資源中將提供資料庫模式。

![資料源](assets/data-source.JPG)


* JDBC驅動程式類： `com.mysql.cj.jdbc.Driver`
* JDBC連接URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>請確保為資料源命名 `StoreAndRetrieveAfData` 因為這是OSGi服務中使用的名稱。


## 建立資料庫


以下資料庫用於此使用案例。 資料庫有一個名為 `formdatawithattachments` 螢幕截圖中顯示的4列。
![資料庫](assets/table-schema.JPG)

* 列 **afdata** 將保存自適應表單資料。
* 列 **附件資訊** 將保存有關表單附件的資訊。
* 列 **電話號碼** 將保存填寫表格的人的手機號碼。

請通過導入 [資料庫模式](assets/data-base-schema.sql)
使用MySQL工作台。

## 建立表單資料模型

建立表單資料模型，並基於上一步中建立的資料源。
配置 **得** 以下螢幕抓圖所示的此表單資料模型的服務。
確保未在 **得** 服務。

此目的 **得** 服務是獲取與應用程式ID關聯的電話號碼。

![獲取服務](assets/get-service.JPG)

此表單資料模型將用於 **我的帳戶表單** 獲取與應用程式ID關聯的電話號碼。

## 後續步驟

[編寫代碼以保存表單附件](./store-form-attachments.md)
