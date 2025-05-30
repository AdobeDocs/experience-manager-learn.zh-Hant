---
title: 設定表單資料模型
description: 根據RDBMS資料來源建立表單資料模型
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
duration: 103
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 0%

---

# 設定表單資料模型

## Apache Sling連線集區資料來源

建立RDBMS支援表單資料模型的第一步是設定Apache Sling Connection Pooled DataSource。 若要設定資料來源，請遵循下列步驟：

* 將瀏覽器指向[configMgr](http://localhost:4502/system/console/configMgr)
* 搜尋&#x200B;**Apache Sling Connection Pooled DataSource**
* 新增專案並提供如熒幕擷圖所示的值。
* ![資料來源](assets/data-source.png)
* 儲存您的變更

>[!NOTE]
>JDBC連線URI、使用者名稱和密碼會依據您的MySQL資料庫組態而變更。


## 建立表單資料模型

* 將瀏覽器指向[資料整合](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* 按一下&#x200B;_建立_->_表單資料模型_
* 為表單資料模型提供有意義的名稱和標題，例如&#x200B;**員工**
* 按一下&#x200B;_下一步_
* 選取在先前區段（論壇）建立的資料來源
* 按一下[建立&#x200B;_]->[編輯]以編輯模式開啟新建立的表單資料模型_
* 展開&#x200B;_論壇_&#x200B;節點以檢視員工結構描述。 展開員工節點以檢視2個表格

## 將實體新增至模型

* 請確定員工節點已展開
* 選取新的和受益人實體，然後按一下&#x200B;_新增選取的專案_

## 將讀取服務新增至新實體

* 選取新實體
* 按一下&#x200B;_編輯屬性_
* 從讀取服務下拉式清單中選取get
* 按一下+圖示以新增引數至get服務
* 指定如熒幕擷圖所示的值
* ![取得服務](assets/get-service.png)
>[!NOTE]
> Get服務需要對應到newhire實體之empID欄的值。傳遞此值的方法有多種，在本教學課程中，empID會透過稱為empID的請求引數傳遞。
>* 按一下&#x200B;_完成_&#x200B;以儲存get服務的引數
>* 按一下&#x200B;_完成_&#x200B;以儲存表單資料模型的變更

## 在2個實體之間新增關聯

在表單資料模型中不會自動建立資料庫實體之間定義的關聯。 實體之間的關聯需要使用表單資料模型編輯器來定義。 每個新實體可以有一或多個受益人，我們需要定義新實體與受益人實體之間的一對多關聯。
下列步驟將引導您完成建立一對多關聯的過程

* 選取newhire實體並按一下&#x200B;_新增關聯_
* 提供有意義的標題和識別碼給關聯和其他屬性，如下方熒幕擷圖所示
  ![關聯](assets/association-entities-1.png)

* 按一下「引數」區段下的&#x200B;_編輯_&#x200B;圖示

* 指定值，如此熒幕擷取畫面所示
* ![關聯–2](assets/association-entities.png)
* **我們正在使用受益人與newhire實體的empID欄連結兩個實體。**
* 按一下&#x200B;_完成_&#x200B;以儲存變更

## 測試您的表單資料模型

我們的表單資料模型現在有&#x200B;**_get_**&#x200B;服務，該服務接受empID並傳回新貴及其受益人的詳細資料。 若要測試取得服務，請遵循下列步驟。

* 選取新實體
* 按一下&#x200B;_測試模型物件_
* 提供有效的empID並按一下&#x200B;_測試_
* 您應該會得到如下方熒幕擷取畫面所示的結果
* ![test-fdm](assets/test-form-data-model.png)

## 後續步驟

[從URL取得empID](./get-request-parameter.md)