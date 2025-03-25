---
title: 在AEM Forms中使用輸出和Forms服務進行開發
description: 瞭解如何使用AEM Forms中的輸出和Forms服務API進行開發。
feature: Output Service
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2024-01-29T00:00:00Z
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
duration: 122
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---

# 在AEM Forms中使用輸出和Forms服務進行開發{#developing-with-output-and-forms-services-in-aem-forms}

瞭解如何使用AEM Forms中的輸出和Forms服務API進行開發。

在本文中，我們將瞭解以下內容

* [輸出服務](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) — 通常此服務是用來合併xml資料與xdp範本或pdf以產生平面化pdf。
* [FormsService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) — 這是功能非常廣泛的服務，可讓您將xdp轉譯為pdf，以及將資料從和PDF檔案匯出/匯入。


下列程式碼片段會從PDF檔案匯出資料

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行會從請求中擷取PDF檔案

Line2會從請求中擷取saveLocation

第5行取得FormsService

第6行會從PDF檔案匯出xmlData

**若要在您的系統上測試範例封裝**

[使用AEM封裝管理員下載並安裝封裝](assets/using-output-and-form-service-api.zip)




**安裝套件後，您必須在Adobe Granite CSRF篩選器中允許列出下列URL。**

1. 請依照下列步驟操作，將上述路徑加入允許清單。
1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Adobe Granite CSRF篩選器
1. 在排除的區段中新增下列3個路徑並儲存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. /content/AemFormsSamples/renderxdp
1. 搜尋「Sling查閱者篩選器」
1. 勾選「允許空白」核取方塊。 （此設定僅供測試之用）

## 測試樣本

測試範常式式碼的方法有很多種。 最快捷、最輕鬆的方式就是使用Postman應用程式。 Postman可讓您向伺服器發出POST要求。

* 在您的系統上安裝Postman app 。
* 啟動應用程式並輸入適當的URL
* 確定您已從下拉式清單中選取「POST」
* 請務必將「授權」指定為「基本驗證」。 指定AEM伺服器使用者名稱和密碼
* 在body標籤中指定請求引數
* 按一下傳送按鈕

此套件包含4個範例。 以下段落說明何時使用輸出服務或Forms服務、服務的URL、每個服務預期的輸入引數

## 使用OutputService將資料與xdp範本合併

* 使用輸出服務將資料與xdp或pdf檔案合併，以產生平面化pdf
* **張貼URL**： http://localhost:4502/content/AemFormsSamples/outputservice.html
* **要求引數 —**

   * **xdp_or_pdf_file** ：您要合併資料的xdp或pdf檔案
   * **xmlfile**：與xdp_or_pdf_file合併的xml資料檔
   * **saveLocation**：將演算後的檔案儲存在檔案系統上的位置。 例如c：\\documents\\sample.pdf

### 使用FormsService API

#### 匯入資料

* 使用FormsService importData將資料匯入PDF檔案
* **張貼URL** - http://localhost:4502/content/AemFormsSamples/mergedata.html

* **要求引數：**

   * **pdf檔案** ：您要與它合併資料的pdf檔案
   * **xmlfile**：與pdf檔案合併的xml資料檔
   * **saveLocation**：將演算後的檔案儲存在檔案系統上的位置。 例如 `c:\\outputsample.pdf`。

#### 匯出資料

* 使用FormsService exportData API從PDF檔案匯出資料
* **張貼URL** - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **要求引數：**

   * **pdf檔案** ：您要匯出資料的pdf檔案
   * **saveLocation**：將匯出的資料儲存在檔案系統上的位置。 例如c：\\documents\\exported_data.xml

#### 轉譯XDP

* 將XDP範本轉譯為靜態/動態pdf
* 使用FormsService renderPDFForm API將xdp範本轉譯為PDF
* **張貼URL** - http://localhost:4502/content/AemFormsSamples/renderxdp?xdpName=f1040.xdp
* 要求引數：
   * xdpName：要呈現為pdf的xdp檔案的名稱

[您可以匯入此Postman集合以測試API](assets/UsingDocumentServicesInAEMForms.postman_collection.json)

