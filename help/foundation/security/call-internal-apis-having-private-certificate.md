---
title: 呼叫具有私人憑證的內部API
description: 瞭解如何呼叫具有私人或自我簽署憑證的內部API。
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 332
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 0%

---

# 呼叫具有私人憑證的內部API

瞭解如何使用私人或自我簽署憑證，從AEM對Web API進行HTTPS呼叫。

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

根據預設，嘗試與使用自我簽署憑證的網頁API建立HTTPS連線時，連線會失敗並出現錯誤：

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

此問題通常發生於&#x200B;**API的SSL憑證未由可辨識的憑證授權單位(CA)**&#x200B;核發，且Java™應用程式無法驗證SSL/TLS憑證時。

讓我們瞭解如何使用[Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)和&#x200B;**AEM的全域TrustStore**，成功呼叫具有私人或自我簽署憑證的API。


## 使用HttpClient的典型API叫用代碼

下列程式碼會建立HTTPS連線至網頁API：

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

程式碼使用[Apache HttpComponent](https://hc.apache.org/)的[HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)程式庫類別及其方法。


## HttpClient並載入AEM TrustStore資料

若要呼叫具有&#x200B;_私人或自我簽署憑證_&#x200B;的API端點，[HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)的`SSLContextBuilder`必須以AEM的TrustStore載入，並用來促進連線。

請遵循下列步驟：

1. 以&#x200B;**管理員**&#x200B;的身分登入&#x200B;**AEM作者**。
1. 瀏覽至&#x200B;**AEM Author > Tools > Security > Trust Store**，然後開啟&#x200B;**全域信任存放區**。 如果第一次存取，請設定全域信任存放區的密碼。

   ![全域信任存放區](assets/internal-api-call/global-trust-store.png)

1. 若要匯入私用憑證，請按一下[選取憑證檔案]按鈕&#x200B;**，然後選取副檔名為`.cer`的憑證檔案。**&#x200B;按一下&#x200B;**提交**&#x200B;按鈕以匯入它。

1. 更新如下所示的Java™程式碼。 請注意，若要使用`@Reference`取得AEM的`KeyStoreService`，呼叫程式碼必須是OSGi元件/服務或Sling模型（其中使用`@OsgiService`）。

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * 將OOTB `com.adobe.granite.keystore.KeyStoreService` OSGi服務插入您的OSGi元件。
   * 使用`KeyStoreService`和`ResourceResolver`取得全域AEM TrustStore，`getAEMTrustStore(...)`方法會執行此動作。
   * 建立`SSLContextBuilder`的物件，請參閱Java™ [API詳細資料](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html)。
   * 使用`loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)`方法將全域AEM TrustStore載入`SSLContextBuilder`。
   * 在上述方法中傳遞`TrustStrategy`的`null`，可確保在API執行期間只有AEM信任憑證能夠成功。


>[!CAUTION]
>
>使用上述方法執行時，具有有效CA核發憑證的API呼叫會失敗。 遵循此方法時，只允許使用AEM信任憑證的API呼叫成功。
>
>使用[標準方法](#prototypical-api-invocation-code-using-httpclient)執行有效CA簽發憑證的API呼叫，這表示只有與私人憑證相關聯的API才應該使用先前提到的方法執行。

## 避免JVM金鑰存放區變更

使用私人憑證有效叫用內部API的傳統方法涉及修改JVM金鑰存放區。 這是透過使用Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549)命令匯入私人憑證來達成。

但是，此方法不符合安全性最佳實務，AEM透過使用&#x200B;**全域信任存放區**&#x200B;和[KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html)提供優越的選項。


## 解決方案套件

可從[這裡](assets/internal-api-call/REST-APIs.zip)下載影片中降級的範例Node.js專案。

AEM servlet程式碼可在WKND Sites專案的`tutorial/web-api-invocation`分支[參閱](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets)中使用。
