---
title: 在Azure儲存體中儲存表單提交
description: 使用REST API在Azure儲存體中儲存表單資料
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-10-23T00:00:00Z
kt: 14238
source-git-commit: 23459de98420d2a489288df4a1b992c17d42972e
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---

# 從Azure儲存體擷取資料

本文說明如何以Azure儲存空間中儲存的資料填入最適化表單。
假設您已將最適化表單資料儲存在Azure儲存體，且現在想要將該資料預先填入最適化表單。

## 建立GET請求

下一步是撰寫程式碼，以使用blobID從Azure儲存體擷取資料。 已寫入下列程式碼以擷取資料。 此URL是使用OSGi設定中的sasToken和storageURI值以及傳遞至getBlobData函式的blobID所建構

```java
 @Override
public String getBlobData(String blobID) {
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    String httpGetURL = storageURI + blobID;
    httpGetURL = httpGetURL + sasToken;
    HttpGet httpGet = new HttpGet(httpGetURL);

    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    CloseableHttpResponse httpResponse = null;
    try {
        httpResponse = httpClient.execute(httpGet);
        HttpEntity httpEntity = httpResponse.getEntity();
        String blobData = EntityUtils.toString(httpEntity);
        log.debug("The blob data I got was " + blobData);
        return blobData;

    } catch (ClientProtocolException e) {

        log.debug("Got Client Protocol Exception " + e.getMessage());
    } catch (IOException e) {

        log.debug("Got IOEXception " + e.getMessage());
    }

    return null;
}
```

當最適化表單在URL中以guid引數轉譯時，與範本相關聯的自訂頁面元件會擷取並填入最適化表單中來自Azure儲存體的資料。
以下是和範本相關之頁面元件的jsp中的程式碼

```java
com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage azureStorage = sling.getService(com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage.class);


String guid = request.getParameter("guid");

if(guid!=null&&!guid.isEmpty())
{
    String dataXml = azureStorage.getBlobData(guid);
    slingRequest.setAttribute("data",dataXml);

}
```

## 測試解決方案

* [部署自訂OSGi套件](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [匯入自訂最適化表單範本以及與範本關聯的頁面元件](./assets/store-and-fetch-from-azure.zip)

* [匯入範例最適化表單](./assets/bank-account-sample-form.zip)

* 使用OSGi設定主控台，在Azure入口網站設定中指定適當的值。

* [預覽和提交BankAccount表單](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* 驗證資料是否儲存在您選擇的Azure儲存容器中。 複製Blob ID。

* [預覽銀行帳戶表單](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) 並針對URL中要預先填入Azure儲存體資料的表單，將Blob ID指定為GUID引數

