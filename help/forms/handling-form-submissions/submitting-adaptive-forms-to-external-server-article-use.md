---
title: 提交最適化表單至外部伺服器
description: 提交最適化表單至在外部伺服器上執行的REST端點
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 87
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 12%

---

# 提交最適化表單至外部伺服器 {#submitting-adaptive-form-to-external-server}

使用「提交至REST端點」動作，將提交的資料發佈至REST URL。 該 URL 可以是內部伺服器 (呈現表單的伺服器) 或外部伺服器。

通常客戶會想要將表單資料提交至外部伺服器，以便進一步處理。

若要將資料發佈到內部伺服器，請提供資源的路徑。 資料會發佈到資源的路徑。例如， &lt;/content restendpoint=&quot;&quot;> . 對於此類發佈請求，會使用提交請求的驗證資訊。

若要將資料發佈到外部伺服器，請提供 URL。URL 的格式是：<http://host:port/path_to_rest_end_point>。確保您已設定匿名處理POST請求的路徑。

為了撰寫本文的目的，我撰寫了一個簡單的war檔案，可部署在您的tomcat執行個體上。 假設您的tomcat正在連線埠8080上執行，則POSTURL將為

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

當您設定最適化表單以提交至此端點時，表單資料和附件（如有）可透過以下程式碼擷取到servlet中

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![表單提交](assets/formsubmission.gif)
若要在您的伺服器上測試此專案，請執行下列動作

1. 請安裝Tomcat （如果尚未安裝）。 [此處提供安裝tomcat的說明](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. 下載 [zip檔案](assets/aemformsenablement.zip) 與此文章相關聯。 解壓縮檔案以取得war檔案。
1. 在您的tomcat伺服器中部署war檔案。
1. 建立具有檔案附件元件的簡單調適型表單，並設定其提交動作，如上方熒幕擷圖所示。 POSTURL為 <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. 如果您的AEM和tomcat未在localhost上執行，請適當的變更URL。
1. 若要啟用將多部分表單資料提交至tomcat的功能，請將下列屬性新增至 &lt;tomcatinstalldir>\conf\context.xml並重新啟動Tomcat伺服器。
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. 預覽最適化表單、新增附件並提交。 檢查tomcat主控台視窗中的訊息。
