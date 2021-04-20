---
title: HTM5表AEM單提交的觸發工作流程
seo-title: HTML5表AEM單提交的觸發工作流程
description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
seo-description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 1%

---


# 建立自訂描述檔

在本部分，我們將建立[自定義配置檔案。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) 描述檔負責將XDP轉換為HTML。預設描述檔是現成可用來將XDP轉換為HTML的。 它代表自訂版本的MobileForms轉譯服務。 您可以使用Mobile Form Rendition服務來自訂行動Forms的外觀、行為和互動。 在我們的自訂描述檔中，我們會使用Guidebridge API擷取行動表單中填入的資料。 然後，此資料會傳送至自訂servlet，然後產生互動式PDF並將它串流回呼叫應用程式。

使用`formBridge` JavaScript API取得表單資料。 我們使用`getDataXML()`方法：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

在成功處理常式方法中，我們會呼叫在中執行的自訂servletAEM。 此servlet將轉換並傳回包含行動表單資料的互動式pdf

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    console.log("The data: " + data);
    xhr.open('POST','/bin/generateinteractivepdf');
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    xhr.send(formData);
    xhr.onload = function(e) {
        
        console.log("The data is ready");
        if (this.status == 200) {
            var blob = new Blob([this.response],{type:'image/pdf'});
                let a = document.createElement("a");
                a.style = "display:none";
                document.body.appendChild(a);
                let url = window.URL.createObjectURL(blob);
                a.href = url;
                a.download = "schengenvisaform.pdf";
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## 產生互動式PDF

以下是負責轉譯互動式pdf並將pdf傳回呼叫應用程式的servlet程式碼。 Servlet調用自定義DocumentServices OSGi服務的`mobileFormToInteractivePdf`方法。

```java
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.PrintWriter;

import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import com.adobe.aemfd.docmanager.Document;
import com.aemformssamples.documentservices.core.DocumentServices;

@Component(
  service = { Servlet.class }, 
  property = { 
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/generateinteractivepdf" 
  }
)
public class GenerateInteractivePDF extends SlingAllMethodsServlet {
    @Reference
    DocumentServices documentServices;

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) { 
       doPost(request, response);
    }

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
      String dataXml = request.getParameter("formData");
      org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
      Document xmlDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
      Document generatedPDF = documentServices.mobileFormToInteractivePdf(xmlDocument,request.getParameter("xdpPath"));
      try {
          InputStream fileInputStream = generatedPDF.getInputStream();
          response.setContentType("application/pdf");
          response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
          response.setContentLength((int) fileInputStream.available());
          OutputStream responseOutputStream = response.getOutputStream();
          int bytes;
          while ((bytes = fileInputStream.read()) != -1) {
              responseOutputStream.write(bytes);
          }
          responseOutputStream.flush();
          responseOutputStream.close();
      } catch (IOException e) {
        // TODO Add proper error logging
      }
    }
}
```

### 轉換互動式PDF

下列程式碼使用[Forms服務API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html)來轉換具有行動表單資料的互動式PDF。

```java
public Document mobileFormToInteractivePdf(Document xmlData,String path) {
    // In mobile form to interactive pdf
    
    String uri = "crx:///content/dam/formsanddocuments";
    String xdpName = path.substring(31,path.lastIndexOf("/jcr:content"));
    PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
    renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
    renderOptions.setContentRoot(uri);
    Document interactivePDF = null;

    try {
        interactivePDF = formsService.renderPDFForm(xdpName, xmlData, renderOptions);
    } catch (FormsServiceException e) {
        // TODO Add proper error logging
    }
    
    return interactivePDF;
}
```

若要檢視從部分完成的行動表單下載互動式PDF的功能，請按一下此處[。
](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content)下載PDF後，下一步是提交PDF以觸發工作AEM流程。 此工作流程將合併提交PDF的資料，並產生非互動式PDF以供審核。

為此使用案例建立的自訂描述檔可做為本教學課程資產的一部分。
