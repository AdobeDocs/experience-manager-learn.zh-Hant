---
title: 在適用性表單提交時傳送電子郵件
seo-title: 在適用性表單提交時傳送電子郵件
description: 使用傳送電子郵件元件，在最適化表單提交時傳送確認電子郵件
seo-description: 使用傳送電子郵件元件，在最適化表單提交時傳送確認電子郵件
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: 適用性表單
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 1%

---


# 在適用性表單提交時傳送電子郵件 {#sending-email-on-adaptive-form-submission}

常見的動作之一，是在成功提交適用性表單時，傳送確認電子郵件給提交者。 要完成此操作，我們會選取「傳送電子郵件」作為提交動作。

您可以使用電子郵件範本，或只是輸入電子郵件的內文，如下方螢幕擷取所示。

請注意在電子郵件中插入表單欄位值的語法。我們還可以選擇在配置屬性中選擇「包含附件」複選框，在電子郵件中包含表單附件。

提交適用性表單時，收件者會收到電子郵件。

![SendEmail](assets/sendemailaction.gif)

## 所需配置 {#configurations-needed}

您必須設定Day CQ Mail服務。 您可將瀏覽器指向[Felix Configuration Manager](http://localhost:4502/system/console/configMgr)進行設定

螢幕擷圖顯示adobe郵件伺服器的設定屬性。

![mailservice](assets/mailservice.png)

要在伺服器上嘗試，請按照以下說明操作：

* [使用套](assets/timeoffrequest.zip) 件管理員，在AEM中匯入與本文相關聯的資產。

* 開啟[TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。

* 填寫詳細資訊。請務必在電子郵件欄位中提供有效的電子郵件地址。

* 提交表單。
