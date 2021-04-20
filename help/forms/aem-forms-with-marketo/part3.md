---
title: AEM Forms與馬克托（下）
seo-title: AEM Forms與馬克托（下）
description: 教學課程，將AEM Forms與Marketo整合，使用AEM Forms表單資料模型。
seo-description: 教學課程，將AEM Forms與Marketo整合，使用AEM Forms表單資料模型。
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# 配置資料源

AEM Forms資料整合可讓您設定並連線至不同的資料來源。 下列類型是現成可用的支援。 不過，只要進行一些自訂，您也可以與其他資料來源整合。

1. 關係資料庫- MySQL、Microsoft SQL Server、IBM DB2和OracleRDBMS。
1. AEM用戶配置檔案
1. REST風格的Web服務
1. 基於SOAP的web services
1. OData服務

為整合AEM Forms與Marketo，我們將使用REST風格的web services。 整合的第一步是設定[資料來源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 請使用本教學課程中提供的Swagger檔案。以下螢幕擷取顯示設定資料來源時需要指定的重要屬性。
![資料源](assets/datasource.jfif)

「marketo.json」是Swagger檔案，是本教學課程資產的一部分。
屬性主機是您的Marketo例項專屬的。
驗證類型是自訂的，且驗證實作必須符合「AemForms與Marketo」。 （除非您在程式碼中變更此項）。

## 建立表單資料模型

之後，設定資料來源的下一步是建立表單資料模型，此模型以前一步驟中設定的資料來源為基礎。 若要建立表單資料模型，請遵循下列步驟：

將您的瀏覽器指向[資料整合頁面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 這會列出在您的例項上建立的所有資料AEM整合。

1. 按一下「建立」 |表單資料模型
1. 提供有意義的標題，例如FormsAndMarketo，然後按一下「下一步」
1. 選擇在先前步驟中配置的資料源，然後按一下建立和編輯以在編輯模式下開啟表單資料模型
1. 展開「FormsAndMarketo」節點。 展開「服務」節點
1. 選擇第一個&quot;Get&quot;操作
1. 按一下「新增選取的項目」
1. 在「添加關聯的模型對象」(Add Associated Model Objects)對話框中按一下「全選」(Select All)，然後按一下「添加」(Add)
1. 按一下「儲存」按鈕，儲存表單資料模型
1. 「服務」頁籤
1. 選擇列出的唯一服務，然後按一下測試服務
1. 提供有效的leadId，然後按一下「測試」。 如果一切順利，您應該回到以下螢幕擷取中顯示的銷售線索詳細資料
   ![測試結果](assets/testresults.jfif)
