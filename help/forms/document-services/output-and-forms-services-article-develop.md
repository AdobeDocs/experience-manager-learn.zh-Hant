---
title: 用產出和Forms服務在AEM Forms發展
description: 在AEM Forms使用Output和Forms服務API
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: d268d5d6-f24f-4db9-b8e0-07dd769c6005
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 46df7b13401ee3497c871eac3b8158148c2e6a04
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# 用產出和Forms服務在AEM Forms發展{#developing-with-output-and-forms-services-in-aem-forms}

在AEM Forms使用Output和Forms服務API

在本文中，我們將看一下

* 輸出服務 — 通常，此服務用於將xml資料與xdp模板或pdf合併，以生成拼合的pdf。 有關詳細資訊，請參閱此 [javadox](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 的子菜單。
* FormsService — 這是一種功能非常廣泛的服務，允許您從PDF檔案導出/導入資料。 有關詳細資訊，請參閱此 [javadox](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/forms/api/FormsService.html) 為Forms服務。


以下代碼段從PDF檔案導出資料

```java
javax.servlet.http.Part pdfPart = request.getPart("pdffile");
String filePath = request.getParameter("saveLocation");
java.io.InputStream pdfIS = pdfPart.getInputStream();
com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);
com.adobe.fd.forms.api.FormsService formsservice = sling.getService(com.adobe.fd.forms.api.FormsService.class);
com.adobe.aemfd.docmanager.Document xmlDocument = formsservice.exportData(pdfDocument,com.adobe.fd.forms.api.DataFormat.Auto);
```

第1行從請求中提取檔案

行2從請求中提取saveLocation

第5行獲取FormsService

第6行從PDF檔案導出xmlData

**test系統上的示例包**

[使用包管理器下載並安裝AEM包](assets/outputandformsservice.zip)




**安裝軟體包後，必須允許在AdobeGranite CSRF過濾器中列出以下URL。**

1. 請按照下面所述的步驟列出上述路徑。
1. [登錄到configMgr](http://localhost:4502/system/console/configMgr)
1. Adobe花崗岩CSRF濾波器的研究
1. 在排除的部分中添加以下3個路徑並保存
1. /content/AemFormsSamples/mergedata
1. /content/AemFormsSamples/exportdata
1. /content/AemFormsSamples/outputservice
1. 搜索「Sling引用者篩選器」
1. 選中「允許空」複選框。 （此設定應僅用於測試目的）有多種test示例代碼的方法。 最快捷最簡單的方法是使用Postman應用。 Postman允許您向伺服器發出POST請求。 在系統上安裝Postman應用。
啟動應用並輸入以下URL以test導出資料API

確保從下拉清單http://localhost:4502/content/AemFormsSamples/exportdata.html中選擇了「POST」，確保將「授權」指定為「基本授權」。 指定服AEM務器用戶名和密碼導航到「正文」頁籤並指定請求參數，如下圖所示
![出口](assets/postexport.png)
然後按一下「發送」按鈕

該包包含3個示例。 以下各段說明何時使用輸出服務或Forms服務，服務的url，輸入每個服務期望的參數

## 合併資料並拼合輸出

* 使用輸出服務將資料與xdp或pdf文檔合併以生成拼合pdf
* **POSTURL**:http://localhost:4502/content/AemFormsSamples/outputservice.html
* **請求參數 —**

   * **xdp_or_pdf_file** :要將資料與合併的xdp或pdf檔案
   * **xml檔案**:與xdp_or_pdf_file合併的xml資料檔案
   * **保存位置**:在檔案系統上保存所呈現文檔的位置。 例如c:\\documents\\sample.pdf

### 將資料導入PDF檔案

* 使用FormsService將資料導入PDF檔案
* **POSTURL** - http://localhost:4502/content/AemFormsSamples/mergedata.html
* **請求參數：**

   * **雜** :要將資料與合併的pdf檔案
   * **xml檔案**:與pdf檔案合併的xml資料檔案
   * **保存位置**:在檔案系統上保存所呈現文檔的位置。 例如 `c:\\outputsample.pdf`。

**從PDF檔案導出資料**
* 使用FormsService從PDF檔案導出資料
* **POSTUR** L - http://localhost:4502/content/AemFormsSamples/exportdata.html
* **請求參數：**

   * **雜** :要從中導出資料的pdf檔案
   * **保存位置**:在檔案系統上保存導出資料的位置。 例如c:\\documents\\exported_data.xml

[您可以導入此郵遞員集合以testAPI](assets/document-services-postman-collection.json)
