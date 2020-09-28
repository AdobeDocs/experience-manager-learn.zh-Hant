---
title: 認證AEM表單中的檔案
seo-title: 認證AEM表單中的檔案
description: 使用Docassurance服務來認證AEM Forms中的PDF檔案
seo-description: 使用Docassurance服務來認證AEM Forms中的PDF檔案
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# 認證AEM表單中的檔案

「認證檔案」為PDF檔案和表單收件者提供其真實性和完整性的額外保證。

若要認證檔案，您可以在桌上型電腦或AEM Forms Document Services上使用Acrobat DC，做為伺服器上自動化程式的一部分。

本文提供您使用AEM Forms Document Services來認證PDF檔案的範例OSGI套件。範例中使用的程式碼可 [在這裡](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

若要使用AEM Forms來認證檔案，必須遵循下列步驟

## 新增憑證至信任存放區 {#adding-certificate-to-trust-store}

請依照下列步驟，將憑證新增至AEM的金鑰庫

* [初始化全域信任存放區](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜索fd-service用戶](http://localhost:4502/security/users.html)
* **您必須捲動結果頁面，以載入所有使用者以尋找fd-service使用者**
* 連按兩下fd-service使用者以開啟使用者設定視窗
* 按一下「從密鑰庫檔案添加私密密鑰」。指定證書專用的別名和密碼
   ![添加證書](assets/adding-certificate-keystore.PNG)
* 儲存變更

## 建立OSGI服務

您可以編寫自己的OSGi套件，並使用AEM Forms Client SDK來實作PDF檔案認證服務。 下列連結對於編寫您自己的OSGi套件非常有用

* [建立您的第一個OSGi套件](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用Document Service API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

或者，您也可以使用隨附的範例套裝，做為本教學課程資產的一部分。
>[!NOTE]
範例包使用別名&quot;ares&quot;來認證檔案。 因此，使用此捆綁包時，請確保您的別名為「ares」

## 在您的本機系統上測試範例

* 下載並安裝自 [訂檔案服務套裝](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下載並安裝使 [用服務使用者套件開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [請確定您已在Apache Sling Service User Mapper Service中新增下列項目](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** ，如以下螢幕擷取所示
   ![用戶映射器](assets/user-mapper-service.PNG)
* [匯入範例最適化表單](assets/certify-pdf-af.zip)
* [匯入並安裝自訂提交](assets/custom-submit-certify.zip)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上傳需要認證的PDF檔案
   **可選** -指定要用於認證文檔的簽名欄位
* 按一下「送出」。
* 認證的PDF應傳回給您。


