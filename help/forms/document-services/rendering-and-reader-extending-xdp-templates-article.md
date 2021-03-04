---
title: 使用使用權將XDP轉換為PDF
seo-title: 使用使用權將XDP轉換為PDF
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
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---


# 使用使用權限將XDP轉換為PDF{#rendering-xdp-into-pdf-with-usage-rights}

常見的使用案例是將xdp轉譯為PDF，並將Reader擴充功能套用至轉譯的PDF。

例如，在AEM Forms的表單入口網站中，當使用者按一下XDP時，我們可將XDP轉換為PDF，而閱讀程式可擴充PDF。

若要測試此功能，您可以試用此[link](https://forms.enablementadobe.com/content/samples/samples.html?query=0)。 範例名稱為「Render XDP with RE」

要完成此使用案例，我們需要執行以下操作。

* 將Reader擴展證書添加到&quot;fd-service&quot;用戶。 添加Reader擴展憑據的步驟列在[此處](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html)

* 建立自訂OSGi服務，以呈現並套用使用權限。 完成此作業的程式碼列於下方

## 演算XDP並套用使用權限{#render-xdp-and-apply-usage-rights}

* 第7行：使用FormsService的renderPDForm，我們會從XDP產生PDF。

* 第8-14行：已設定適當的使用權限。 這些使用權限是從OSGi配置設定中提取的。

* 第20行：使用與服務用戶fd-service關聯的資源解析器

* 第24行：DocumentAssuranceService的secureDocument方法用於應用使用權

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

以下螢幕擷取顯示已公開的設定屬性。 大部分的常用使用權限都會透過此組態公開。

![](assets/configurationproperties.gif)

以下代碼顯示用於構建OSGi配置設定的代碼

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

## 建立Servlet以串流化PDF {#create-servlet-to-stream-the-pdf}

下一步是使用GET方法建立servlet，將Reader擴充的PDF傳回給使用者。 在這種情況下，系統會要求使用者將PDF儲存至其檔案系統。 這是因為PDF會轉譯為動態PDF，而隨瀏覽器提供的PDF檢視器則不會處理動態PDF。

以下是servlet的代碼。 我們將CRX儲存庫中的XDP路徑傳遞到此servlet。

然後，我們會呼叫com.aemformssamples.documentservices.core.DocumentServices的renderAndExtendXdp方法。

然後，將Reader擴充的PDF串流至呼叫應用程式

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

若要在本機伺服器上測試此項，請依照下列步驟進行
1. [下載並安裝DevelopingWithServiceUser Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載並安裝AEMFormsDocumentServices套件](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
1. [使用套件管理員將與本文相關的資產下載AEM並匯入](assets/renderandextendxdp.zip)
   * 此套件包含範例入口網站和xdp檔案
1. 將Reader擴展證書添加到&quot;fd-service&quot;用戶
1. 將瀏覽器指向[入口網頁](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. 按一下pdf圖示以轉譯xdp並取得PDF，此為ReaderExtended



