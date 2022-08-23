---
title: 從MySQL資料庫儲存和檢索表單資料 — 部署
description: 多部分教程，引導您完成儲存和檢索表單資料所涉及的步驟
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 2%

---

# 在您的伺服器上部署此

>[!NOTE]
>
>以下是在您的系統上運行此程式所必需的
>
>* AEM Forms（6.3或更高版本）
>* MySql資料庫


要在AEM Forms實例上test此功能，請執行以下步驟

* 下載和部署 [MySql驅動程式Jar](assets/mysqldriver.jar) 使用 [felix Web控制台](http://localhost:4502/system/console/bundles)
* 下載和部署 [OSGi束](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) 使用 [felix Web控制台](http://localhost:4502/system/console/bundles)
* 下載並安裝 [包含客戶端庫、自適應表單模板和自定義頁面元件的包](assets/store-and-fetch-af-with-data.zip) 使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 導入 [示例自適應窗體](assets/sample-adaptive-form.zip) 使用 [FormsAndDocuments介面](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 導入 [form-data-db-sql](assets/form-data-db.sql) 使用MySql Workbench。 這將在資料庫中建立必要的架構和表，以便本教程能夠運行。
* 登錄到 [configMgr。](http://localhost:4502/system/console/configMgr) 搜索「Apache Sling連接池化資料源」。 建立名為的新Apache Sling連接池資料源條目 **保存並繼續** 使用以下屬性：

| 屬性名稱 | 值 |
| ------------------------|---------------------------------------|
| 資料源名稱 | 保存並繼續 |
| JDBC驅動程式類 | com.mysql.cj.jdbc.Driver |
| JDBC連接URI | jdbc:mysql://localhost:3306/aemformational |

* 開啟 [自適應窗體](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 填寫一些詳細資訊，然後按一下「保存並稍後繼續」按鈕。
* 您應返回GUID為的URL。
* 複製URL並將其貼上到新的瀏覽器頁籤中。 **確保URL末尾沒有空格。**
* 應使用上一步中的資料填充自適應表單。
