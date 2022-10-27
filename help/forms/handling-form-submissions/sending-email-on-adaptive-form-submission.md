---
title: 在適用性表單提交時傳送電子郵件
seo-title: Sending Email on Adaptive Form Submission
description: 使用傳送電子郵件元件，在最適化表單提交時傳送確認電子郵件
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 1%

---

# 在適用性表單提交時傳送電子郵件 {#sending-email-on-adaptive-form-submission}

常見的動作之一，是在成功提交適用性表單時，傳送確認電子郵件給提交者。 要完成此操作，我們會選取「傳送電子郵件」作為提交動作。

您可以使用電子郵件範本，或只是輸入電子郵件的內文，如下方螢幕擷取所示。

請注意在電子郵件中插入表單欄位值的語法。我們還可以選擇在配置屬性中選擇「包含附件」複選框，在電子郵件中包含表單附件。

提交適用性表單時，收件者會收到電子郵件。

![SendEmail](assets/sendemailaction.gif)

## 所需配置 {#configurations-needed}

您必須設定Day CQ Mail服務。 您可將瀏覽器指向 [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)

螢幕擷圖顯示adobe郵件伺服器的設定屬性。

![mailservice](assets/mailservice.png)

要在伺服器上嘗試，請按照以下說明操作：

* [匯入資產](assets/timeoffrequest.zip) 與本文相關聯(在AEM中)。

* 開啟 [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* 填寫詳細資訊。請務必在電子郵件欄位中提供有效的電子郵件地址。

* 提交表單.
