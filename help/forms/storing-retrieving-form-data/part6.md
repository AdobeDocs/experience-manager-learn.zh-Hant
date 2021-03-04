---
title: 從MySQL資料庫儲存和檢索表單資料
description: 多部分教學課程，引導您逐步瞭解儲存和擷取表單資料的相關步驟
feature: 自適應表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 2%

---


# 將它部署在您的伺服器上

>[!NOTE]
>
>以下是在您的系統上執行此項作業的必要條件
>
>* AEM Forms（6.3版或更高版本）
>* MySql資料庫


若要在您的AEM Forms實例上測試此功能，請遵循下列步驟

* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)下載和部署[MySql驅動程式Jar](assets/mysqldriver.jar)檔案
* 使用[felix網頁主控台](http://localhost:4502/system/console/bundles)下載並部署[OSGi bundle](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)
* 使用[套件管理器](http://localhost:4502/crx/packmgr/index.jsp)下載並安裝包含客戶端庫、最適化表單模板和自定義頁面元件](assets/store-and-fetch-af-with-data.zip)的[套件
* 使用[FormsAndDocuments介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[範例最適化表單](assets/sample-adaptive-form.zip)

* 使用MySql Workbench匯入[form-data-db.sql](assets/form-data-db.sql)。 這將在資料庫中建立必要的模式和表，以便本教程使用。
* 登入[configMgr。](http://localhost:4502/system/console/configMgr) 搜尋「Apache Sling Connection Pooled DataSource」。使用下列屬性，建立名為&#x200B;**SaveAndContinue**&#x200B;的新Apache Sling Connection Pooled Datasource項目：

| 屬性名稱 | 值 |
------------------------|---------------------------------------
| 資料來源名稱 | SaveAndContinue |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接uri | jdbc:mysql://localhost:3306/aemformstutorial |


* 開啟[最適化表單](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填寫部分詳細資訊，然後按一下「儲存並稍後繼續」按鈕。
* 您應該回傳內含GUID的URL。
* 複製URL並貼到新的瀏覽器標籤中。 **請確定URL結尾沒有空白。**
* 最適化表單應填入上一步驟的資料。
