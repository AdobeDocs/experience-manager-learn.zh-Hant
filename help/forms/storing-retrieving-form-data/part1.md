---
title: 從MySQL資料庫儲存和檢索表單資料
description: 多部分教學課程，引導您逐步瞭解儲存和擷取表單資料的相關步驟
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 配置資料源

AEM可透過許多方式與外部資料庫整合。 資料庫整合最常見的標準做法之一，就是透過 [configMgr使用Apache Sling Connection Pooled DataSource組態屬性](http://localhost:4502/system/console/configMgr)。
第一個步驟是在AEM中下載並部署適當 [的My SQL驅動程式](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 。
然後設定Sling Connection Pooled DataSource屬性。 這些屬性是您的資料庫專用的。 以下螢幕擷取顯示本教學課程所使用的設定。 本教學課程資產會提供資料庫架構給您。
![資料源](assets/data-source.png)

資料庫有一個稱為formdata的表格，其中3欄如資料庫下方的螢幕擷取畫![面所示](assets/data-base-tables.PNG)
