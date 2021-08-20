---
title: 測試解決方案
description: 在表單中新增附件以測試解決方案，並觸發工作流程以傳送電子郵件。
feature: 適用性表單
version: 6.5
topic: 開發
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# 測試兩種方法所需的步驟

* 設定[Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service)以從AEM Forms伺服器傳送電子郵件
* 使用[felix web console](http://localhost:4502/system/console/bundles)部署[表單附件](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar)套件組合

## 以電子郵件附件的形式傳送zip檔案



* 部署[SendFormAttachmentsViaEmail工作流。](assets/zipped-form-attachments-model.zip) 此工作流程使用傳送電子郵件元件來傳送zipped_attachments.zip檔案，此檔案會由自訂處理步驟儲存在裝載資料夾下。根據您的需求配置發件人和收件人的電子郵件地址。
* 從[Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[範例表單](assets/zip-form-attachments-form.zip)
* [預覽](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 表單並新增幾個附件並提交表單。
* 應會觸發工作流程，並傳送包含zip檔案的電子郵件通知。

## 以個別檔案的形式傳送表單附件

* 部署[SendForm工作流。](assets/send-form-attachments-model.zip) 此工作流程使用傳送電子郵件元件，以個別檔案的形式傳送表單附件。根據您的需求配置發件人和收件人電子郵件地址。
* 從[Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[範例表單](assets/send-list-attachments-form.zip)
* [預覽](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) 表單並新增幾個附件並提交表單。
* 應會觸發工作流程，並傳送包含表單附件的電子郵件通知。
