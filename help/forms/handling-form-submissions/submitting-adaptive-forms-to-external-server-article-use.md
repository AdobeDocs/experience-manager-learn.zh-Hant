---
title: 向外部伺服器提交Adaptive表單
seo-title: Submitting Adaptive Form to External Server
description: 將Adaptive Form提交到在外部伺服器上運行的REST終結點
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
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# 向外部伺服器提交Adaptive表單 {#submitting-adaptive-form-to-external-server}

使用「提交到REST終結點」(Submit to REST Endpoint)操作將提交的資料發佈到REST URL。 該URL可以是內部（其上呈現表單的伺服器）或外部伺服器。

通常，客戶希望將表單資料提交到外部伺服器以進行進一步處理。

要將資料發佈到內部伺服器，請提供資源路徑。 資料將發佈到資源的路徑。 比如說， &lt;/content restendpoint=&quot;&quot;> 。 對於這種帖子請求，使用提交請求的驗證資訊。

要將資料發佈到外部伺服器，請提供URL。 URL的格式為 <http://host:port/path_to_rest_end_point>。 確保已配置路徑以匿名處理POST請求。

為了本文的目的，我編寫了一個簡單的戰爭檔案，可以部署在您的tomcat實例上。 假設tomcat在埠8080上運行，POSTurl將

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

將自適應表單配置為提交到此終結點時，表單資料和附件（如果有）可通過以下代碼在servlet中提取

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
要在伺服器上test此項，請執行以下操作

1. 如果尚未安裝Tomcat，請安裝它。 [此處提供了安裝tomcat的說明](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. 下載 [zip檔案](assets/aemformsenablement.zip) 與本文關聯。 解壓縮檔案以獲取戰爭檔案。
1. 在tomcat伺服器中部署war檔案。
1. 使用檔案附件元件建立一個簡單的自適應表單，並配置其提交操作，如上面的螢幕快照所示。 POSTURL為 <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>。 如果AEM您的和tomcat未在localhost上運行，請相應更改URL。
1. 要啟用向tomcat提交多部件表單資料，請將以下屬性添加到 &lt;tomcatinstalldir>\conf\context.xml並重新啟動Tomcat伺服器。
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. 預覽自適應表單，添加附件並提交。 檢查tomcat控制台窗口中的消息。
