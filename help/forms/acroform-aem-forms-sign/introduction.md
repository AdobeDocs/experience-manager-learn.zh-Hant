---
title: Acroforms與AEM Forms
seo-title: 將最適化表單資料與Acroform合併
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 5eeeb197f9a2ee4216e1f9220c830751c36f01ab
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# 從Acroforms建立最適化表單

企業組織有各種不同的表單。 其中有些表格是在Microsoft Word中建立並轉換為PDF。 依預設，這些表單無法使用Adobe Reader或Acrobat填寫。 若要使用Acrobat或Reader填寫這些表格，我們必須將這些表格轉換為Acroform。 Acroforms是使用Acrobat建立的表格。 本文逐步從Acroform建立最適化表單，並將資料合併回Acroform以取得PDF。 您也可以使用Adobe Sign傳送包含合併資料的PDF以進行簽署。

>[!NOTE]
>
>如果您使用AEM Forms 6.5，請使用「自動化表單轉換」功能。

## 必備條件

* 已安裝並設定AEM Forms 6.3或6.4
* 存取Adobe Acrobat
* 熟悉AEM/AEM Forms。

### 要使此功能在您的系統上運作，需要以下各項

* 使用 [Felix Web Console下載和部署套件](http://localhost:4502/system/console/bundles)
* [DocumentServicesBundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [DevelopingWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AcroFormsToAEMFormsBundle](https://forms.enablementadobe.com/content/DemoServerBundles/AcroFormToAEMForm.core-1.0-SNAPSHOT.jar)
* [下載並匯入此套件至AEM](assets/acro-form-aem-form.zip)。 此套件包含從Acroform建立XSD的範例工作流程和html頁面
* 開啟 [configMgr](http://localhost:4502/system/console/configMgr)
   * 搜尋&#39;Apache Sling Service User Mapper Service&#39;，然後按一下以開啟屬性
   * 按一下 `+` 圖示（加號）以新增下列服務對應
      * `DevelopingWithServiceUser.core:getresourceresolver=data`
      * `DevelopingWithServiceUser.core:getformsresourceresolver=fd-service`
   * 按一下「儲存」
