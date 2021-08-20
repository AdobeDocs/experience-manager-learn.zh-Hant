---
title: 配置資料源
description: 建立指向MySQL資料庫的DataSource
feature: 適用性表單
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: 開發
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 2%

---


# 配置資料源

AEM可透過許多方式啟用與外部資料庫的整合。 資料庫整合最常見且標準的作法之一，是透過[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection Pooled DataSource設定屬性。
第一步是下載並將相應的[MySQL驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java)部署到AEM。
然後設定資料庫專屬的Sling Connection Pooled DataSource屬性。 以下螢幕擷取畫面顯示本教學課程所使用的設定。 本教學課程資產會提供您資料庫結構。

![資料來源](assets/data-source.JPG)


* JDBC驅動程式類：`com.mysql.cj.jdbc.Driver`
* JDBC連接URI:`jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>請確定您為資料源命名`StoreAndRetrieveAfData`，因為這是OSGi服務中使用的名稱。


## 建立資料庫


以下資料庫用於此使用案例。 資料庫有一個名為`formdatawithattachments`的表，其中有4列，如下面的螢幕抓圖所示。
![資料庫](assets/table-schema.JPG)

* 欄&#x200B;**afdata**&#x200B;將保留最適化表單資料。
* 列&#x200B;**attachmentsInfo**&#x200B;將保存有關表單附件的資訊。
* 欄&#x200B;**telephoneNumber**&#x200B;將保留填寫表單之人員的行動電話號碼。

請通過導入[資料庫架構](assets/data-base-schema.sql)來建立資料庫
使用MySQL Workbench。

## 建立表單資料模型

建立表單資料模型，並以前一步驟中建立的資料來源為基礎。
設定此表單資料模型的**get**服務，如下方螢幕擷取畫面所示。
請確定您未在**get**&#x200B;服務中傳回陣列。

此&#x200B;**get**&#x200B;服務用於擷取與應用程式ID相關聯的電話號碼。

![get-service](assets/get-service.JPG)

然後，此表單資料模型將用於&#x200B;**MyAccountForm**&#x200B;中，以獲取與應用程式ID關聯的電話號碼。
