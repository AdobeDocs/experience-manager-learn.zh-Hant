---
title: 從一個資料檔案產生多個PDF
description: OutputService提供了多種使用表單設計和資料建立文檔的方法，以便與表單設計合併。 了解如何從包含多個個別記錄的大型xml產生多個pdf。
feature: 輸出服務
version: 6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '526'
ht-degree: 1%

---


# 從一個xml資料檔案生成一組PDF文檔

OutputService提供了多種使用表單設計和資料建立文檔的方法，以便與表單設計合併。 以下文章說明如何從包含多個個別記錄的大型xml產生多個pdf的使用案例。
以下是包含多個記錄的xml檔案的螢幕擷取畫面。

![多記錄 — xml](assets/multi-record-xml.PNG)

資料xml有2條記錄。 每個記錄都由form1元素表示。 此xml會傳遞到OutputService [generatePDFOutputBatch方法](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html)我們會獲得pdf文檔清單（每條記錄一個）
generatePDFOutputBatch方法的簽名採用以下參數

* 範本 — 包含範本的對應，以關鍵字識別
* data — 包含xml資料文檔的映射，由鍵標識
* pdfOutputOptions — 配置pdf生成的選項
* batchOptions — 配置batch的選項

>[!NOTE]
>
>此使用案例在此[website](https://forms.enablementadobe.com/content/samples/samples.html?query=0)上提供即時範例。

## 使用案例詳細資訊{#use-case-details}

在此使用案例中，我們將提供簡單的網頁介面，以上傳範本和資料(xml)檔案。 檔案上傳完成且POST要求傳送至AEM servlet後。 此Servlet提取文檔並調用OutputService的generatePDFOutputBatch方法。 產生的PDF會壓縮為Zip檔案，供一般使用者從網頁瀏覽器下載。

## Servlet程式碼{#servlet-code}

以下是servlet中的程式碼片段。 程式碼會從請求中擷取範本(xdp)和資料檔案(xml)。 模板檔案將保存到檔案系統。 建立了兩個映射 — templateMap和dataFileMap，它們分別包含模板和xml(data)檔案。 然後會呼叫DocumentServices服務的generateMultipleRecords方法。

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

### 介面實作程式碼{#Interface-Implementation-Code}

下列程式碼會使用OutputService的generatePDFOutputBatch產生多個pdf，並將包含pdf檔案的zip檔案傳回至呼叫servlet

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

若要在您的伺服器上測試此功能，請依照下列指示操作：

* [將zip檔案內容下載並解壓縮至您的檔案系統](assets/mult-records-template-and-xml-file.zip)。此zip檔案包含範本和xml資料檔案。
* [將瀏覽器指向Felix Web Console](http://localhost:4502/system/console/bundles)
* [部署DevelopingWithServiceUser套件組合](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)。
* [部署自定義AEMFormsDocumentServices包](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。自定義包使用OutputService API生成PDF
* [將瀏覽器指向包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [匯入並安裝套件](assets/generate-multiple-pdf-from-xml.zip)。此套件包含html頁面，可讓您放置範本和資料檔案。
* [將瀏覽器指向MultiRecords.html](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* 拖放範本和xml資料檔案
* 下載已建立的zip檔案。 此zip檔案包含輸出服務產生的pdf檔案。

>[!NOTE]
>有多種方式可觸發此功能。 在此範例中，我們使用Web介面來放置範本和資料檔案，以展示功能。

