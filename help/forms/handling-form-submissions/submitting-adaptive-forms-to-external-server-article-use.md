---
title: 向外部伺服器提交最適化表單
seo-title: Submitting Adaptive Form to External Server
description: 將適用性表單提交到在外部伺服器上運行的REST端點
seo-description: Submitting Adaptive Form to REST endpoint running on external server
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# 向外部伺服器提交最適化表單 {#submitting-adaptive-form-to-external-server}

使用「提交至REST端點」動作，將提交的資料張貼至REST URL。 URL可以是內部（轉譯表單的伺服器）或外部伺服器。

通常客戶會想要將表單資料提交至外部伺服器，以進行進一步處理。

若要將資料發佈至內部伺服器，請提供資源的路徑。 資料會發佈在資源的路徑上。 例如， &lt;/content restendpoint=&quot;&quot;> . 對於這些帖子請求，使用提交請求的驗證資訊。

若要將資料發佈至外部伺服器，請提供URL。 URL的格式為 <http://host:port/path_to_rest_end_point>. 請確定您已設定以匿名方式處理POST要求的路徑。

為了本文的目的，我編寫了一個簡單的戰爭檔案，可以部署在您的tomcat實例上。 假設您的tomcat在埠8080上運行，則POSTurl將為

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

當您將適用性表單設定為提交至此端點時，表單資料和附件（如果有的話）可透過下列程式碼在servlet中擷取

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

![提交](assets/formsubmission.gif)
要在您的伺服器上測試，請執行以下操作

1. 如果尚未安裝Tomcat，請安裝它。 [此處提供安裝tomcat的說明](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. 下載 [zip檔案](assets/aemformsenablement.zip) 與此文章相關聯。 將檔案解壓縮以獲取戰爭檔案。
1. 在您的tomcat伺服器中部署war檔案。
1. 建立包含檔案附件元件的簡單適用性表單，並設定其提交動作，如上方螢幕擷取所示。 POSTURL為 <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. 如果您的AEM和tomcat未在localhost上執行，請據以變更URL。
1. 若要將多部分表單資料提交功能啟用到tomcat，請將以下屬性添加到的上下文元素中 &lt;tomcatinstalldir>\conf\context.xml ，然後重新啟動您的Tomcat伺服器。
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. 預覽最適化表單、新增附件及提交。 檢查tomcat控制台窗口中的消息。
