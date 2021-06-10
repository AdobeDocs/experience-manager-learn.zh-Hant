---
title: 測試解決方案
description: 在表單中新增附件以測試解決方案，並觸發工作流程以傳送電子郵件。
sub-product: 表單
feature: 工作流程
topics: adaptive forms
audience: developer
doc-type: article
activity: develop
version: 6.5
topic: 開發
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 1%

---


# 測試解決方案


* 設定[Day CQ Mail Service](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service)以從AEM Forms伺服器傳送電子郵件
* 使用[felix web console](http://localhost:4502/system/console/bundles)部署[表單附件](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar)套件組合
* 部署[SendFormAttachmentsViaEmail工作流。](assets/zipped-form-attachments-model.zip) 此工作流程使用傳送電子郵件元件來傳送zipped_attachments.zip檔案，此檔案會由自訂處理步驟儲存在裝載資料夾下。根據您的需求配置發件人和收件人電子郵件地址。
* 從[Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)匯入[範例表單](assets/zip-form-attachments-form.zip)
* [預覽](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 表單並新增幾個附件並提交表單。
* 應會觸發工作流程，並傳送包含zip檔案的電子郵件通知。

