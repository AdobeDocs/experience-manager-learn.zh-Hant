---
title: 在AEM Forms中使用Forms服務呈現互動式PDF
description: 在AEM Forms中使用Forms Service API來呈現互動式PDF
feature: Forms Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9b2ef4c9-8360-480d-9165-f56a959635fb
last-substantial-update: 2020-07-07T00:00:00Z
duration: 75
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# 在AEM Forms中使用Forms服務呈現互動式PDF

在AEM Forms中使用Forms Service API來呈現互動式PDF

在本文中，我們將瞭解以下服務

* FormsService — 這項功能非常廣泛的服務可讓您從PDF檔案匯出/匯入資料，也可以將xml資料合併到xdp範本中，產生互動式pdf

此處列出適用於AEM Forms API的官方[javadoc](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/package-summary.html)

下列程式碼片段會使用FormsService的renderPDFForm作業來轉譯互動式pdf。 schengen.xdp是用於合併xml資料的範本。

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

Line2-4：建立PDFFormRenderOptions並設定其屬性

第7行：使用FormsService的renderPDFForm服務操作產生互動式PDF

第11行：將產生的互動式pdf傳回給呼叫應用程式

**若要在您的系統上測試範例封裝**
1. [下載並安裝DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [使用Felix網頁主控台下載並安裝DocumentServices範例套件](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用AEM封裝管理員下載並安裝封裝](assets/downloadinteractivepdffrommobileform.zip)

1. [登入configMgr](http://localhost:4502/system/console/configMgr)
1. 搜尋AdobeGranite CSRF篩選器
1. 在排除的區段中新增以下路徑並儲存
1. /bin/generateinteractivepdf
1. 搜尋&#x200B;_Apache Sling Service使用者對應程式服務_，然後按一下以開啟屬性
   1. 按一下&#x200B;*+*&#x200B;圖示（加號）以新增下列服務對應
      * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
   1. 按一下「儲存」
1. [開啟行動表單](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)
1. 填寫幾個欄位，然後按一下&#x200B;***下載並填寫....位***&#x200B;按鈕
1. 互動式pdf應下載至您的本機系統


範例套件包含與行動表單相關聯的自訂設定檔。 請探索[customtoolbar.jsp](http://localhost:4502/apps/AEMFormsDemoListings/customprofiles/addImageToMobileForm/demo/customtoolbar.jsp)檔案。 此jsp會從行動表單中擷取資料，並向&#x200B;***/bin/generateinteractivepdf***&#x200B;路徑上掛接的servlet發出POST請求。 此servlet會將互動式pdf傳回至呼叫的應用程式。 customtoolbar.jsp中的程式碼然後將檔案下載到您的本機系統
