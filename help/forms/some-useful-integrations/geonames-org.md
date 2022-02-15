---
title: 級聯下拉清單
description: 根據上一個下拉清單選擇填充下拉清單。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# 級聯下拉清單

級聯下拉清單是一系列從屬的DropDownList控制項，其中一個DropDownList控制項依賴於父級或以前的DropDownList控制項。 DropDownList控制項中的項是根據用戶從另一個DropDownList控制項中選擇的項來填充的。

## 演示使用案例

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=9&learn=on)

在本教程中，我使用 [Geonames REST API](http://api.geonames.org/) 來證明這種能力。
有許多組織提供此類服務，只要他們有詳細的REST API文檔，您就可以使用資料整合功能輕鬆與AEM Forms整合

按照以下步驟在AEM Forms實施級聯下拉清單

## 建立開發人員帳戶

建立開發人員帳戶 [吉奧納梅斯](https://www.geonames.org/login)。 記下用戶名。 調用geonames.org的REST API時需要此用戶名。

## 建立Swagger/OpenAPI檔案

OpenAPI規範（以前稱為Swagger規範）是REST API的API說明格式。 OpenAPI檔案允許您描述整個API，包括：

* 可用端點(/users)和每個端點上的操作(GET/users、POST/users)
* 操作參數每個操作的輸入和輸出身份驗證方法
* 聯繫資訊、許可證、使用條款和其他資訊。
* API規範可以用YAML或JSON編寫。 該格式易於學習，對人和機器都易讀。

要建立第一個swagger/OpenAPI檔案，請按照 [OpenAPI文檔](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規範2.0版(FKA Swagger)。

使用 [斯瓦格編輯器](https://editor.swagger.io/) 建立swagger檔案，以描述讀取國家/地區或州的所有國家/地區和子元素的操作。 可以使用JSON或YAML格式建立swagger檔案。 已完成的交換器檔案可從 [這裡](assets/swagger-files.zip)
交換器檔案描述以下REST API
* [獲取所有國家/地區](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [獲取Geoname對象的子項](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## 建立資料源

要將AEM/AEM Forms與第三方應用程式整合，我們需要 [建立資料源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲服務配置中。 請使用 [swagger檔案](assets/swagger-files.zip) 建立資料源。
您需要建立2個資料源（一個用於讀取所有國家/地區，另一個用於獲取子元素）


## 建立表單資料模型

AEM Forms資料整合提供直觀的用戶介面，用於建立和使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 將表單資料模型基於在前一步中建立的資料源。 具有2個資料源的表單資料模型

![fd](assets/geonames-fdm.png)


## 建立自適應窗體

將表單資料模型的GET調用與自適應表單整合，以填充下拉清單。
建立包含2個下拉清單的自適應窗體。 一個列出國家，一個列出根據選定國家而定的州/省。

### 填充國家/地區下拉清單

在首次初始化表單時填充國家清單。 以下螢幕抓圖顯示了配置為填充國家（地區）下拉清單選項的規則編輯器。 您必須向用戶名提供geonames帳戶才能使此帳戶正常工作。
![get國家](assets/get-countries-rule-editor.png)

#### 填充「省/市/自治區」下拉清單

我們需要根據所選的國家/地區填充「省/市/自治區」下拉清單。 以下螢幕抓圖顯示規則編輯器配置
![省/自治區選項](assets/state-province-options.png)

### 練習

在表格中添加2個名為「縣和市」的下拉清單，根據選定的國家和省列出縣和市。
![練](assets/cascading-drop-down-exercise.png)





