---
title: 觸AEM發HTM5表單提交工作流 — 建立自定義配置檔案
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 以離線模式繼續填寫移動表單並提交移動表單以觸發工AEM作流
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4, 6.5
topic: Development
role: Developer
level: Experienced
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 建立自定義配置檔案

在此部分，我們將建立 [自定義配置檔案。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html) 配置檔案負責將XDP呈現為HTML。 在框外提供預設配置檔案，將XDP呈現為HTML。 它代表了MobileForms格式副本服務的自定義版本。 您可以使用Mobile表單格式副本服務來定制MobileForms的外觀、行為和交互。 在我們的自定義配置檔案中，我們將使用引導橋API捕獲移動表單中填充的資料。 然後，此資料被發送到自定義servlet，該Servlet將生成互動式PDF並將其流回調用應用程式。

使用 `formBridge` JavaScript API。 我們利用 `getDataXML()` 方法：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

在成功處理程式方法中，我們調用中運行的自定義ServletAEM。 此Servlet將呈現並返回帶有移動表單中資料的互動式pdf

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

## 生成交互PDF

以下是Servlet代碼，負責呈現互動式pdf並將pdf返回給調用應用程式。 Servlet調用 `mobileFormToInteractivePdf` 自定義DocumentServices OSGi服務的方法。

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

### 呈現互動式PDF

以下代碼使用 [Forms服務API](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html) 與來自移動表單的資料進行交互PDF。

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

要查看從部分完成的移動表單下載交互PDF的功能， [請按一下這裡](https://forms.enablementadobe.com/content/dam/formsanddocuments/xdptemplates/schengenvisa.xdp/jcr:content)。
下載PDF後，下一步是提交PDF以觸發工AEM作流。 此工作流將合併已提交PDF中的資料，並生成非互動式PDF以供審閱。

為此使用案例建立的自定義配置檔案可作為本教程資產的一部分使用。
