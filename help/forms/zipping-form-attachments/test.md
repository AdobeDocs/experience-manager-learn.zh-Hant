---
title: Test解決方案 — test兩種方法所需的步驟
description: Test解決方案，方法是將附件添加到表單並觸發工作流以發送電子郵件。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# test這兩種辦法所需的步驟

* 配置 [第CQ天郵件服務](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) 從AEM Forms伺服器發送電子郵件
* 部署 [格式附件](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) 束使用 [felix Web控制台](http://localhost:4502/system/console/bundles)

## 將zip檔案作為電子郵件附件發送



* 部署 [SendFormAttachmentsViaEmail工作流。](assets/zipped-form-attachments-model.zip) 此工作流使用發送電子郵件元件來發送zipped_attachments.zip檔案，該檔案通過自定義進程步驟保存在負載資料夾下。 根據您的需要配置發件人和收件人的電子郵件地址。
* 導入 [樣式](assets/zip-form-attachments-form.zip) 從 [Forms和文檔UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 並添加幾個附件並提交表格。
* 應觸發工作流，併發送包含zip檔案的電子郵件通知。

## 將表單附件作為單個檔案發送

* 部署 [SendForm工作流。](assets/send-form-attachments-model.zip) 此工作流使用發送電子郵件元件將表單附件作為單個檔案發送。 根據您的需要配置發件人和收件人電子郵件地址。
* 導入 [樣式](assets/send-list-attachments-form.zip) 從 [Forms和文檔UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽窗體](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) 並添加幾個附件並提交表格。
* 應觸發工作流，併發送帶有表單附件的電子郵件通知。
