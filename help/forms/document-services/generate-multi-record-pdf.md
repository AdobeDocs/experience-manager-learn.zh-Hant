---
title: 從一個資料檔案生成多個PDF
description: OutputService提供了多種方法，可使用表單設計和資料建立文檔，以與表單設計合併。 瞭解如何從包含多個單獨記錄的一個大型xml中生成多個pdf。
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 58582acd-cabb-4e28-9fd3-598d3cbac43c
last-substantial-update: 2020-01-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 0%

---

# 從一個xml資料檔案生成一組PDF文檔

OutputService提供了多種方法，可使用表單設計和資料建立文檔，以與表單設計合併。 以下文章說明了使用案例從包含多個單個記錄的大型xml中生成多個pdf。
下面是包含多個記錄的xml檔案的螢幕抓圖。

![多記錄XML](assets/multi-record-xml.PNG)

資料xml有2條記錄。 每個記錄由form1元素表示。 此xml將傳遞給OutputService [generatePDFOutputBatch方法](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html) 我們得到pdf文檔清單（每條記錄1個）generatePDFOutputBatch方法的簽名採用以下參數

* 模板 — 包含模板的映射，由鍵標識
* data — 包含XML資料文檔的映射，由鍵標識
* pdfOutputOptions — 配置pdf生成的選項
* batchOptions — 用於配置批處理的選項



## 用例詳細資訊{#use-case-details}

在此使用情形中，我們將提供一個簡單的Web介面來上載模板和資料(xml)檔案。 完成檔案上載並將POST請求發送到AEMservlet。 此Servlet提取文檔並調用OutputService的generatePDFOutputBatch方法。 生成的pdf將壓縮到一個zip檔案中，供最終用戶從Web瀏覽器下載。

## Servlet代碼{#servlet-code}

以下是servlet中的代碼段。 代碼從請求中提取模板(xdp)和資料檔案(xml)。 模板檔案將保存到檔案系統。 建立了兩個映射 — templateMap和dataFileMap，分別包含模板和xml(data)檔案。 然後調用以生成DocumentServices服務的MultipleRecords方法。

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### 介面實現代碼{#Interface-Implementation-Code}

以下代碼使用OutputService的generatePDFOutputBatch生成多個pdf檔案，並將包含pdf檔案的zip檔案返回給調用的servlet

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### 在伺服器上部署{#Deploy-on-your-server}

要在伺服器上test此功能，請遵循以下說明：

* [將zip檔案內容下載並解壓到檔案系統](assets/mult-records-template-and-xml-file.zip).此zip檔案包含模板和xml資料檔案。
* [將瀏覽器指向Felix Web控制台](http://localhost:4502/system/console/bundles)
* [部署DevelopingWithServiceUser捆綁包](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)。
* [部署自定義AEMFormsDocumentServices捆綁包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).使用OutputService API生成PDF的自定義包
* [將瀏覽器指向包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [導入並安裝包](assets/generate-multiple-pdf-from-xml.zip)。 此包包含html頁，可用於刪除模板和資料檔案。
* [將瀏覽器指向MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* 將模板和xml資料檔案拖放到一起
* 下載建立的zip檔案。 此zip檔案包含由輸出服務生成的pdf檔案。

>[!NOTE]
>有多種方法可觸發此功能。 在本示例中，我們使用Web介面刪除模板和資料檔案來演示該功能。
