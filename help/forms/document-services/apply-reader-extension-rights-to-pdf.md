---
title: 將使用權套用至上傳的pdf
description: 將使用權套用至pdf
version: 6.4,6.5
feature: Reader Extensions
topic: Development
role: Developer
level: Experienced
exl-id: ea433667-81db-40f7-870d-b16630128871
source-git-commit: f1afccdad8d819604c510421204f59e7b3dc68e4
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---

# 套用Reader擴充功能

Reader擴充功能可讓您操控PDF檔案的使用權限。 使用權限與Acrobat中可用但Adobe Reader中無法使用的功能相關。 由「Reader擴充功能」控制的功能包括可向檔案新增註解、填寫表單及儲存檔案的功能。 添加了使用權限的PDF文檔稱為啟用權限的文檔。 在Adobe Reader中開啟啟用權限的PDF文檔的用戶可以執行為該文檔啟用的操作。
若要測試此功能，您可以嘗試此[link](https://forms.enablementadobe.com/content/forms/af/applyreaderextensions.html)。

若要完成此使用案例，我們必須執行下列動作：
* [將Reader擴充功能憑](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) 證新增 `fd-service` 至使用者。

## 建立自訂OSGi服務

建立將使用權限套用至檔案的自訂OSGi服務。 完成此作業的程式碼列於下方

```java
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.docassurance.client.api.DocAssuranceService;
import com.adobe.fd.docassurance.client.api.ReaderExtensionOptions;
import com.adobe.fd.readerextensions.client.ReaderExtensionsOptionSpec;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.adobe.fd.signatures.pdf.inputs.UnlockOptions;
import com.aemforms.ares.core.ReaderExtendPDF;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = ApplyUsageRights.class)
public class ApplyUsageRights implements ReaderExtendPDF {
        @Reference
        DocAssuranceService docAssuranceService;
        @Reference
        GetResolver getResolver;
        Logger logger = LoggerFactory.getLogger(ApplyUsageRights.class);
        @Override
        public Document applyUsageRights(Document pdfDocument, UsageRights usageRights) {

                ReaderExtensionsOptionSpec reOptionsSpec = new ReaderExtensionsOptionSpec(usageRights, "Sample ARES");
                UnlockOptions unlockOptions = null;
                ReaderExtensionOptions reOptions = ReaderExtensionOptions.getInstance();
                reOptions.setCredentialAlias("ares");

                reOptions.setResourceResolver(getResolver.getFormsServiceResolver());

                reOptions.setReOptions(reOptionsSpec);
                System.out.println("Applying Usage Rights");

                try {
                        Document readerExtended = docAssuranceService.secureDocument(pdfDocument, null, null, reOptions,
                                unlockOptions);
                        reOptions.getResourceResolver().close();
                        return readerExtended;
                } catch (Exception e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                }
                return null;
        }

}
```

## 建立Servlet以流化讀取器擴展PDF

下一步是使用POST方法建立Servlet，以將讀取器擴展PDF返回給用戶。 在這種情況下，系統會要求使用者將PDF儲存至其檔案系統。 這是因為PDF會以動態PDF呈現，而隨瀏覽器提供的pdf檢視器不會處理動態PDF。

以下是servlet的程式碼。 系統會從適用性表單的自訂提交動作叫用Servlet。
Servlet會建立UsageRights物件，並根據使用者在適用性表單中輸入的值來設定其屬性。 然後，Servlet將調用為此目的建立的服務的applyUsageRights方法。

```java
package com.aemforms.ares.core.servlets;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.StringReader;
import java.util.Map;
import javax.servlet.Servlet;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.request.RequestParameter;
import org.apache.sling.api.request.RequestParameterMap;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.readerextensions.client.UsageRights;
import com.aemforms.ares.core.impl.ApplyUsageRights;

@Component(service = Servlet.class, property = {

        "sling.servlet.methods=post",

        "sling.servlet.paths=/bin/applyrights"

})

public class GetReaderExtendedPDF extends SlingAllMethodsServlet {

        private static final long serialVersionUID = -883724052368090823 L;
        @Reference
        ApplyUsageRights applyRights;
        Logger logger = LoggerFactory.getLogger(GetReaderExtendedPDF.class);

        public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
                try {
                        System.out.println("the submitted data is " + xmlString);
                        DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
                        InputSource is = new InputSource();
                        is.setCharacterStream(new StringReader(xmlString));

                        return db.parse(is);
                } catch (Exception e) {
                        logger.debug(e.getMessage());
                }
                return null;
        }
        @Override
        protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                doPost(request, response);
        }
        @Override
        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
                System.out.println("In my do POST");
                UsageRights usageRights = new UsageRights();
                String submittedData = request.getParameter("jcr:data");
                org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
                usageRights.setEnabledDynamicFormFields(true);
                usageRights.setEnabledDynamicFormPages(true);
                usageRights.setEnabledFormDataImportExport(true);
                usageRights.setEnabledFormFillIn(Boolean.valueOf(submittedXml.getElementsByTagName("formfill").item(0).getTextContent()));
                usageRights.setEnabledComments(Boolean.valueOf(submittedXml.getElementsByTagName("comments").item(0).getTextContent()));
                usageRights.setEnabledEmbeddedFiles(Boolean.valueOf(submittedXml.getElementsByTagName("attachments").item(0).getTextContent()));
                usageRights.setEnabledDigitalSignatures(Boolean.valueOf(submittedXml.getElementsByTagName("digitalsignatures").item(0).getTextContent()));
                usageRights.setEnabledBarcodeDecoding(Boolean.valueOf(submittedXml.getElementsByTagName("barcode").item(0).getTextContent()));

                RequestParameterMap requestParameterMap = request.getRequestParameterMap();

                for (Map.Entry < String, RequestParameter[] > pairs: requestParameterMap.entrySet()) {
                        final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                        final org.apache.sling.api.request.RequestParameter param = pArr[0];

                        if (!param.isFormField()) {
                                try {
                                        System.out.println("Got form attachment!!!!" + param.getFileName());
                                        logger.debug("Got attachment!!!!" + param.getFileName());
                                        InputStream is = param.getInputStream();
                                        Document documentToReaderExtend = new Document(is);
                                        documentToReaderExtend = applyRights.applyUsageRights(documentToReaderExtend, usageRights);
                                        if (logger.isDebugEnabled()) {
                                                documentToReaderExtend.copyToFile(new File(param.getFileName().split("/")[1]));
                                        }

                                        InputStream fileInputStream = documentToReaderExtend.getInputStream();

                                        response.setContentType("application/pdf");
                                        response.addHeader("Content-Disposition", "attachment; filename=" + param.getFileName().split("/")[1]);
                                        response.setContentLength((int) fileInputStream.available());
                                        OutputStream responseOutputStream = response.getOutputStream();
                                        documentToReaderExtend.close();
                                        int bytes;
                                        while ((bytes = fileInputStream.read()) != -1) {
                                                responseOutputStream.write(bytes);
                                        }
                                        responseOutputStream.flush();
                                        responseOutputStream.close();

                                } catch (IOException e) {
                                        logger.debug("Exception in streaming pdf back to client  " + e.getMessage());
                                }
                        }

                }

        }

}
```

若要在本機伺服器上測試，請執行下列步驟：
1. [下載並安裝DevelopingWithServiceUser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. [下載並安裝ares.ares.core-ares套件組合](assets/ares.ares.core-ares.jar)。這有自訂服務和servlet，可套用使用權限並將pdf串流回
1. [匯入用戶端LIB和自訂提交](assets/applyaresdemo.zip)
1. [匯入最適化表單](assets/applyaresform.zip)
1. 將Reader擴充功能憑證新增至「fd-service」使用者。 確認別名為「ares」。
1. [預覽最適化表單](http://localhost:4502/content/dam/formsanddocuments/applyreaderextensions/jcr:content?wcmmode=disabled)
1. 選取適當的權限並上傳PDF檔案
1. 按一下提交以取得Reader延伸PDF
