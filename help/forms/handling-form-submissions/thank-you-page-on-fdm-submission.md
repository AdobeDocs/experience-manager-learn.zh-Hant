---
title: 在表單提交上顯示提交ID
description: 在感謝頁面中顯示表單資料模型提交的回應
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13900
last-substantial-update: 2023-09-09T00:00:00Z
exl-id: 18648914-91cc-470d-8f27-30b750eb2f32
duration: 72
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 自訂感謝頁面

將最適化表單提交至REST端點時，您會想要顯示確認訊息，讓使用者知道已成功提交表單。 POST回應包含提交的詳細資訊，例如提交ID，而設計良好的確認訊息則包含有助於提供更佳使用者體驗的提交ID。 此回應會顯示於已設定最適化表單的感謝頁面中。

以下熒幕擷圖顯示正在使用表單資料模型提交動作提交表單，並設定感謝頁面

![感謝頁面](./assets/thank-you-page-fdm-submit.png)

表單資料模型的POST一律會在回應中傳回JSON物件。 此JSON可在「感謝您」頁面URL中作為稱為&#x200B;_fdmSubmitResult_的查詢引數使用。 您可以剖析此查詢引數，並在感謝頁面中顯示JSON元素。
下列範常式式碼會剖析JSON回應以擷取數字欄位的值。 接著會建構適當的xml，並在slingRequest中傳遞以填入表單。 此程式碼通常會以與調適型表單範本相關聯的頁面元件的jsp撰寫。

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

建議您以新的最適化表單範本作為感謝頁面的基礎，此範本可讓您編寫自訂程式碼，以從查詢引數中擷取回應。

## 測試解決方案

建立最適化表單，並設定以使用表單資料模型提交動作來提交表單。
[部署範例最適化表單範本](assets/thank-you-page-template.zip)
根據此範本建立感謝表單
將此感謝頁面與主要表單建立關聯
修改[createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp)中的jsp程式碼以建置預填最適化表單所需的xml。
預覽並提交您的最適化表單。
此時應會顯示感謝頁面，並預先填入XML中指定的資料
