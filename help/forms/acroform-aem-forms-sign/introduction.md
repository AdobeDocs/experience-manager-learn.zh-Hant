---
title: AEM Forms
description: 一個教程，該教程使用Acroform建立自適應表單並合併資料以獲得PDF。 隨後，可以使用Acrobat Sign發送帶有合併資料的PDF以進行簽名。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# 從頂體建立自適應Forms

組織形式多種多樣。 其中一些表格在MicrosoftWord中建立並轉換為PDF。 預設情況下，這些表單不能使用Adobe Reader或Acrobat填寫。 要使這些表單可以使用Acrobat或Reader填充，我們需要將這些表單轉換為Acroform。 頂體是使用Acrobat建立的形式。 本文從Acroform中建立自適應表單，將資料合併回Acroform中，得到PDF。 還可以發送包含合併資料的PDF，以便使用Acrobat Sign簽名。

>[!NOTE]
>
>如果您使用AEM Forms6.5，請使用Automated forms conversion功能。

## 必備條件

* AEM Forms6.3或6.4已安裝並配置
* 訪問Adobe Acrobat
* 熟悉AEM/AEM Forms。

### 以下是使此功能在您的系統上運行所必需的

* 使用 [Felix Web控制台](http://localhost:4502/system/console/bundles)
* [文檔服務捆綁包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [下載並導入此包AEM](assets/acro-form-aem-form.zip)。 此包包含從頂層表單建立XSD的示例工作流和html頁
* 開啟 [configMgr](http://localhost:4502/system/console/configMgr)
   * 搜索「Apache Sling服務用戶映射器服務」並按一下以開啟屬性
   * 按一下 `+` 表徵圖（加號）以添加以下服務映射
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 按一下「保存」
