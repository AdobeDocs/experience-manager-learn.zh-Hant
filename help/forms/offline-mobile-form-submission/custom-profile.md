---
title: 在HTML5表單提交時觸發AEM工作流程 — 建立自訂設定檔
description: 建立自訂設定檔，從已部分填滿的HTML5表單下載包含資料的互動式pdf
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: b6e3acee-4a07-4d00-b3a1-f7aedda21e6e
duration: 102
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# 建立自訂設定檔

在本部分中，我們將建立[自訂設定檔。](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html)設定檔負責將XDP轉譯為HTML。 提供預設設定檔，可立即將XDP轉譯為HTML。 它代表自訂版的Mobile Forms轉譯服務。 您可以使用Mobile Form Rendition服務來自訂Mobile Forms的外觀、行為和互動。 在我們的自訂設定檔中，我們將使用Guidelbridge API擷取填入行動表單的資料。 然後，這些資料會傳送到自訂servlet，接著會產生互動式PDF，並將其串流回呼叫的應用程式。

使用`formBridge` JavaScript API取得表單資料。 我們使用`getDataXML()`方法：

```javascript
window.formBridge.getDataXML({success:suc,error:err});
```

在成功處理常式方法中，我們會呼叫在AEM中執行的自訂servlet。 此servlet將會轉譯並傳回互動式pdf，其中包含行動表單中的資料

```javascript
var suc = function(obj) {
    let xhr = new XMLHttpRequest();
    var data = obj.data;
    let postURL ="/bin/generateinteractivepdf";
    console.log("The data: " + data);
    xhr.open('POST',postURL);
    xhr.responseType = 'blob';
    let formData = new FormData();
    formData.append("formData", data);
    formData.append("xdpPath", window.location.pathname);
    let parts = window.location.pathname.split("/");
    let formName = parts[parts.length-2];
    const updatedFilename = formName.replace(/\.xdp$/, '.pdf');

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
                a.download = updatedFilename;
                a.click();
                window.URL.revokeObjectURL(url);
        }
    }
}
```

## 產生互動式PDF

以下是servlet程式碼，負責呈現互動式pdf並將pdf傳回給呼叫的應用程式。 此servlet會叫用自訂DocumentServices OSGi服務的`mobileFormToInteractivePdf`方法。

```java
package com.aemforms.mobileforms.core.servlets;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import java.io.*;
import java.nio.charset.StandardCharsets;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/generateInteractivePDF"})
public class GeneratePDFFromMobileFormData extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final transient Logger logger = LoggerFactory.getLogger(this.getClass());
    @Reference
    GeneratePDFFromMobileForm generatePDFFromMobileForm;

    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) throws IOException {
        String dataXml = request.getParameter("formData");
        logger.debug("The data is "+dataXml);
        InputStream inputStream = new ByteArrayInputStream(dataXml.getBytes(StandardCharsets.UTF_8));
        Document submittedXml = new Document(inputStream);
       Document interactivePDF =  generatePDFFromMobileForm.generateInteractivePDF(submittedXml,request.getParameter("xdpPath"));
        interactivePDF.copyToFile(new File("interactive.pdf"));
        try {
            InputStream fileInputStream = interactivePDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
            response.setContentLength(fileInputStream.available());
            ServletOutputStream responseOutputStream = response.getOutputStream();

            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }

            responseOutputStream.flush();
            responseOutputStream.close();
        }
        catch(Exception e)
        {
            logger.debug(e.getMessage());
        }


    }
}
```

### 呈現互動式PDF

下列程式碼會使用[Forms服務API](https://helpx.adobe.com/tw/aem-forms/6/javadocs/com/adobe/fd/forms/api/FormsService.html)，以行動表單中的資料來呈現互動式PDF。

```java
package com.aemforms.mobileforms.core.documentservices.impl;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.AcrobatVersion;
import com.adobe.fd.forms.api.FormsService;
import com.adobe.fd.forms.api.FormsServiceException;
import com.adobe.fd.forms.api.PDFFormRenderOptions;
import com.aemforms.mobileforms.core.documentservices.GeneratePDFFromMobileForm;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(service = GeneratePDFFromMobileForm.class, immediate = true)
public class GeneratePDFFromMobileFormImpl implements GeneratePDFFromMobileForm {
    @Reference
    FormsService formsService;
    private   transient Logger log = LoggerFactory.getLogger(this.getClass());

    @Override
    public Document generateInteractivePDF(Document xmlData, String xdpPath)
    {
        String uri = "crx:///content/dam/formsanddocuments";
        String xdpName = xdpPath.substring(31, xdpPath.lastIndexOf("/jcr:content"));
        log.debug("####In mobile form to interactive pdf####   " + xdpName);
        PDFFormRenderOptions renderOptions = new PDFFormRenderOptions();
        renderOptions.setAcrobatVersion(AcrobatVersion.Acrobat_11);
        renderOptions.setContentRoot(uri);
        Document interactivePDF = null;
        try
        {
            interactivePDF = this.formsService.renderPDFForm(xdpName, xmlData, renderOptions);
        }
        catch (FormsServiceException e)
        {
            log.error(e.getMessage());
        }

        return interactivePDF;

    }
}
```

## 後續步驟

[處理表單提交](./handle-form-submission.md)
