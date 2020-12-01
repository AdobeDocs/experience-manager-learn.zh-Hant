---
title: 在AEM Forms中使用Output和Forms Services進行開發
seo-title: 在AEM Forms中使用Output和Forms Services進行開發
description: 在AEM Forms中使用Output and Forms Service API
seo-description: 在AEM Forms中使用Output and Forms Service API
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '578'
ht-degree: 0%

---


# 在AEM Forms中使用Output and Forms Services進行開發{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用Output and Forms Service API

在本文中，我們將檢視下列

* 輸出服務——通常，此服務用於將xml資料與xdp範本或pdf合併，以產生平面化的pdf
* FormsService —— 這項多功能服務可讓您從PDF檔案匯出／匯入資料

AEM Forms API的正式Javadoc列在[這裡](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

下列程式碼片段會從PDF檔案匯出資料

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行從請求中擷取檔案

Line2從請求中提取saveLocation

第5行擁有FormsService

第6行從PDF檔案匯出xmlData

**在系統上測試示例包**

[使用AEM套件管理員下載並安裝套件](assets/outputandformsservice.zip)




**在安裝套件後，您必須允許在Adobe Granite CSRF Filter中列出下列URL。**

1. 請依照下列步驟，列出上述路徑。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Adobe Granite CSRF濾鏡
1. 在排除的區段中新增下列3個路徑並儲存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜尋「Sling Referrer篩選」
1. 選中「允許空」複選框。 （此設定應僅用於測試用途）
有許多方法可測試范常式式碼。 最快速最簡單的方式就是使用Postman應用程式。 郵遞員可讓您向伺服器提出POST要求。 在您的系統上安裝Postman應用程式。
啟動應用程式並輸入下列URL以測試匯出資料API

確定您已從下拉式清單中選取「POST」
http://localhost:4502/content/AemFormsSamples/exportdata.html
請確定您指定「授權」為「基本授權」。 指定AEM Server使用者名稱和密碼
導覽至「內文」標籤，並指定請求參數，如下圖所示
![export](assets/postexport.png)
然後按一下「傳送」按鈕

包含3個樣本。 以下各段說明何時使用輸出服務或Forms服務、服務的URL、輸入每個服務所需的參數

**合併資料並平面化輸出：**

* 使用Output Service將資料與xdp或pdf檔案合併，以產生平面化的pdf
* **貼文URL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **請求參數-**

   * xdp_or_pdf_file:您要將資料與
   * xmlfile:將與xdp_or_pdf_file合併的xml資料檔案
   * saveLocation:在檔案系統上保存所呈現文檔的位置

**將資料匯入PDF檔案：**
* 使用FormsService將資料匯入PDF檔案
* **貼文URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **請求參數：**

   * pdf檔案：您要將資料與
   * xmlfile:將與pdf檔案合併的xml資料檔案
   * saveLocation:在檔案系統上保存所呈現文檔的位置。 例如c:\\\outputsample.pdf。

**從PDF檔案匯出資料**
* 使用FormsService從PDF檔案匯出資料
* **貼文** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **請求參數：**

   * pdf檔案：您要從
   * saveLocation:在檔案系統上保存導出資料的位置

[您可以匯入此郵遞員系列來測試API](assets/document-services-postman-collection.json)

