---
title: 擷取儲存的最適化表單
description: 使用儲存的資料轉譯最適化表單的Servlet
feature: 適用性表單
type: Tutorial
version: 6.4,6.5
kt: 6553
thumbnail: 6553.jpg
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 2%

---

# 檢索保存的表單

下一步是建立Servlet，以便使用儲存的資料及其附件呈現最適化表單。
在驗證OTP代碼後，將執行以下Servlet代碼。 從資料庫中擷取與唯一應用程式ID相關聯的適用性表單資料及其檔案附件映射。 請求物件會填入儲存的最適化表單資料和檔案附件對應。 然後，請求會轉送，呈現預先填入原始資料及其附件的「storeafwithattachments」表單。

```java
package store.and.fetch.core.servlets;

import java.io.IOException;
import java.io.StringReader;

import javax.servlet.RequestDispatcher;
import javax.servlet.Servlet;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathExpressionException;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.xml.sax.InputSource;
import org.xml.sax.SAXException;
import com.day.cq.wcm.api.WCMMode;
import store.and.fetch.core.*;
@Component(service = {
    Servlet.class
}, property = {
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/renderaf"
})

public class RenderForm extends SlingAllMethodsServlet {
    /**
     * 
     */
    private static final Logger log = LoggerFactory.getLogger(RenderForm.class);
    private static final long serialVersionUID = 1 L;
    @Reference
    AemFormsAndDB aemFormsAndDB;
    public org.w3c.dom.Document w3cDocumentFromStrng(String xmlString) {
        try {
            log.debug("Inside w3cDocumentFromString");
            DocumentBuilder db = DocumentBuilderFactory.newInstance().newDocumentBuilder();
            InputSource is = new InputSource();
            is.setCharacterStream(new StringReader(xmlString));
            return db.parse(is);
        } catch (Exception e) {
            log.debug(e.getMessage());
        }
        return null;
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("*****In my Render Form Servlet*****");
        String submittedData = request.getParameter("jcr:data");
        String applicationNo = "/afData/afUnboundData/data/ApplicationNumber";
        org.w3c.dom.Document submittedXml = w3cDocumentFromStrng(submittedData);
        XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
        try {
            org.w3c.dom.Node applicationNode = (org.w3c.dom.Node) xPath.compile(applicationNo).evaluate(submittedXml, javax.xml.xpath.XPathConstants.NODE);
            log.debug("The application number we got was " + applicationNode.getTextContent());
            org.json.JSONObject afDataObject = aemFormsAndDB.getAFFormDataWithAttachments(applicationNode.getTextContent());
            log.debug("The data I got was " + afDataObject.toString());
            request.setAttribute("data", afDataObject.get("afData"));
            org.json.JSONObject customMap = new org.json.JSONObject();
            customMap.put("fileAttachmentMap", afDataObject.get("afAttachments"));
            request.setAttribute("customContextProperty", customMap.toString());
            ServletContext sc = getServletContext();
            RequestDispatcher dispatcher = sc.getRequestDispatcher("/content/forms/af/storeafwithattachments.html");
            WCMMode.DISABLED.toRequest(request);
            dispatcher.forward(request, response);
        } catch (JSONException | ServletException | IOException | XPathExpressionException e) {
            log.debug("The error message is " + e.getMessage());
        }

 }

}
```
