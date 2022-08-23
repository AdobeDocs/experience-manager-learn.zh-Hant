---
title: 「提交至感謝」頁
seo-title: Submitting To Thank You Page
description: 在提交自適應表單時顯示感謝頁
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 0%

---

# 「提交至感謝」頁 {#submitting-to-thank-you-page}

作為HTTPGET請求的一部分，「提交到REST端點」選項會將表單中填寫的資料傳遞到配置的確認頁。 您可以添加要請求的欄位的名稱。 請求的格式為：

\{fieldName\} = \{parameterName\}。 例如，submitterName是自適應表單域的名稱，submitter是參數的名稱。 在「感謝」頁中，您可以使用request.getParameter(&quot;submitter&quot;)訪問提交者參數，以獲取提交者名稱欄位的值。

提交者名稱=提交者

在下面的螢幕截圖中，我們將提交自適應表單以感謝您，該頁位於/content/tankyou。 在此感謝頁面中，我們傳遞了3個請求屬性，這些屬性將保留表單欄位值。

![謝謝](assets/thankyoupage.gif)

您還可以通過POST提交到外部端點。 要完成此操作，您只需選中「啟用後請求」複選框並提供外部終結點的URL。 當您提交表單時，您將收到「感謝」頁，並同時調用POST終結點。

![捕獲](assets/capture.gif)


要在伺服器上test此功能，請按照以下說明操作：

* 導入 [與此文章關聯的資產文AEM件使用包管理器](assets/submittingtorestendpoint.zip)
* 將瀏覽器指向 [請求表格的截止時間](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫必填欄位並提交表單
* 您應在「感謝」頁面中填入您的資訊
