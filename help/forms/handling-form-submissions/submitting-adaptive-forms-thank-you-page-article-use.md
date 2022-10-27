---
title: 提交至感謝頁面
seo-title: Submitting To Thank You Page
description: 提交最適化表單時顯示感謝頁面
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
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 提交至感謝頁面 {#submitting-to-thank-you-page}

提交至REST端點選項會將表單中填入的資料傳遞至已設定的確認頁面，作為HTTPGET請求的一部分。 您可以新增要請求的欄位名稱。 請求的格式為：

\{fieldName\} = \{parameterName\}。 例如，submitterName是適用性表單欄位的名稱，submitter是參數的名稱。 在感謝頁中，您可以使用request.getParameter(&quot;submitter&quot;)訪問提交者參數，以獲取提交者名稱欄位的值。

`submitterName=submitter`

在下方的螢幕擷取中，我們會提交最適化表單以感謝位於/content/thankyou的頁面。 在此感謝頁面中，我們傳送了3個包含表單欄位值的請求屬性。

![感謝頁面](assets/thankyoupage.gif)

您也可以透過POST提交至外部端點。 若要完成此操作，您只需選取「啟用貼文要求」核取方塊，並提供外部端點的URL即可。 提交表單時，您會收到感謝頁面，並同時叫用POST端點。

![捕獲配置](assets/capture.gif)

若要在您的伺服器上測試此功能，請遵循下列指示：

* 匯入 [與此文章相關聯的資產檔案，請使用封裝管理程式將檔案放入AEM](assets/submittingtorestendpoint.zip)
* 將瀏覽器指向 [請求時間表](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫必填欄位並提交表單
* 您應該會在感謝頁面上填入您的資訊
