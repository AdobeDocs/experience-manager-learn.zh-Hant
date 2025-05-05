---
title: 從MySQL資料庫儲存及擷取表單資料 — 部署
description: 多部分教學課程，逐步引導您完成儲存和擷取表單資料的相關步驟
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: Experience Manager 6.4, Experience Manager 6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 47
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---

# 在您的伺服器上部署此專案

>[!NOTE]
>
>若要讓此專案在您的系統上執行，需要下列專案
>
>* AEM Forms（6.3版或更高版本）
>* MySql資料庫

若要在您的AEM Forms執行個體上測試此功能，請遵循下列步驟

* 使用[felix Web主控台](http://localhost:4502/system/console/bundles)下載並部署[MySql驅動程式Jar](assets/mysqldriver.jar)檔案
* 使用[felix網頁主控台](http://localhost:4502/system/console/bundles)下載並部署[OSGi套件](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)
* 使用[封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)下載並安裝包含使用者端程式庫、最適化表單範本和自訂頁面元件[&#128279;](assets/store-and-fetch-af-with-data.zip)的封裝
* 使用[FormsAndDocuments介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[範例最適化表單](assets/sample-adaptive-form.zip)

* 使用MySql工作台匯入[form-data-db.sql](assets/form-data-db.sql)。 這會在您的資料庫中建立必要的綱要和表格，讓本教學課程能夠運作。
* 登入[configMgr。](http://localhost:4502/system/console/configMgr)搜尋「Apache Sling連線集區DataSource」。 使用以下屬性建立名為&#x200B;**SaveAndContinue**&#x200B;的新Apache Sling連線集區資料來源專案：

| 屬性名稱 | 值 |
| ------------------------|---------------------------------------|
| 資料來源名稱 | `SaveAndContinue` |
| JDBC驅動程式類別 | `com.mysql.cj.jdbc.Driver` |
| JDBC連線URI | `jdbc:mysql://localhost:3306/aemformstutorial` |

* 開啟[最適化表單](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填寫一些詳細資訊，然後按一下「儲存並稍後繼續」按鈕。
* 您應該要取回含有GUID的URL。
* 複製URL並將其貼到新的瀏覽器標籤中。 **請確定URL結尾沒有空白字元。**
* 最適化表單應填入上一步驟的資料。
