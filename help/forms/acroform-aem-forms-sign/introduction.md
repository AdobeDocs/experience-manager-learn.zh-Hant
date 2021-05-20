---
title: Acroforms與AEM Forms
description: 本教學課程會逐步說明如何使用Acroform建立適用性表單，以及合併資料以取得PDF。 接著，即可傳送包含合併資料的PDF，以使用Adobe Sign進行簽署。
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---


# 從Acroforms建立適用性Forms

組織形式多種多樣。 其中有些表單是以Microsoft Word建立，並轉換為PDF。 依預設，使用Adobe Reader或Acrobat無法填寫這些表單。 若要使用Acrobat或Reader填寫這些表單，我們需要將這些表單轉換為Acroform。 Acroforms是使用Acrobat建立的表單。 本文逐步說明如何從Acroform建立最適化表單，以及將資料合併回Acroform以取得PDF。 也可以傳送包含合併資料的PDF，以使用Adobe Sign進行簽署。

>[!NOTE]
>
>如果您使用AEM Forms 6.5，請使用Automated forms conversion功能。

## 必備條件

* AEM Forms 6.3或6.4已安裝及設定
* 存取Adobe Acrobat
* 熟悉AEM/AEM Forms。

### 以下是讓此功能在您的系統上運作的必要條件

* 使用[Felix Web Console](http://localhost:4502/system/console/bundles)下載並部署套件組合
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [將此套件下載並匯入AEM](assets/acro-form-aem-form.zip)。此套件包含從Acroform建立XSD的範例工作流程和html頁面
* 開啟[configMgr](http://localhost:4502/system/console/configMgr)
   * 搜尋「Apache Sling Service使用者對應程式服務」，然後按一下以開啟屬性
   * 按一下`+`圖示（加號）以新增下列服務對應
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 按一下「儲存」
