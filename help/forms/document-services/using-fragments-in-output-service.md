---
title: 在輸出服務中使用片段
description: 產生PDF檔案，其片段位於crx存放庫中
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
exl-id: d7887e2e-c2d4-4f0c-b117-ba7c41ea539a
duration: 106
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# 使用片段產生pdf檔案{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我們將使用輸出服務產生使用xdp片段的pdf檔案。 主要xdp和片段位於crx存放庫中。 在AEM中模擬檔案系統資料夾結構是很重要的事。 例如，如果您在xdp的片段資料夾中使用片段，您必須在AEM中的基底資料夾下建立名為&#x200B;**fragments**&#x200B;的資料夾。 基底資料夾將包含您的基底xdp範本。 例如，如果您的檔案系統有下列結構
* c：\xdptemplates — 這將包含您的基本xdp範本
* c：\xdptemplates\fragments — 此資料夾將包含片段，主要範本將參考如下所示的片段
  ![片段 — xdp](assets/survey-fragment.png)。
* 資料夾xdpdocuments將包含您的基礎範本和&#x200B;**片段**&#x200B;資料夾中的片段

您可以使用[表單和檔案ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)來建立必要的結構

以下是使用2個片段的範例xdp的資料夾結構
![表單檔案](assets/fragment-folder-structure-ui.png)


* 輸出服務 — 通常此服務用於合併xml資料與xdp範本或pdf以產生平面化pdf。 如需詳細資訊，請參閱輸出服務的[javadoc](https://helpx.adobe.com/tw/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)。 在此範例中，我們使用crx存放庫中的片段。


下列程式碼是用來在PDF檔案中包含片段

```java
System.out.println("I am in using fragments POST.jsp");
// contentRootURI is the base folder. All fragments are relative to this folder
String contentRootURI = request.getParameter("contentRootURI");
String xdpName = request.getParameter("xdpName");
javax.servlet.http.Part xmlDataPart = request.getPart("xmlDataFile");
System.out.println("Got xml file");
String filePath = request.getParameter("saveLocation");
java.io.InputStream xmlIS = xmlDataPart.getInputStream();
com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlIS);
com.adobe.fd.output.api.OutputService outputService = sling.getService(com.adobe.fd.output.api.OutputService.class);

if (outputService == null) {
  System.out.println("The output service is  null.....");
} else {
  System.out.println("The output service is  not null.....");

}
com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);

pdfOptions.setContentRoot(contentRootURI);

com.adobe.aemfd.docmanager.Document generatedDocument = outputService.generatePDFOutput(xdpName, xmlDocument, pdfOptions);
generatedDocument.copyToFile(new java.io.File(filePath));
out.println("Document genreated and saved to " + filePath);
```

**若要在您的系統上測試範例封裝**

* [下載範例xdp檔案並將其匯入至AEM](assets/xdp-templates-fragments.zip)
* [使用AEM封裝管理員下載並安裝封裝](assets/using-fragments-assets.zip)
* [範例xdp和片段可以從這裡下載](assets/xdptemplates.zip)

**安裝套件後，您必須在Adobe Granite CSRF篩選器中允許列出下列URL。**

1. 請依照下列步驟操作，將上述路徑加入允許清單。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Adobe Granite CSRF篩選器
1. 在排除的區段中新增以下路徑並儲存
1. /content/AemFormsSamples/usingfragments

測試範常式式碼的方法有很多種。 最快捷、最輕鬆的方式就是使用Postman應用程式。 Postman可讓您向伺服器發出POST要求。 在您的系統上安裝Postman app 。
啟動應用程式並輸入以下URL以測試匯出資料API

確定您已從下拉式清單中選取「POST」
http://localhost:4502/content/AemFormsSamples/usingfragments.html
請務必將「授權」指定為「基本驗證」。 指定AEM伺服器使用者名稱和密碼
導覽至「內文」標籤，並指定請求引數，如下圖所示
![匯出](assets/using-fragment-postman.png)
然後按一下「傳送」按鈕

[您可以匯入此Postman集合以測試API](assets/usingfragments.postman_collection.json)
