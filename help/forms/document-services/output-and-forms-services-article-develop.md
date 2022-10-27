---
title: 在AEM Forms與產出和Forms服務一起發展
description: 在AEM Forms中使用輸出和Forms服務API
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# 在AEM Forms與產出和Forms服務一起發展{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms中使用輸出和Forms服務API

在本文中，我們將審視以下內容

* 輸出服務 — 通常，此服務用於將xml資料與xdp範本合併，或以pdf產生平面化的pdf。 如需更多詳細資訊，請參閱[javado](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) （輸出服務）。
* FormsService — 這項服務用途廣泛，可讓您從匯出檔案匯入資料，並匯入PDF檔案。 如需更多詳細資訊，請參閱 [javado](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/forms/api/class-use/FormsService.html) Forms局。


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

第6行從PDF檔案導出xmlData

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
1. 勾選「允許空白」核取方塊。 （此設定應僅用於測試用途）有許多方法可測試范常式式碼。 最快、最簡單的方式是使用Postman應用程式。 Postman可讓您向伺服器提出POST請求。 在您的系統上安裝Postman應用程式。
啟動應用程式，然後輸入下列URL以測試匯出資料API

確認您已從下拉式清單http://localhost:4502/content/AemFormsSamples/exportdata.html中選取「POST」。請確定您將「授權」指定為「基本驗證」。 指定AEM伺服器使用者名稱和密碼導覽至「Body」標籤，並指定要求參數，如下圖所示
![匯出](assets/postexport.png)
然後按一下「傳送」按鈕

包含3個樣本。 以下各段說明何時使用輸出服務或Forms服務（服務的url），輸入每個服務所需的參數

## 合併資料並平面化輸出

* 使用輸出服務來合併資料與xdp或pdf檔案，以產生平面化的pdf
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **請求參數 —**

   * **xdp_or_pdf_file** :要合併資料的xdp或pdf檔案
   * **xml檔案**:與xdp_or_pdf_file合併的xml資料檔案
   * **saveLocation**:在檔案系統上保存已呈現文檔的位置。 例如c:\\documents\\sample.pdf

### 將資料匯入PDF檔案

* 使用FormsService將資料匯入PDF檔案
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **請求參數：**

   * **pdfile** :要合併資料的PDF檔案
   * **xml檔案**:與pdf檔案合併的xml資料檔案
   * **saveLocation**:在檔案系統上保存已呈現文檔的位置。 例如 `c:\\outputsample.pdf`.

**從PDF檔案匯出資料**
* 使用FormsService從PDF檔案匯出資料
* **POSTUR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **請求參數：**

   * **pdfile** :要從中導出資料的PDF檔案
   * **saveLocation**:在檔案系統上保存導出資料的位置。 例如c:\\documents\\exported_data.xml

[您可以匯入此Postman集合以測試API](assets/document-services-postman-collection.json)
