---
title: 使用輸出服務產生PDF檔案
description: 將資料與XDP範本合併，以產生非互動式PDF
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-16384
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 8a5a4d11-12a2-462d-8684-a0c6ec0cac0e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 8%

---

# 使用輸出服務產生PDF檔案

[Output服務](https://javadoc.io/static/com.adobe.aem/aem-forms-sdk-api/2024.07.31.00-240800/com/adobe/fd/output/api/OutputService.html)是屬於AEM Document Services一部分的OSGi服務。 它支援各種AEM Forms Designer的輸出格式和設計功能。 輸出服務會轉換XFA範本和XML資料，以產生不同格式的列印檔案。

AEM Forms as a Cloud Service中的輸出服務與AEM Forms 6.5中的輸出服務非常類似，因此如果您熟悉AEM Forms 6.5中的輸出服務，轉換到AEM Forms as a Cloud Service應該相當簡單明瞭。

使用Output服務，您可以建立以下用途的應用程式：

+ 使用 XML 資料填寫範本檔案來產生最終表單文件。
+ 產生多種格式的輸出表單，包括非互動式PDF、PostScript、PCL和ZPL列印資料流。
+ 從 XFA 表單 PDF 產生列印 PDF。
+ 將多組資料與提供的範本合併，以大量產生PDF、PostScript、PCL和ZPL檔案。

此服務旨在用於AEM Forms as a Cloud Service執行個體的內容。 下列程式碼片段會使用`OutputService`在servlet中產生PDF檔案。

```java
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.PDFOutputOptions;
import com.adobe.fd.output.api.AcrobatVersion;
import com.adobe.aemfd.docmanager.Document;
import org.apache.sling.api.servlets.HttpConstants;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.api.servlets.SlingServletPaths;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.nio.charset.StandardCharsets;

@Component(service = Servlet.class,
           property = {
               "sling.servlet.methods=" + HttpConstants.METHOD_POST,
               "sling.servlet.paths=/bin/generateStatement"
           })
public class GenerateStatementServlet extends SlingAllMethodsServlet {

    @Reference
    private OutputService outputService;

    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Access the submitted form data
        String formData = request.getParameter("formData");

        // Define the XDP template document
        String templateName = "/content/dam/formsanddocuments/adobe/statement.xdp";
        Document xdpDocument = new Document(templateName);

        // Set the PDF output options
        PDFOutputOptions pdfOutputOptions = new PDFOutputOptions();
        pdfOutputOptions.setAcrobatVersion(AcrobatVersion.Acrobat_10);

        // Create the submitted data document from the form data
        InputStream inputStream = new ByteArrayInputStream(formData.getBytes(StandardCharsets.UTF_8));
        Document submittedData = new Document(inputStream);

        // Use the output service to generate the PDF output
        Document generatedPDF = outputService.generatePDFOutput(xdpDocument, submittedData, pdfOutputOptions);

        // Process the generated PDF as per your use case        
    }
}
```
