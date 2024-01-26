---
title: 在您的伺服器上部署範例資產
description: 測試互動式通訊的另存為草稿功能
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 42
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---

# 在您的伺服器上部署範例資產

請依照下列指示，讓此功能在您的AEM伺服器上運作

* [建立資料庫綱要](assets/icdrafts.sql)
* [匯入使用者端資源庫](assets/icdrafts.zip)
* [匯入最適化表單](assets/SavedDraftsAdaptiveForm.zip)
* 建立資料來源，稱為 _SaveAndContinue_

![建立資料來源](assets/data-source.png)

| 屬性名稱 | 屬性值 |
|---|---|
| 資料來源名稱 | SaveAndContinue |
| JDBC驅動程式類別 | com.mysql.cj.jdbc.Driver |
| JDBC連線URL | jdbc:mysql://localhost：3306/aemformstutorial？autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [部署icdraft組合](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* 請確定 _使用CCRDocumentInstanceService啟用儲存_ 在OSGI設定中，如下所示
  ![啟用草稿](assets/enable-drafts.png)
* 開啟任何互動式通訊。 按一下另存為草稿以儲存
* [檢視已儲存的草稿](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>xml檔案儲存在AEM伺服器安裝的根資料夾中。 Eclipse專案是提供給您根據您的需求自訂解決方案。

包含範例實作的eclipse專案可以是 [已從此處下載](assets/icdrafts-eclipse-project.zip)
