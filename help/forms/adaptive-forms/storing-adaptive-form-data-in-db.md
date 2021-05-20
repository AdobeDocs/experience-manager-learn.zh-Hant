---
title: 儲存最適化表單資料
seo-title: 儲存最適化表單資料
description: 將最適化表單資料儲存至DataBase，作為AEM工作流程的一部分
seo-description: 將最適化表單資料儲存至DataBase，作為AEM工作流程的一部分
feature: 適用性Forms，工作流程，表單資料模型
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 0%

---


# 將最適化表單提交儲存在資料庫中

在您選擇的資料庫中儲存已提交表單資料的方法有很多。 JDBC資料源可用於將資料直接儲存到資料庫中。 可以編寫自訂OSGI套件組合，將資料儲存至資料庫。 本文使用AEM工作流程中的自訂處理步驟來儲存資料。
使用案例是在適用性表單提交上觸發AEM工作流程，工作流程中的步驟會將提交的資料儲存至資料庫。

**請依照下列步驟操作，以便在您的系統上運作**

* [下載Zip檔案，並將其內容解壓縮至硬碟](assets/storeafdataindb.zip)

   * 使用套件管理器將StoreAFInDBWorkflow.zip匯入AEM。 包具有將AF資料儲存到DB的示例工作流。 開啟工作流模型。 工作流程只有一個步驟。 此步驟會呼叫套件中寫入的程式碼，以將AF資料儲存至資料庫。 我將一個單一的引數傳遞給這個過程。 這是要儲存其資料的適用性表單的名稱。
   * 使用Felix Web控制台部署insertdata.core-0.0.1-SNAPSHOT.jar。 此套件包含將提交的表單資料寫入資料庫的代碼

* 轉至[ConfigMgr](http://localhost:4502/system/console/configMgr)

   * 搜索「JDBC連接池」。 建立新的Day Commons JDBC連接池。 指定資料庫的特定設定。

   * ![jdb連接池](assets/jdbc-connection-pool.png)
   * 搜索「**將表單資料插入DB**」
   * 指定資料庫的特定屬性。
      * DataSourceName：您以前配置的資料源的名稱。
      * TableName — 要儲存AF資料的表的名稱
      * FormName — 用於保存表單名稱的列名
      * ColumnName — 用於保存AF資料的列名

   ![插入資料](assets/insertdata.PNG)

* 建立最適化表單。

* 將最適化表單與AEM Workflow(StoreAFValuesinDB)關聯，如下方螢幕擷取畫面所示。

* 請務必在資料檔案路徑中指定「data.xml」，如下方螢幕擷取畫面所示

   ![提交](assets/submissionafforms.png)

* 預覽表單並提交

* 如果一切順利，您應該會看到表單資料儲存在您指定的表格和欄中



