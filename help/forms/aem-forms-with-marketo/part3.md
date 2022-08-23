---
title: AEM Forms與Marketo（下）
description: 使用AEM Forms表單資料模型將Marketo與AEM Forms整合的教程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 1%

---

# 配置資料源

AEM Forms資料整合允許您配置和連接到不同的資料源。 支援開箱即用的以下類型。 但是，通過進行少量定製，您也可以與其他資料源整合。

1. 關係資料庫 — MySQL、MicrosoftSQL Server、IBMDB2和OracleRDBMS
1. AEM用戶配置檔案
1. REST風格的Web服務
1. 基於SOAP的Web服務
1. OData服務

對於AEM Forms與Marketo的整合，我們將使用REST風格的Web服務。 整合的第一步是配置 [資料源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 請使用本教程中提供的swagger檔案。 以下螢幕快照顯示配置資料源時需要指定的重要屬性。
![資料源](assets/datasource.jfif)

「marketo.json」是swagger檔案，作為本教程資產的一部分提供給您。
屬性主機特定於您的Marketo實例。
身份驗證類型是自定義的，身份驗證實施必須與「AemForms與Marketo」相匹配。 （除非您在代碼中更改了此項）。

## 建立表單資料模型

之後，配置資料源的下一步是建立基於前面步驟中配置的資料源的表單資料模型。 要建立表單資料模型，請執行以下步驟：

將瀏覽器指向 [資料整合頁。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 這將列出在實例上建立的所有資料AEM整合。

1. 按一下建立 |窗體資料模型
1. 提供有意義的標題，如FormsAndMarketo，然後按一下「下一步」
1. 選擇在前一步中配置的資料源，然後按一下建立和編輯以在編輯模式下開啟表單資料模型
1. 展開「FormsAndMarketo」節點。 展開「服務」節點
1. 選擇第一個「獲取」操作
1. 按一下添加選定項
1. 在「添加關聯的模型對象」對話框中按一下「全選」，然後按一下「添加」
1. 按一下「保存」按鈕保存表單資料模型
1. 「服務」頁籤
1. 選擇列出的唯一服務，然後按一下Test服務
1. 提供有效的leadId並按一下Test。 如果一切順利，您應返回以下螢幕快照中所示的潛在客戶詳細資訊
   ![測試結果](assets/testresults.jfif)
