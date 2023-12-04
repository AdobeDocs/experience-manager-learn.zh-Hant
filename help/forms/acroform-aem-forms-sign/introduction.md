---
title: 使用AEM Forms的Acroforms
description: 此教學課程會逐步引導您使用Acroform建立最適化表單，以及合併資料以取得PDF。 接著，合併資料的PDF便可使用Acrobat Sign傳送以供簽署。
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 69
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# 從Acroforms建立最適化Forms

組織有多種多樣的表格。 其中一些表單是在Microsoft Word中建立並轉換為PDF。 使用Adobe Reader或Acrobat時，預設無法填寫這些表單。 若要使用Acrobat或Reader讓這些表單可填寫，我們需要將這些表單轉換為Acroform。 Acroform是使用Acrobat建立的表單。 本文將逐步說明如何從Acroform建立最適化表單，並將資料合併回Acroform以取得PDF。 您也可以使用Acrobat Sign傳送包含合併資料的PDF以供簽署。

>[!NOTE]
>
>如果您使用AEM Forms 6.5，請使用Automated forms conversion功能。

## 先決條件

* 已安裝並設定AEM Forms 6.3或6.4
* 存取Adobe Acrobat
* 熟悉AEM/AEM Forms。

### 若要讓此功能在您的系統上運作，需具備下列條件

* 使用下載並部署套件組合 [Felix Web主控台](http://localhost:4502/system/console/bundles)
* [DocumentservicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [下載此套件並將其匯入至AEM](assets/acro-form-aem-form.zip). 此套件包含從Acroform建立XSD的工作流程範例和html頁面
* 開啟 [configMgr](http://localhost:4502/system/console/configMgr)
   * 搜尋「Apache Sling Service使用者對應程式服務」，然後按一下以開啟屬性
   * 按一下 `+` 圖示（加號）以新增以下服務對應
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 按一下「儲存」
