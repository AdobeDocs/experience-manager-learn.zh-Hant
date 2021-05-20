---
title: 在HTM5表單提交上觸發AEM工作流程
seo-title: 在HTML5表單提交上觸發AEM工作流程
description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---


# 建立自訂設定檔

在本部分，我們將建立[自訂設定檔。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) 設定檔負責將XDP轉譯為HTML。預設設定檔會立即提供，以便將XDP轉譯為HTML。 代表自訂版本的Mobile Forms轉譯服務。 您可以使用Mobile Form Rendition服務來自訂Mobile Forms的外觀、行為和互動。 在自訂設定檔中，我們將使用Guidebridge API擷取行動表單中填入的資料。 然後，此資料會傳送至自訂servlet，由servlet產生互動式PDF，並將其串流回呼叫應用程式。

使用`formBridge` JavaScript API取得表單資料。 我們使用`getDataXML()`方法：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

在成功處理常式方法中，我們會呼叫在AEM中執行的自訂servlet。 此Servlet會呈現並傳回含有行動表單資料的互動式pdf

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

以下是servlet程式碼，負責轉譯互動式pdf並將pdf傳回呼叫應用程式。 Servlet調用自定義DocumentServices OSGi服務的`mobileFormToInteractivePdf`方法。

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

### 轉譯互動式PDF

下列程式碼會使用[Forms服務API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html)，以行動表單中的資料轉譯互動式PDF。

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

若要檢視從部分完成的行動表單下載互動式PDF的功能，請按一下這裡[。
](https://forms.enablementadobe.com/content/dam/formsanddocuments/schengen.xdp/jcr:content)下載PDF後，下一步是提交PDF以觸發AEM工作流程。 此工作流程將合併提交的PDF中的資料，並產生非互動式PDF以供審核。

針對此使用案例建立的自訂設定檔可在本教學課程資產中使用。
