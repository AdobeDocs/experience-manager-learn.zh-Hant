---
title: 在AEM Forms中使用組合器服務
seo-title: 在AEM Forms中使用組合器服務
description: 使用AEM Forms中的組合器服務來組合多個pdf檔案
seo-description: 使用AEM Forms中的組合器服務來組合多個pdf檔案
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: 組合器
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 3%

---


# 在AEM Forms中使用組合器服務{#using-assembler-service-in-aem-forms}

本文提供相關資產，展示將多個PDF檔案拖放至瀏覽器，以及將已組合的PDF檔案儲存至檔案系統的能力。 以下是servlet的程式碼，用於組合使用瀏覽器上傳的pdf檔案。

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

若要讓此功能在您的AEM伺服器上運作

* 將[AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip)下載到本地系統。
* 使用[套件管理器](http://localhost:4502/crx/packmgr/index.jsp)上傳並安裝套件
* 下載[自定義文檔服務包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下載[使用服務用戶包開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 使用[felix web控制台](http://localhost:4502/system/console/bundles)部署和啟動套件組合
* 將瀏覽器指向[AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* 拖放幾個PDF檔案

>[!NOTE]
>
>確認AEM Forms安裝完成。 您的所有套件組合都必須處於作用中狀態。
>
>請務必新增 — 如本[安裝AEM Forms](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)中所述，引導委派RSA和BuncyCastle程式庫
>
>**本示範的注意事項**
>
> * 程式碼不會處理以XFA為基礎的PDF檔案
   >
   > 
* 請務必僅拖放PDF檔案
>
>







