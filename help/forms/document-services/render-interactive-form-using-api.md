---
title: 在AEM Forms中使用Forms服務轉譯互動式PDF
description: 在AEM Forms中使用Forms服務API來轉譯互動式PDF
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# 在AEM Forms中使用Forms服務轉譯互動式PDF

在AEM Forms中使用Forms服務API來轉譯互動式PDF

在本文中，我們將審視下列服務

* FormsService — 這項服務用途廣泛，可讓您從PDF檔案匯出/匯入資料，並透過將xml資料合併至xdp範本，產生互動式pdf

官員 [此處列出AEM Forms API的javadoc](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

下列程式碼片段會使用FormsService的renderPDFForm操作轉譯互動式pdf。 申根.xdp是用於合併xml資料的模板。

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

第1行：包含xdp範本的資料夾位置

第2-4行：建立PDFFormRenderOptions並設定其屬性

第7行：使用FormsService的renderPDFForm服務操作生成互動式PDF

第11行：將產生的互動式pdf傳回給呼叫應用程式

**在系統上測試示例包**
1. [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [使用Felix Web Console下載並安裝DocumentServices範例套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用AEM套件管理器下載及安裝套件](assets/downloadinteractivepdffrommobileform.zip)

1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增下列路徑並儲存
1. /bin/generateinteractivepdf
1. 搜尋 _Apache Sling Service使用者對應程式服務_ 並按一下以開啟屬性
   1. 按一下 *+* 圖示（加號）以新增下列服務對應
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. 按一下「保存」
1. [開啟行動表單](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填寫幾個欄位，然後按一下 ***下載並填寫…….*** 按鈕
1. 應將互動式pdf下載至您的本機系統


範例套件包含與行動表單相關聯的自訂設定檔。 請探索 [customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp) 檔案。 此jsp會從行動表單中擷取資料，並對裝載於的servlet發出POST請求 ***/bin/generateinteractivepdf*** 路徑。 Servlet會將互動式PDF傳回至呼叫應用程式。 customtoolbar.jsp中的代碼，然後將檔案下載到本地系統
