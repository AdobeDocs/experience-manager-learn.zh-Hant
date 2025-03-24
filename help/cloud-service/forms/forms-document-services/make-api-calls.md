---
title: 使用usagerights API
description: 將使用許可權套用至提供的PDF的程式碼範例
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a4e2132b-3cfd-4377-8998-6944365edec5
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---

# 進行API呼叫

## 套用使用許可權

取得存取Token後，下一步就是提出API要求，將使用許可權套用至指定的PDF。 這涉及在請求標頭中包含存取權杖以驗證呼叫，確保安全和授權處理檔案。

以下函式套用使用許可權

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## 功能劃分：



* **設定API端點和承載**
   * 使用提供的`endPoint`和預先定義的`BUCKET`建構API URL。
   * 定義JSON字串(`usageRights`)，指定要套用的許可權，例如：
      * 評論
      * 內嵌檔案
      * 表單填寫
      * 表單資料匯出

* **載入PDF檔案**
   * 從`pdffiles`目錄中擷取`withoutusagerights.pdf`檔案。
   * 記錄錯誤，並會在找不到檔案時結束。

* **準備HTTP要求**
   * 將PDF檔案讀入位元組陣列。
   * 使用`MultipartEntityBuilder`建立包含下列內容的多部分要求：
      * 作為二進位主體的PDF檔案。
      * `usageRights` JSON為文字內文。
   * 設定具有標頭的HTTP `POST`要求：
      * `Authorization: Bearer <accessToken>`以進行驗證。
      * `X-Adobe-Accept-Experimental: 1` （可能是API相容性的必要專案）。

* **傳送要求與處理回應**
   * 使用`httpClient.execute(httpPost)`執行HTTP要求。
   * 讀取回應(預期為已套用使用許可權的更新PDF)。
   * 在`SAVE_LOCATION`將接收的PDF內容寫入&#x200B;**&quot;ReaderExtended.pdf&quot;**。

* **錯誤處理與清理**
   * 擷取並記錄任何`IOException`錯誤。
   * 確保`finally`區塊中的所有資源（串流、HTTP使用者端、回應）已正確關閉。

以下是叫用applyUsageRights函式的main.java程式碼

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

`main`方法會透過從`AccessTokenService`呼叫`getAccessToken()`來初始化，預期此方法會傳回有效的權杖。

* 然後它會從`DocumentGeneration`類別呼叫`applyUsageRights()`，並傳入：
   * 已擷取的`accessToken`
   * 用於套用使用許可權的API端點。


## 後續步驟

[部署範例專案](sample-project.md)
