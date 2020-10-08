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
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# 將它部署在您的伺服器上

>[!NOTE]
>
>以下是在您的系統上執行此項作業的必要條件
>
>* AEM Forms（6.3版或更新版本）
>* MYSQL資料庫


若要在您的AEM Forms例項上測試此功能，請依照下列步驟進行

* 下載教學課程 [資產](assets/store-retrieve-form-data.zip) ，並將其解壓縮至您的本機系統
* 使用 [Felix Web主控台部署並啟動techmarketingdemos.jar和mysqldriver.jar組合](http://localhost:4502/system/console/configMgr)
* 使用MYSQL Workbench匯入aemformstutorial.sql。 這將在資料庫中建立必要的模式和表，以便本教程使用。
* 使用 [AEM套件管理員匯入StoreAndRetrieve.zip。](http://localhost:4502/crx/packmgr/index.jsp) 此軟體包包含Adaptive Form模板、頁元件客戶端庫以及示例adaptive form和資料源配置。
* 登入 [configMgr。](http://localhost:4502/system/console/configMgr) 搜尋「Apache Sling Connection Pooled DataSource」。 開啟與Aemformatior相關的資料來源項目，並輸入您資料庫例項專屬的使用者名稱和密碼。
* 開啟最適 [化表單](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填寫部分詳細資訊，然後按一下「儲存並稍後繼續」按鈕
* 您應該回傳內含GUID的URL。
* 複製URL並貼到新的瀏覽器標籤中。 **請確定URL結尾沒有空白字元**
* 最適化表單應填入上一步驟的資料
