---
title: 在伺服器上部署示例資產
description: TestInteractive Communications的另存為草稿功能
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 2%

---

# 在伺服器上部署示例資產

請按照以下說明使此功能在您的伺服器上AEM工作

* [建立資料庫架構](assets/icdrafts.sql)
* [導入客戶端庫](assets/icdrafts.zip)
* [導入自適應窗體](assets/SavedDraftsAdaptiveForm.zip)
* 建立調用的資料源 _保存並繼續_

![建立資料源](assets/data-source.png)

| 屬性名稱 | 屬性值 |
|---|---|
| 資料源名稱 | 保存並繼續 |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接URL | jdbc:mysql://localhost:3306/aemformstufal?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [部署icdrafts捆綁包](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* 確保 _使用CCRDocumentInstanceService啟用保存_ 在OSGI配置中，如下所示
   ![啟用草稿](assets/enable-drafts.png)
* 開啟任何互動式通信。 按一下「另存為草稿」(Save as Draft)以保存
* [查看保存的草稿](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>xml檔案儲存在伺服器安裝的根AEM資料夾中。 Eclipse項目>將提供給您，以根據您的要求定制解決方案。

具有示例實現的Eclipse項目可以 [從此處下載](assets/icdrafts-eclipse-project.zip)
