---
title: 在AEM Forms中使用Output和Forms Services進行開發
seo-title: 在AEM Forms中使用Output和Forms Services進行開發
description: 在AEM Forms中使用Output and Forms Service API
seo-description: 在AEM Forms中使用Output and Forms Service API
feature: forms-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---


# 在AEM Forms中使用Forms Services轉換互動式PDF

在AEM Forms中使用Forms Service API來轉換互動式PDF

在本文中，我們將檢視下列服務

* FormsService —— 這項多功能服務可讓您從PDF檔案匯出／匯入資料，並借由將xml資料合併為xdp範本，產生互動式pdf

此處列出AEM Forms API的正式Javadoc [。](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

下列程式碼片段會使用FormsService的renderPDFForm操作來轉譯互動式pdf。 申根。xdp是用於合併xml資料的範本。

```java
String uri = "crx:///content/dam/formsanddocuments";
PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
renderOptions.setContentRoot(uri);
Document interactivePDF = null;
try {
interactivePDF = formsService.renderPDFForm("schengen.xdp", xmlData, renderOptions);
} catch (FormsServiceException e) {
 e.printStackTrace();
}
return interactivePDF;
```

第1行：包含xdp模板的資料夾的位置

第2-4行：建立PDFFormRenderOptions並設定其屬性

第7行：使用FormsService的renderPDFForm服務作業產生互動式PDF

第11行：將產生的互動式pdf傳回至呼叫應用程式

**在系統上測試示例包**
1. [使用Felix Web Console下載並安裝DocumentServices範例套裝](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用AEM套件管理員下載並安裝套件](assets/downloadinteractivepdffrommobileform.zip)



1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋Adobe Granite CSRF濾鏡
1. 在排除的區段中新增下列路徑並儲存
1. /bin/generateinteractivepdf
1. [開啟行動表單](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填寫幾個欄位，然後按一下「下 ***載並填寫……」.*** 按鈕
1. 互動式pdf應下載至您的本機系統


範例套件包含與Mobile Form關聯的自訂描述檔。 請瀏覽 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 檔案。 此jsp從移動表單中提取資料，並對裝載在 ***/bin/generateinteractivepdf路徑上的servlet發出POST請求*** 。 Servlet會將互動式pdf傳回至呼叫應用程式。 customtoolbar.jsp中的代碼然後將檔案下載到本地系統


