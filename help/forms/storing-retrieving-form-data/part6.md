---
title: 從MySQL資料庫儲存和檢索表單資料
description: 多部分教學課程，逐步引導您完成儲存和擷取表單資料的相關步驟
feature: 適用性表單
topic: 開發
role: Developer
level: Experienced
version: 6.3,6.4,6.5
source-git-commit: 3569d8b2a38d1cce02f6f4ff8b0c583f4dc118b6
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 3%

---


# 在您的伺服器上部署

>[!NOTE]
>
>需要下列條件才能在您的系統上運行此程式
>
>* AEM Forms（6.3版或更高版本）
>* MySql資料庫


若要在您的AEM Forms執行個體上測試此功能，請執行下列步驟

* 使用[felix web控制台](http://localhost:4502/system/console/bundles)下載並部署[MySql驅動程式Jar](assets/mysqldriver.jar)檔案
* 使用[felix Web控制台](http://localhost:4502/system/console/bundles)下載並部署[OSGi套件組合](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)
* 使用[套件管理器](http://localhost:4502/crx/packmgr/index.jsp)下載並安裝包含用戶端庫、最適化表單範本和自訂頁面元件](assets/store-and-fetch-af-with-data.zip)的[套件
* 使用[FormsAndDocuments介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[範例適用性表單](assets/sample-adaptive-form.zip)

* 使用MySql Workbench匯入[form-data-db.sql](assets/form-data-db.sql)。 這將在您的資料庫中建立必要的架構和表，以便本教學課程發揮作用。
* 登錄[configMgr。](http://localhost:4502/system/console/configMgr) 搜尋「Apache Sling Connection Pooled DataSource」。使用下列屬性，建立名為&#x200B;**SaveAndContinue**&#x200B;的新Apache Sling Connection Pooled Datasource項目：

| 屬性名稱 | 值 |
| ------------------------|---------------------------------------|
| 資料源名稱 | SaveAndContinue |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接uri | jdbc:mysql:/localhost:3306/aemformation |

* 開啟[適用性表單](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填寫一些詳細資訊，然後按一下「儲存並稍後繼續」按鈕。
* 您應該會傳回內含GUID的URL。
* 複製URL並貼到新的瀏覽器標籤中。 **請確定URL結尾沒有空格。**
* 適用性表單應填入上一個步驟的資料。
