---
title: AEM Forms與Marketo（第3部分）
description: 使用AEM Forms表單資料模型整合AEM Forms與Marketo的教學課程。
feature: 適用性Forms，表單資料模型
version: 6.3,6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '385'
ht-degree: 1%

---


# 配置資料源

AEM Forms資料整合可讓您設定並連線至不同的資料來源。 下列類型可立即使用。 不過，透過一些自訂功能，您也可以與其他資料來源整合。

1. 關係資料庫 — MySQL、Microsoft SQL Server、IBM DB2和OracleRDBMS。
1. AEM使用者設定檔
1. RESTful Web服務
1. 基於SOAP的Web服務
1. OData服務

為整合AEM Forms與Marketo，我們將使用RESTful網站服務。 整合的第一步是設定[資料來源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 請使用本教學課程中提供的Swagger檔案。以下螢幕截圖顯示配置資料源時需要指定的重要屬性。
![資料來源](assets/datasource.jfif)

「marketo.json」是Swagger檔案，會隨本教學課程的資產提供給您。
屬性主機是您的Marketo執行個體專屬的。
驗證類型為自訂類型，且驗證實作必須符合「AemForms與Marketo」。 （除非您已在程式碼中變更此項目）。

## 建立表單資料模型

之後，設定資料來源的下一步是建立表單資料模型，此模型以先前步驟中設定的資料來源為基礎。 要建立表單資料模型，請執行以下步驟：

將瀏覽器指向[資料整合頁面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 這會列出您AEM例項上建立的所有資料整合。

1. 按一下建立 |表單資料模型
1. 提供有意義的標題，例如FormsAndMarketo，然後按一下「下一步」
1. 選取先前步驟中設定的資料來源，然後按一下「建立」並編輯，以在編輯模式中開啟「表單資料模型」
1. 展開「FormsAndMarketo」節點。 展開「服務」節點
1. 選擇第一個「獲取」操作
1. 按一下新增選取的項目
1. 按一下「添加關聯模型對象」(Add Associated Model Objects)對話框中的「全部選擇」(Select All)，然後按一下「添加」(Add)
1. 按一下「儲存」按鈕，儲存表單資料模型
1. 頁簽到「服務」頁簽
1. 選擇所列的唯一服務，然後按一下測試服務
1. 提供有效的leadId，然後按一下測試。 如果一切順利，您應該會傳回潛在客戶詳細資訊，如下方螢幕擷取畫面所示
   ![測試結果](assets/testresults.jfif)
