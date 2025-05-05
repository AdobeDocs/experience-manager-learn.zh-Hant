---
title: 設定Data Source
description: 建立指向MySQL資料庫的DataSource
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 64
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 3%

---

# 設定Data Source

AEM有許多方式可啟用與外部資料庫的整合。 資料庫整合最常見和標準的作法之一，是透過[configMgr](http://localhost:4502/system/console/configMgr)使用Apache Sling Connection Pooled DataSource組態屬性。
第一步是下載適當的[MySQL驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java)並部署至AEM。
然後，設定資料庫專屬的Sling Connection Pooled DataSource屬性。 下列熒幕擷圖顯示用於本教學課程的設定。 資料庫結構描述會作為本教學課程資產的一部分提供給您。

>[!NOTE]
>請確定您為資料來源命名`StoreAndRetrieveAfData`，因為這是OSGi服務中使用的名稱。


![資料來源](assets/data-source.JPG)

| 屬性名稱 | 屬性值 |   |
|---------------------|------------------------------------------------------------------------------------|---|
| 資料來源名稱 | `StoreAndRetrieveAfData` |   |
| jdbc磁碟機類別 | `jdbc:mysql://localhost:3306/aemformstutorial` |   |
| JDBC連線URI | `jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&autoReconnect=true` |   |
|                     |                                                                                    |   |


## 建立資料庫


以下資料庫用於此使用案例。 資料庫有一個名為`formdatawithattachments`的資料表，包含4個資料行，如下面的熒幕擷圖所示。
![資料庫](assets/table-schema.JPG)

* 資料行&#x200B;**afdata**&#x200B;將儲存最適化表單資料。
* 資料行&#x200B;**attachmentsInfo**&#x200B;將儲存表單附件的相關資訊。
* 欄&#x200B;**telephoneNumber**&#x200B;將保留填寫表單之人員的行動電話號碼。

請匯入[資料庫結構描述](assets/data-base-schema.sql)以建立資料庫
使用MySQL Workbench。

## 建立表單資料模型

建立表單資料模型，並以上一步驟中建立的資料來源為基礎。
設定此表單資料模型的&#x200B;**get**&#x200B;服務，如下列熒幕擷圖所示。
請確定您沒有在&#x200B;**get**&#x200B;服務中傳回陣列。

此&#x200B;**get**&#x200B;服務的目的是擷取與應用程式ID關聯的電話號碼。

![取得服務](assets/get-service.JPG)

然後，此表單資料模型將用於&#x200B;**MyAccountForm**，以擷取與應用程式ID關聯的電話號碼。

## 後續步驟

[撰寫程式碼以儲存表單附件](./store-form-attachments.md)
