---
title: 在輸出服務中使用片段
description: 產生PDF檔案，其中片段位於crx存放庫中
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2022-07-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# 使用片段產生PDF檔案{#developing-with-output-and-forms-services-in-aem-forms}


在本文中，我們將使用輸出服務，使用xdp片段來產生pdf檔案。 主要xdp和片段位於crx存放庫。 務必在AEM中模擬檔案系統資料夾結構。 例如，如果您在xdp的片段資料夾中使用片段，則必須建立名為 **片段** 在AEM的基礎資料夾下。 基本資料夾將包含基本xdp範本。 例如，如果檔案系統上有下列結構
* c:\xdptemplates - This will contain your base xdp template
* c:\xdptemplates\fragments - This folder will contain fragments and the main template will reference the fragment as shown below
   ![fragment-xdp](assets/survey-fragment.png).
* 資料夾xdpdocuments將包含您的基本範本，以及 **片段** 資料夾

您可以使用 [表單和檔案ui](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

以下是範例xdp的資料夾結構，其使用2個片段
![表單&amp;文檔](assets/fragment-folder-structure-ui.png)


* 輸出服務 — 通常，此服務用於將xml資料與xdp範本合併，或以pdf產生平面化的pdf。 如需更多詳細資訊，請參閱 [javado](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) （輸出服務）。 在此範例中，我們使用的片段位於crx存放庫中。


下列程式碼用於在PDF檔案中包含片段

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

**在系統上測試示例包**

* [下載範例xdp檔案並匯入AEM](assets/xdp-templates-fragments.zip)
* [使用AEM套件管理器下載及安裝套件](assets/using-fragments-assets.zip)
* [範例xdp和片段可從此處下載](assets/xdptemplates.zip)

**安裝套件後，您必須在AdobeGranite CSRF篩選器中允許列出下列URL。**

1. 請依照下列步驟允許列出上述路徑。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增下列路徑並儲存
1. /content/AemFormsSamples/usingfragments

測試范常式式碼有許多方式。 最快、最簡單的方式是使用Postman應用程式。 Postman可讓您向伺服器提出POST請求。 在您的系統上安裝Postman應用程式。
啟動應用程式，然後輸入下列URL以測試匯出資料API

確認您已從下拉式清單http://localhost:4502/content/AemFormsSamples/usingfragments.html中選取「POST」。請確定您將「授權」指定為「基本驗證」。 指定AEM伺服器使用者名稱和密碼導覽至「Body」標籤，並指定要求參數，如下圖所示
![匯出](assets/using-fragment-postman.png)
然後按一下「傳送」按鈕

[您可以匯入此Postman集合以測試API](assets/usingfragments.postman_collection.json)
