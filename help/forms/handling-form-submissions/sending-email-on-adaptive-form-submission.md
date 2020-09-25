---
title: 在最適化表單提交時傳送電子郵件
seo-title: 在最適化表單提交時傳送電子郵件
description: 使用傳送電子郵件元件，在自適應表單提交時傳送確認電子郵件
seo-description: 使用傳送電子郵件元件，在自適應表單提交時傳送確認電子郵件
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: adaptive-forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
translation-type: tm+mt
source-git-commit: 272e43ee4aeb756a3a1fd0ce55eaca94a9553fa4
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# 在最適化表單提交時傳送電子郵件 {#sending-email-on-adaptive-form-submission}

常見的操作之一是在成功提交Adaptive Form時向提交者發送確認電子郵件。 為完成此作業，我們會選取「傳送電子郵件」作為提交動作。

您可以使用電子郵件範本，或只輸入電子郵件的內文，如下面此螢幕擷取所示。

請注意在電子郵件中插入表格欄位值的語法。我們還可以選擇在配置屬性中選中「包含附件」複選框，將表格附件包含在電子郵件中。

提交最適化表單時，收件者會收到電子郵件。

![SendEmail](assets/sendemailaction.gif)

## 需要的配置 {#configurations-needed}

您必須設定Day CQ Mail服務。 您可將瀏覽器指向 [Felix Configuration Manager來設定此設定](http://localhost:4502/system/console/configMgr)

此螢幕擷取顯示adobe郵件伺服器的設定屬性。

![mailservice](assets/mailservice.png)

要在伺服器上試用此功能，請遵循以下說明：

* [使用套件管理器](assets/timeoffrequest.zip) ，在AEM中匯入與此文章關聯的資產。

* 開啟 [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。

* 填寫詳細資訊。請務必在電子郵件欄位中提供有效的電子郵件地址。

* 提交表格。
