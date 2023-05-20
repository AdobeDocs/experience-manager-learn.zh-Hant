---
title: 將XDP呈現為具有使用權限的PDF
description: 將使用權限應用於pdf
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
exl-id: ce1793d1-f727-4bc4-9994-f495b469d1e3
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 2%

---

# 將XDP呈現為具有使用權限的PDF{#rendering-xdp-into-pdf-with-usage-rights}

通用用例是將xdp呈現到PDF中，並將Reader擴展應用到所呈現的PDF。

例如，在AEM Forms的表單門戶中，當用戶按一下XDP時，可以將XDP呈現為PDF，讀取器擴展PDF。


要完成此使用情形，我們需要執行以下操作。

* 將Reader擴展證書添加到「fd-service」用戶。 列出了添加Reader擴展憑據的步驟 [這裡](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=zh-Hant)


* 您還可以參考上的視頻 [配置Reader擴展憑據](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html)


* 建立呈現和應用使用權限的自定義OSGi服務。 要完成此操作的代碼列於下面

## 呈現XDP並應用使用權限 {#render-xdp-and-apply-usage-rights}

* 第7行：使用FormsService的renderPDFForm，我們會從XDP生成PDF。

* 第8-14行：設定相應的使用權限。 從OSGi配置設定獲取這些使用權限。

* 第20行：使用與服務用戶fd-service關聯的資源解析器

* 24號線：DocumentAssuranceService的secureDocument方法用於應用使用權限

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

以下螢幕快照顯示了已公開的配置屬性。 大多數常用權限都通過此配置公開。

![配置屬性](assets/configurationproperties.gif)

以下代碼顯示用於生成OSGi配置設定的代碼

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

## 建立Servlet以流式傳輸PDF {#create-servlet-to-stream-the-pdf}

下一步是使用GET方法建立Servlet，將讀取器擴展PDF返回給用戶。 在這種情況下，系統會要求用戶將PDF保存到其檔案系統。 這是因為PDF被呈現為動態PDF，而瀏覽器附帶的pdf查看器不處理動態pdf。

以下是servlet的代碼。 我們將CRX儲存庫中的XDP路徑傳遞到此servlet。

然後，我們調用com.aemformssamples.documentservices.core.DocumentServices的renderAndExtendXdp方法。

然後，將讀取器擴展PDF流式傳輸到調用應用程式

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

要在本地伺服器上test此功能，請執行以下步驟
1. [下載並安裝DevelopingWithServiceUser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載並安裝AEMFormsDocumentServices捆綁包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

1. [下載自定義門戶模板html](assets/render-and-extend-template.zip)
1. [使用包管理器將與本文相關的資產下載AEM並導入](assets/renderandextendxdp.zip)
   * 此包包含示例入口和xdp檔案
1. 將Reader擴展證書添加到「fd-service」用戶
1. 將瀏覽器指向 [門戶網頁](http://localhost:4502/content/AemForms/ReaderExtensionsXdp.html)
1. 按一下pdf表徵圖將xdp呈現為應用了使用權限的pdf檔案。
