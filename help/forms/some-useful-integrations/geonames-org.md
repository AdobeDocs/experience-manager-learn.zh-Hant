---
title: 階層式下拉式清單
description: 根據上一個下拉式清單選取項目填入下拉式清單。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# 階層式下拉式清單

級聯下拉清單是一系列相依的DropDownList控制項，其中一個DropDownList控制項取決於父項或前一個DropDownList控制項。 DropDownList控制項中的項根據用戶從另一個DropDownList控制項中選擇的項來填充。

## 使用案例的展示

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

在本教學課程中，我已使用 [Geonames REST API](http://api.geonames.org/) 來展示此功能。
有許多組織提供這類服務，只要他們有記錄完善的REST API，您就可以透過資料整合功能，輕鬆與AEM Forms整合

請依照下列步驟，在AEM Forms中實作階層式下拉式清單

## 建立開發人員帳戶

使用建立開發人員帳戶 [蓋納梅斯](https://www.geonames.org/login). 記下使用者名稱。 需要此使用者名稱，才能叫用geonames.org的REST API。

## 建立Swagger/OpenAPI檔案

OpenAPI規格（原稱Swagger規格）是REST API的API說明格式。 OpenAPI檔案可讓您描述整個API，包括：

* 每個端點上的可用端點(/users)和操作(GET/users、POST/users)
* 操作參數每個操作的輸入和輸出身份驗證方法
* 聯絡資訊、授權、使用條款和其他資訊。
* API規格可以用YAML或JSON編寫。 該格式便於人和機器學習和閱讀。

若要建立您的第一個swagger/OpenAPI檔案，請遵循 [OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(FKA Swagger)。

使用 [swagger編輯器](https://editor.swagger.io/) 建立swagger檔案，以說明擷取國家/地區或州的所有國家/地區和子項元素的作業。 可以使用JSON或YAML格式建立Swagger檔案。 已完成的Swagger檔案可從 [此處](assets/swagger-files.zip)
Swagger檔案說明下列REST API
* [取得所有國家/地區](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [獲取Geoname對象的子項](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## 建立資料來源

若要將AEM/AEM Forms與協力廠商應用程式整合，我們需要 [建立資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲端服務設定中。 請使用 [swagger檔案](assets/swagger-files.zip) 來建立資料來源。
您需要建立2個資料來源（一個用來擷取所有國家/地區，其他則用來取得子元素）


## 建立表單資料模型

AEM Forms資料整合提供直覺式的使用者介面，讓您建立及使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 將表單資料模型建立在先前步驟中建立的資料來源上。 具有2個資料源的表單資料模型

![fdm](assets/geonames-fdm.png)


## 建立最適化表單

將表單資料模型的GET叫用與最適化表單整合，以填入下拉式清單。
建立具有2個下拉式清單的最適化表單。 一個列出國家，一個列出各州/省，具體取決於所選國家。

### 填入國家/地區下拉式清單

首次初始化表單時，會填入國家/地區清單。 下列螢幕擷取畫面顯示設定為填入國家/地區下拉式清單選項的規則編輯器。 您必須提供使用者名稱及地名帳戶，才能順利運作。
![get-countries](assets/get-countries-rule-editor.png)

#### 填入州/省下拉式清單

我們需要根據所選國家填入州/省下拉清單。 下列螢幕擷取畫面會顯示規則編輯器設定
![state-province-options](assets/state-province-options.png)

### 練習

在表格中新增2個名為「縣市」的下拉式清單，以根據所選國家/地區和州/省列出縣和市。
![練習](assets/cascading-drop-down-exercise.png)
