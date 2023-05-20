---
title: 在AEM Forms使用匯編服務
description: 使用AEM Forms的匯編服務匯編多個pdf檔案
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# 在AEM Forms使用匯編服務{#using-assembler-service-in-aem-forms}

本文為您提供了一些資源，用於演示將多個PDF檔案拖放到瀏覽器中並將已裝配的pdf檔案保存到檔案系統的能力。 以下是Servlet的代碼，它匯編了使用瀏覽器上載的pdf檔案。

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

使此功能在伺服器上AEM工作

* 下載 [裝配多個檔案.zip](assets/assemble-multiple-files.zip) 到本地系統。
* 使用 [軟體包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 下載[自定義文檔服務包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下載 [使用服務用戶包開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用 [felix Web控制台](http://localhost:4502/system/console/bundles)
* 將瀏覽器指向 [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* 拖放兩個檔案PDF檔案

>[!NOTE]
>
>確保AEM Forms安裝完成。 您的所有捆綁包都需要處於活動狀態。
>
>確保已添加 — 如本中所述，引導委託RSA和BouncyCastle庫 [安裝AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**本演示的注意事項**
>
> * 代碼不處理基於XFA的PDF文檔
>
> * 確保僅拖放PDF檔案
>
>

