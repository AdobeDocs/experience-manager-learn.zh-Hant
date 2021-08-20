---
title: AEM Forms的證明檔案
description: 使用Docassurance服務驗證AEM Forms中的PDF檔案
feature: 文件安全性
version: 6.4,6.5
topic: 開發
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---


# AEM Forms的證明檔案

認證檔案為PDF檔案和收件者提供其真實性和完整性的額外保證。

若要驗證檔案，您可以在案頭上使用Acrobat DC，或在伺服器上使用AEM Forms檔案服務，做為自動化程式的一部分。

本文提供您使用AEM Forms Document Services來認證PDF檔案的範例OSGI套件組合。範例中使用的程式碼為[，可在此取得](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

若要使用AEM Forms來驗證檔案，請遵循下列步驟

## 將證書添加到信任儲存 {#adding-certificate-to-trust-store}

請依照下列步驟，將憑證新增至AEM中的金鑰存放區

* [初始化全局信任儲存](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜索fd-](http://localhost:4502/security/users.html) serviceuser
* **您必須捲動結果頁面以載入所有使用者，才能尋找fd-service使用者**
* 按兩下fd-service用戶以開啟用戶設定窗口
* 按一下「從密鑰庫檔案添加私鑰」。指定證書特有的別名和密碼
   ![新增憑證](assets/adding-certificate-keystore.PNG)
* 儲存您的變更

## 建立OSGI服務

您可以撰寫自己的OSGi套件組合，並使用AEM Forms Client SDK實作服務以認證PDF檔案。 以下連結對於編寫您自己的OSGi套件會很有用

* [建立第一個OSGi捆綁包](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用檔案服務API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

或者，您也可以使用本教學課程資產中包含的範例套件組合。

>[!NOTE]
>
>範例套件組合使用名為「ares」的別名來驗證檔案。 因此，使用此捆綁包時，請確保您的別名稱為「ares」

## 在本機系統上測試樣本

* 下載並安裝[自定義文檔服務包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下載並安裝[使用服務用戶包開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [請務必在Apache Sling Service使用者對應程式服務中新增下列項目](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** service，如下方螢幕擷取所示
   ![User-Mapper](assets/user-mapper-service.PNG)
* [匯入範例最適化表單](assets/certify-pdf-af.zip)
* [匯入和安裝自訂提交](assets/custom-submit-certify.zip)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上傳需要認證的PDF檔案
   **可選**  — 指定要用於驗證文檔的簽名欄位
* 按一下提交。
* 認證的PDF應傳回給您。


