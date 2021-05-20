---
title: 在AEM Forms與產出和Forms服務一起發展
seo-title: 在AEM Forms與產出和Forms服務一起發展
description: 在AEM Forms中使用輸出和Forms服務API
seo-description: 在AEM Forms中使用輸出和Forms服務API
uuid: be018eb5-dbe7-4101-a1a9-bee11ac97273
feature: 輸出服務
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 57f478a9-8495-469e-8a06-ce1251172fda
topic: 開發
role: Developer
level: Intermediate
source-git-commit: 67be45dbd72a8af8b9ab60452ff15081c6f9f192
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 0%

---


# 在AEM Forms中與產出和Forms服務一起開發{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用輸出和Forms服務API

在本文中，我們將審視以下內容

* 輸出服務 — 通常，此服務用於將xml資料與xdp範本合併，或以pdf產生平面化的pdf。 有關更多詳細資訊，請參閱輸出服務的[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)。
* FormsService — 這項服務用途廣泛，可讓您從PDF檔案匯出/匯入資料。 有關更多詳細資訊，請參閱Forms服務的[javadoc](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html)。


下列程式碼片段會從PDF檔案匯出資料

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行會從請求中擷取檔案

第2行會從請求中擷取saveLocation

第5行獲得FormsService

第6行將xmlData從PDF檔案中導出

**在系統上測試示例包**

[使用AEM套件管理器下載及安裝套件](assets/outputandformsservice.zip)




**安裝套件後，您必須在AdobeGranite CSRF篩選器中允許列出下列URL。**

1. 請依照下列步驟允許列出上述路徑。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增下列3個路徑並儲存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜尋「Sling反向連結篩選器」
1. 勾選「允許空白」核取方塊。 （此設定應僅用於測試用途）
測試范常式式碼有許多方式。 最快、最簡單的做法是使用Postman應用程式。 Postman可讓您向伺服器提出POST請求。 在您的系統上安裝Postman應用程式。
啟動應用程式，然後輸入下列URL以測試匯出資料API

請確定您已從下拉式清單中選取「POST」
http://localhost:4502/content/AemFormsSamples/exportdata.html
請務必將「授權」指定為「基本驗證」。 指定AEM伺服器使用者名稱和密碼
導覽至「內文」標籤，並指定要求參數，如下圖所示
![export](assets/postexport.png)
然後按一下「傳送」按鈕

包含3個樣本。 以下各段說明何時使用輸出服務或Forms服務（服務的url），輸入每個服務所需的參數

**合併資料並平面化輸出：**

* 使用輸出服務來合併資料與xdp或pdf檔案，以產生平面化的pdf
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **請求參數 —**

   * xdp_or_pdf_file :要合併資料的xdp或pdf檔案
   * xmlfile:將與xdp_or_pdf_file合併的xml資料檔案
   * saveLocation:在檔案系統上保存已呈現文檔的位置

**將資料匯入PDF檔案：**
* 使用FormsService將資料匯入PDF檔案
* **POSTURL**  - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **請求參數：**

   * pdfile :要合併資料的PDF檔案
   * xmlfile:將與pdf檔案合併的xml資料檔案
   * saveLocation:在檔案系統上保存已呈現文檔的位置。 例如c:\\\outputsample.pdf。

**從PDF檔案匯出資料**
* 使用FormsService從PDF檔案匯出資料
* **POST** URL - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **請求參數：**

   * pdfile :要從中導出資料的PDF檔案
   * saveLocation:在檔案系統上保存導出資料的位置

[您可以匯入此Postman集合以測試API](assets/document-services-postman-collection.json)

