---
title: 使用權限將XDP轉譯為PDF
seo-title: 使用權限將XDP轉譯為PDF
description: 將使用權套用至pdf
seo-description: 將使用權套用至pdf
uuid: 5e60c61e-821d-439c-ad89-ab169ffe36c0
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
feature: 表單服務
discoiquuid: aefb4124-91a0-4548-94a3-86785ea04549
topic: 開發
role: Developer
level: Experienced
source-git-commit: aa90b2c1a066dc36d4ba26ecdb8b58939445ef34
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---


# 使用權限將XDP轉譯為PDF{#rendering-xdp-into-pdf-with-usage-rights}

常見的使用案例是將xdp轉譯為PDF，並將Reader擴充功能套用至轉譯的PDF。

例如，在AEM Forms的表單入口網站中，當使用者按一下XDP時，我們可以將XDP轉譯為PDF，並透過閱讀器擴充PDF。

若要測試此功能，您可以嘗試此[link](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse2)。 範例名稱為「Render and Extend XDP」

若要完成此使用案例，我們必須執行下列動作。

* 將Reader擴充功能憑證新增至「fd-service」使用者。 新增Reader擴充功能憑證的步驟如下： [此處](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=en)


* 您也可以參閱[設定Reader擴充功能憑證](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)上的影片


* 建立將呈現和套用使用權限的自訂OSGi服務。 完成此作業的程式碼列於下方

## 呈現XDP並套用使用權限 {#render-xdp-and-apply-usage-rights}

* 第7行：我們會使用FormsService的renderPDForm，從XDP產生PDF。

* 第8-14行：已設定適當的使用權限。 這些使用權限會從OSGi組態設定中擷取。

* 第20行：使用與服務使用者fd-service相關聯的資源解析器

* 第24行：DocumentAssuranceService的secureDocument方法用於應用使用權限

```java
 public Document renderAndExtendXdp(String xdpPath) {
  // TODO Auto-generated method stub
  log.debug("In renderAndExtend xdp the alias is " + docConfig.ReaderExtensionAlias());
  PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
  renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
  try {
   Document xdpRenderedAsPDF = formsService.renderPDFForm("crx://" + xdpPath, null, renderOptions);
   UsageRights usageRights = new UsageRights();
   usageRights.setEnabledBarcodeDecoding(docConfig.BarcodeDecoding());
   usageRights.setEnabledFormFillIn(docConfig.FormFill());
   usageRights.setEnabledComments(docConfig.Commenting());
   usageRights.setEnabledEmbeddedFiles(docConfig.EmbeddingFiles());
   usageRights.setEnabledDigitalSignatures(docConfig.DigitialSignatures());
   usageRights.setEnabledFormDataImportExport(docConfig.FormDataExportImport());
   ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
   UnlockOptions unlockOptions = null;
   ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
   reOptions.setCredentialAlias(docConfig.ReaderExtensionAlias());
   log.debug("set the credential");
   reOptions.setResourceResolver(getResolver.getFormsServiceResolver());
   
   reOptions.setReOptions(reOptionsSpec);
   log.debug("set the resourceResolver and re spec");
   xdpRenderedAsPDF = docAssuranceService.secureDocument(xdpRenderedAsPDF, null, null, reOptions,
     unlockOptions);

   return xdpRenderedAsPDF;
  } catch (FormsServiceException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;

 }
```

以下螢幕擷圖顯示公開的設定屬性。 大部分的常見使用權限會透過此設定公開。

![](assets/configurationproperties.gif)

下列程式碼會顯示用來建置OSGi組態設定的程式碼

```java
package com.aemformssamples.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "AEM Forms Samples Doc Services Configuration", description = "AEM Forms Samples Doc Services Configuration")
public @interface DocSvcConfiguration {
 @AttributeDefinition(name = "Allow Form Fill", description = "Allow Form Fill", type = AttributeType.BOOLEAN)
 boolean FormFill() default false;

 @AttributeDefinition(name = "Allow BarCode Decoding", description = "Allow BarCode Decoding", type = AttributeType.BOOLEAN)
 boolean BarcodeDecoding() default false;

 @AttributeDefinition(name = "Allow File Embedding", description = "Allow File Embedding", type = AttributeType.BOOLEAN)
 boolean EmbeddingFiles() default false;

 @AttributeDefinition(name = "Allow Commenting", description = "Allow Commenting", type = AttributeType.BOOLEAN)
 boolean Commenting() default false;

 @AttributeDefinition(name = "Allow DigitialSignatures", description = "Allow File DigitialSignatures", type = AttributeType.BOOLEAN)
 boolean DigitialSignatures() default false;

 @AttributeDefinition(name = "Allow FormDataExportImport", description = "Allow FormDataExportImport", type = AttributeType.BOOLEAN)
 boolean FormDataExportImport() default false;

 @AttributeDefinition(name = "Reader Extension Alias", description = "Alias of your Reader Extension")
 String ReaderExtensionAlias() default "";

}
```

## 建立Servlet以串流PDF {#create-servlet-to-stream-the-pdf}

下一步是使用GET方法建立Servlet，將Reader延伸PDF傳回給使用者。 在此情況下，系統會要求使用者將PDF儲存至其檔案系統。 這是因為PDF會轉譯為動態PDF，而瀏覽器隨附的PDF檢視器不會處理動態PDF。

以下是servlet的程式碼。 我們會將CRX存放庫中XDP的路徑傳遞至此servlet。

接著，我們會呼叫com.aemformssamples.documentservices.core.DocumentServices的renderAndExtendXdp方法。

然後，將讀取器擴展的PDF流式化到調用應用程式

```java
package com.aemformssamples.documentservices.core.servlets;

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(service = Servlet.class, property = {

  "sling.servlet.methods=get",

  "sling.servlet.paths=/bin/renderandextend"

})
public class RenderAndReaderExtend extends SlingSafeMethodsServlet {
 @Reference
 FormsService formsService;
 @Reference
 DocumentServices documentServices;
 private static final Logger log = LoggerFactory.getLogger(RenderAndReaderExtend.class);
 private static final long serialVersionUID = 1L;

 protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  log.debug("The path of the XDP I got was " + request.getParameter("xdpPath"));
  Document renderedPDF = documentServices.renderAndExtendXdp(request.getParameter("xdpPath"));
  response.setContentType("application/pdf");
  response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
  try {
   response.setContentLength((int) renderedPDF.length());
   InputStream fileInputStream = null;
   fileInputStream = renderedPDF.getInputStream();
   OutputStream responseOutputStream = null;
   responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    {
     responseOutputStream.write(bytes);
    }

   }
  } catch (IOException e2) {
   // TODO Auto-generated catch block
   e2.printStackTrace();
  }

 }

}
```

若要在本機伺服器上測試，請執行下列步驟
1. [下載並安裝DevelopingWithServiceUser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載並安裝AEMFormsDocumentServices套件組合](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [下載自訂入口網站範本html](assets/render-and-extend-template.zip)
1. [使用套件管理器將與本文相關的資產下載並匯入AEM](assets/renderandextendxdp.zip)
   * 此套件包含範例入口網站和xdp檔案
1. 將Reader擴充功能憑證新增至「fd-service」使用者
1. 將瀏覽器指向[門戶網頁](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. 按一下pdf圖示，將xdp轉譯為已套用使用權限的pdf檔案。



