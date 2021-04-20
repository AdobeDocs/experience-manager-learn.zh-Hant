---
title: 送出以感謝頁面
seo-title: 送出以感謝頁面
description: 在提交最適化表單時顯示感謝頁面
seo-description: 在提交最適化表單時顯示感謝頁面
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# 送出以感謝頁面{#submitting-to-thank-you-page}

「提交到REST端點」選項會將表單中填入的資料傳遞至已設定的確認頁面，作為HTTPGET要求的一部分。 您可以新增要請求的欄位名稱。 請求的格式為：

\{fieldName\} = \{parameterName\}。 例如，submitterName是自適應表單域的名稱，submitter是參數的名稱。 在感謝頁中，您可以使用request.getParameter(&quot;submitter&quot;)訪問提交者參數，以獲取提交者名稱欄位的值。

submitterName=submitter

在以下的螢幕擷取中，我們會提交最適化表單，以感謝位於/content/thankyou的頁面。 在此感謝頁面中，我們傳遞了3個將包含表單欄位值的請求屬性。

![感謝](assets/thankyoupage.gif)

您也可以透過POST提交至外部端點。 若要完成此作業，您只需選取「啟用貼文要求」核取方塊，並提供外部端點的URL。 當您送出表格時，將會收到感謝頁面，並同時呼叫POST端點。

![捕獲](assets/capture.gif)


若要在您的伺服器上測試此功能，請依照下列指示進行：

* 使用套件管理員](assets/submittingtorestendpoint.zip)將與本文相關的[AEMassets檔案匯入
* 將瀏覽器指向[關閉請求表單](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫必填欄位並提交表格
* 您應該會收到感謝頁面，並在頁面中填入您的資訊

