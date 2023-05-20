---
title: 在自適應表單提交時發送電子郵件
seo-title: Sending Email on Adaptive Form Submission
description: 使用發送電子郵件元件在自適應表單提交時發送確認電子郵件
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

# 在自適應表單提交時發送電子郵件 {#sending-email-on-adaptive-form-submission}

常見操作之一是在成功提交Adaptive Form時向提交者發送確認電子郵件。 要完成此操作，我們將選擇「發送電子郵件」作為提交操作。

您可以使用電子郵件模板，或只鍵入電子郵件正文，如下面此螢幕快照所示。

請注意在電子郵件中插入表單域值的語法。我們還可以選擇在配置屬性中選中複選框「包括附件」將表單附件包含在電子郵件中。

提交自適應表單後，收件人將收到電子郵件。

![發送電子郵件](assets/sendemailaction.gif)

## 所需配置 {#configurations-needed}

您必須配置第CQ天郵件服務。 可通過將瀏覽器指向 [Felix配置管理器](http://localhost:4502/system/console/configMgr)

螢幕快照顯示Adobe郵件伺服器的配置屬性。

![郵件服務](assets/mailservice.png)

要在伺服器上嘗試此操作，請遵循以下說明：

* [導入資產](assets/timeoffrequest.zip) 與本文相關AEM聯。

* 開啟 [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)。

* 填寫詳細資訊。確保在電子郵件欄位中提供有效的電子郵件地址。

* 提交表單.
