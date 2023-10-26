---
title: 在Azure儲存體中儲存表單提交
description: 使用REST API在Azure儲存體中儲存表單資料
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-08-14T00:00:00Z
kt: 13781
exl-id: 2bec5953-2e0c-4ae6-ae98-34492d4cfbe4
source-git-commit: 5e761ef180182b47c4fd2822b0ad98484db23aab
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# 在Azure儲存體中儲存表單提交

本文說明如何進行REST呼叫，以將提交的AEM Forms資料儲存在Azure儲存空間。
若要將提交的表單資料儲存在Azure儲存體，必須執行下列步驟。

## 建立Azure儲存體帳戶

[登入您的Azure入口網站帳戶並建立儲存帳戶](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal#create-a-storage-account-1). 為儲存體帳戶提供有意義的名稱，按一下[檢閱]，然後按一下[建立]。 這會使用所有預設值建立您的儲存體帳戶。 出於本文的目的，我們已命名儲存帳戶 `aemformstutorial`.


## 建立容器

接下來，我們需要建立一個容器，用於儲存表單提交作業中的資料。
在儲存體帳戶頁面中，按一下左側的「容器」功能表專案，並建立名為的容器 `formssubmissions`. 請確定公用存取層級已設為私人
![容器](./assets/new-container.png)

## 在容器上建立SAS

我們將使用共用存取簽章或SAS授權方法來與Azure儲存容器互動。
導覽至儲存帳戶中的容器，按一下省略符號並選取產生SAS選項，如熒幕擷取畫面所示
![sas-on-container](./assets/sas-on-container.png)
請務必指定適當的許可權和適當的結束日期（如底下熒幕擷圖所示），然後按一下「產生SAS權杖和URL」 。 複製Blob SAS權杖和Blob SAS url。 我們將使用這兩個值來進行HTTP呼叫
![shared-access-key](./assets/shared-access-signature.png)


## 提供Blob SAS權杖和儲存URI

若要讓程式碼更通用，可以使用OSGi設定來設定這兩個屬性，如下所示。 此 _**aemformstutorial**_ 是儲存帳戶的名稱， _**formsubmitments**_ 是儲存資料的容器。
![OSGI設定](./assets/azure-portal-osgi-configuration.png)


## 建立PUT請求

下一步是建立PUT要求，以將提交的表單資料儲存在Azure儲存體。 每個表單提交都需要以唯一的BLOB ID識別。 唯一的BLOB ID通常會在您的程式碼中建立，並插入PUT請求的URL中。
以下是PUT請求的部分URL。 此 `aemformstutorial` 是儲存帳戶的名稱，formsubmissions是將以唯一BLOB ID儲存資料的容器。 URL的其餘部分將維持不變。
https://aemformstutorial.blob.core.windows.net/formsubmissions/blobid/sastoken下列是使用PUT要求將提交的表單資料儲存在Azure儲存體的函式。 請注意URL中使用容器名稱和uuid。 您可以使用下列範常式式碼建立OSGi服務或Sling servlet，並將表單提交專案儲存在Azure儲存空間。

```java
 public String saveFormDatainAzure(String formData) {
    log.debug("in SaveFormData!!!!!" + formData);
    String sasToken = azurePortalConfigurationService.getSASToken();
    String storageURI = azurePortalConfigurationService.getStorageURI();
    log.debug("The SAS Token is " + sasToken);
    log.debug("The Storage URL is " + storageURI);
    org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    UUID uuid = UUID.randomUUID();
    String putRequestURL = storageURI + uuid.toString();
    putRequestURL = putRequestURL + sasToken;
    HttpPut httpPut = new HttpPut(putRequestURL);
    httpPut.addHeader("x-ms-blob-type", "BlockBlob");
    httpPut.addHeader("Content-Type", "text/plain");

    try {
        httpPut.setEntity(new StringEntity(formData));

        CloseableHttpResponse response = httpClient.execute(httpPut);
        log.debug("Response code " + response.getStatusLine().getStatusCode());
        if (response.getStatusLine().getStatusCode() == 201) {
            return uuid.toString();
        }
    } catch (IOException e) {
        log.error("Error: " + e.getMessage());
        throw new RuntimeException(e);
    }
    return null;

}
```

## 驗證儲存在容器中的資料

![form-data-in-container](./assets/form-data-in-container.png)

## 測試解決方案

* [部署自訂OSGi套件](./assets/SaveAndFetchFromAzure.core-1.0.0-SNAPSHOT.jar)

* [匯入自訂最適化表單範本以及與範本關聯的頁面元件](./assets/store-and-fetch-from-azure.zip)

* [匯入範例最適化表單](./assets/bank-account-sample-form.zip)

* 使用OSGi設定主控台，在Azure入口網站設定中指定適當的值
* [預覽和提交BankAccount表單](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled)

* 驗證資料是否儲存在您選擇的Azure儲存容器中。 複製Blob ID。
* [預覽銀行帳戶表單](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/bankaccount/jcr:content?wcmmode=disabled&amp;guid=dba8ac0b-8be6-41f2-9929-54f627a649f6) 並針對URL中要預先填入Azure儲存體資料的表單，將Blob ID指定為GUID引數

