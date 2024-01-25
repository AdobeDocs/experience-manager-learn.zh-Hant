---
title: 新增使用者指定中繼資料標籤的教學課程
description: 建立自訂提交，將含有中繼資料標籤的表單資料儲存在Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 99
exl-id: 5cd5e37e-9881-4fce-a0cb-402d738f83ae
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---

# 建立自訂提交以處理表單提交

AEM Forms CS提供立即可用的提交動作，可在Azure中儲存表單資料，但無法在blob上建立blob索引標籤。 為了滿足使用案例，已建立自訂提交服務，以將提交的資料儲存在Azure中，並使用表單中標籤為可搜尋的欄位來建立blob索引資料標籤。

[以下提供核心元件型最適化表單的範例自訂提交處理常式](https://github.com/adobe/aem-core-forms-components/blob/master/it/core/src/main/java/com/adobe/cq/forms/core/components/it/service/CustomAFSubmitService.java#L56). 已寫入下列自訂提交以處理表單提交

```java
package com.aemforms.saveandfecthfromazure.prefill;

import com.adobe.aemds.guide.common.GuideValidationResult;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.adobe.forms.common.service.FileAttachmentWrapper;
import com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage;
import org.apache.commons.lang3.StringUtils;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.UUID;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class CustomAFSubmitService implements FormSubmitActionService {
    private static final String serviceName = "Store Form Submission with Metadata tags in Azure";
    private static Logger logger = LoggerFactory.getLogger(CustomAFSubmitService.class);

    @Reference
    DataManager dataManager;
    @Reference
    StoreAndFetchDataFromAzureStorage storeAndFetchDataFromAzureStorage;

    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        Map<String, Object> result = new HashMap<>();
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.FALSE);
        try {

            String guideContainerPath = formSubmitInfo.getFormContainerPath();

            String submittedFormName = formSubmitInfo.getFormContainerResource().getParent().getParent().getName();
            logger.debug("The submitted form name is "+submittedFormName);
            String data = formSubmitInfo.getData();
            logger.debug(data);
            String metadataTags = storeAndFetchDataFromAzureStorage.getMetaDataTags(submittedFormName,formSubmitInfo.getFormContainerPath(),formSubmitInfo.getFormContainerResource().getResourceResolver().adaptTo(Session.class),data);
            String uniqueID = UUID.randomUUID().toString();
            logger.debug("The metadata tags are  "+metadataTags);
            // The code for saveFormDatainAzure is listed in this article
            logger.debug(storeAndFetchDataFromAzureStorage.saveFormDatainAzure(data,metadataTags));
            
            if(dataManager != null) {
                dataManager.put(uniqueID, data);
               
            }
            logger.info("AF Submission successful using custom submit service for: {}", guideContainerPath);
            result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
            result.put(DataManager.UNIQUE_ID, uniqueID);
            // adding id here so that this available in redirect parameters in final thank you page
            Map<String, Object> redirectParamMap = new HashMap<String, Object>() {{
                put(DataManager.UNIQUE_ID, uniqueID);
            }};
            // todo: move this to constant, once forms SDK is released
            result.put("fd:redirectParameters", redirectParamMap);
        } catch (Exception ex) {
            logger.error("Error while using the AF Submit service", ex);
            GuideValidationResult guideValidationResult = new GuideValidationResult();
            guideValidationResult.setOriginCode("500");
            guideValidationResult.setErrorMessage("Internal server error");
            result.put(GuideConstants.FORM_SUBMISSION_ERROR, guideValidationResult);
        }
        return result;
    }

}
```

## 服務實作saveFormDatainAzure

```java
    @Override
    public String saveFormDatainAzure(String formData,String metaDataTags) {
        String sasToken = azurePortalConfigurationService.getSASToken();
        String storageURI = azurePortalConfigurationService.getStorageURI();
        logger.debug("The SAS Token is "+sasToken);
        logger.debug("The Storage URL is "+storageURI);
        logger.debug("The form data is "+formData);
        org.apache.http.impl.client.CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        UUID uuid = UUID.randomUUID();
        String putRequestURL = storageURI+uuid.toString();
        putRequestURL = putRequestURL+sasToken;
        logger.debug("The put Request "+putRequestURL);
        HttpPut httpPut = new HttpPut(putRequestURL);
        httpPut.addHeader("x-ms-blob-type","BlockBlob");
        httpPut.addHeader("Content-Type","text/plain");
        httpPut.addHeader("x-ms-tags",metaDataTags);

        try
        {
            httpPut.setEntity(new StringEntity(formData));
            CloseableHttpResponse response = httpClient.execute(httpPut);
            logger.debug("Response code "+response.getStatusLine().getStatusCode());
            if(response.getStatusLine().getStatusCode() == 201)
            {
                return uuid.toString();
            }
        }
        catch (IOException e)
        {
            logger.debug("Error: "+e.getMessage());
            throw new RuntimeException(e);
        }
        return null;
    }
```
