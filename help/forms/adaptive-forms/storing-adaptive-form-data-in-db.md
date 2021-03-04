---
title: 儲存自適應表單資料
seo-title: 儲存自適應表單資料
description: 將自適應表單資料儲存到DataBase，做為工作流程的一AEM部分
seo-description: 將自適應表單資料儲存到DataBase，做為工作流程的一AEM部分
feature: 自適應Forms，工作流，表單資料模型
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---


# 將自適應表單提交儲存在資料庫中

在您選擇的資料庫中儲存已提交表單資料的方式有很多。 JDBC資料源可用於將資料直接儲存到資料庫中。 可編寫自訂OSGI套件，將資料儲存至資料庫。 本文使用工作流中的定製流程AEM步驟來儲存資料。
使用案例是在Adaptive FormAEM提交上觸發工作流，工作流中的步驟將提交的資料儲存到資料庫中。

**請遵循下列步驟，讓此功能在您的系統上運作**

* [下載Zip檔案並將其內容解壓縮至硬碟](assets/storeafdataindb.zip)

   * 使用套件管理器將StoreAFInDBWorkflow.zip匯AEM入。 軟體包具有將AF資料儲存到資料庫的示例工作流。 開啟工作流程模型。 工作流程只有一個步驟。 此步驟調用在包中寫入的代碼，以將AF資料儲存到資料庫中。 我正在將單一論據傳遞至程式。 這是保存其資料的最適化表單的名稱。
   * 使用Felix網頁主控台部署insertdata.core-0.0.1-SNAPSHOT.jar。 此捆綁包具有將提交的表單資料寫入資料庫的代碼

* 轉至[ConfigMgr](http://localhost:4502/system/console/configMgr)

   * 搜索「JDBC連接池」。 建立新的日公用JDBC連接池。 指定您資料庫的特定設定。

   * ![jdbc連接池](assets/jdbc-connection-pool.png)
   * 搜索「**將表單資料插入DB**」
   * 指定資料庫的特定屬性。
      * DataSourceName：您先前設定之資料來源的名稱。
      * TableName —要儲存AF資料的表的名稱
      * FormName —— 用於保存表單名稱的列名
      * ColumnName —— 用於保存AF資料的列名

   ![插入資料](assets/insertdata.PNG)

* 建立最適化表單。

* 將最適化表單與AEM工作流程(StoreAFValuesinDB)產生關聯，如下面的螢幕擷取畫面所示。

* 請確定您在資料檔案路徑中指定「data.xml」，如下方螢幕擷取畫面所示

   ![提交](assets/submissionafforms.png)

* 預覽表單並送出

* 如果一切順利，您應該會看到表單資料儲存在您指定的表格和欄中



