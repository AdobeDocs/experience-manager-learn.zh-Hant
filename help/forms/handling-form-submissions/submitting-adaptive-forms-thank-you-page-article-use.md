---
title: 提交至感謝頁面
description: 在提交最適化表單時顯示感謝頁面
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 51
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 6%

---

# 提交至感謝頁面 {#submitting-to-thank-you-page}

「提交至REST端點」選項會將表單中填入的資料傳遞至已設定的確認頁面，作為HTTP GET請求的一部分。 您可以新增要求的欄位名稱。要求的格式為：

\{fieldName\} = \{parameterName\}。 例如，submitterName是調適型表單欄位的名稱，submitter是引數的名稱。 在感謝頁面中，您可以使用request.getParameter(「submitter」)存取提交者引數，以保留提交者名稱欄位的值。

`submitterName=submitter`

在下方熒幕擷圖中，我們提交最適化表單以感謝位於/content/thankyou的頁面。 在此感謝頁面中，我們將傳遞3個保留表單欄位值的請求屬性。

![感謝頁面](assets/thankyoupage.gif)

您也可以透過POST提交至外部端點。 若要完成此操作，您只需選取「啟用發佈請求」核取方塊，並提供外部端點的URL。 當您提交表單時，您會看到感謝頁面，並同時叫用POST端點。

![擷取組態](assets/capture.gif)

若要在您的伺服器上測試此功能，請遵循下列指示：

* 使用封裝管理員[&#128279;](assets/submittingtorestendpoint.zip)將與此文章相關的資產檔案匯入AEM
* 將瀏覽器指向[休假申請表](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫必填欄位並提交表單
* 您應該會收到感謝頁面，並在頁面上填入資訊
