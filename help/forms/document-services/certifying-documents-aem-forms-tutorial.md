---
title: AEM Forms證明檔案
description: 使用代理保險服務證明AEM Forms的PDF文檔
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# AEM Forms證明檔案

認證文檔向PDF文檔和表單收件人提供對其真實性和完整性的附加保證。

要驗證文檔，您可以在案頭或AEM Forms文檔服務上使用Acrobat DC作為伺服器上自動化流程的一部分。

本文提供OSGI捆綁包示例，以使用AEM Forms文檔服務驗證pdf文檔。示例中使用的代碼是 [此處](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

要驗證使用AEM Forms的文檔，需要執行以下步驟

## 正在向信任儲存添加證書 {#adding-certificate-to-trust-store}

請按照以下步驟將證書添加到密鑰庫AEM

* [初始化全局信任儲存](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜索fd-service](http://localhost:4502/security/users.html) 用戶
* **您必須滾動結果頁以載入所有用戶以查找fd-service用戶**
* 按兩下fd-service用戶以開啟用戶設定窗口
* 按一下「從密鑰庫檔案添加私鑰」。指定特定於證書的別名和口令
   ![添加證書](assets/adding-certificate-keystore.PNG)
* 保存更改

## 建立OSGI服務

您可以編寫自己的OSGi捆綁包，並使用AEM Forms客戶端SDK實現一項服務來驗證PDF文檔。 以下連結對編寫您自己的OSGi捆綁包非常有用

* [建立第一個OSGi捆綁包](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用文檔服務API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

或者，可以將包含的示例捆綁包用作本教程資產的一部分。

>[!NOTE]
>
>示例捆綁包使用名為「ares」的別名來驗證文檔。 因此，使用此捆綁包時，請確保您的別名稱為「ares」

## 在本地系統上測試示例

* 下載並安裝 [自定義文檔服務包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下載並安裝 [使用服務用戶包開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [確保已在Apache Sling服務用戶映射器服務中添加以下條目](http://localhost:4502/system/console/configMgr)

   **DevegingWithServiceUser.core:getformsresourceresolver=fd-service** 如下面的螢幕截圖所示
   ![用戶映射器](assets/user-mapper-service.PNG)
* [導入示例自適應窗體](assets/certify-pdf-af.zip)
* [導入並安裝自定義提交](assets/custom-submit-certify.zip)
* [開啟自適應窗體](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上傳需要認證的PDF文檔
   **可選**  — 指定要在驗證文檔時使用的簽名欄位
* 按一下提交。
* 應將認證PDF退還給您。
