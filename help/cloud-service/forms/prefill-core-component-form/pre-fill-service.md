---
title: 根據最適化表單預先填寫核心元件
description: 瞭解如何使用資料預先填寫最適化表單
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14675
duration: 98
exl-id: 1cd385b3-7e00-4f75-b728-1b47f7a8fb2a
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 1%

---

# 實作DataProvider介面

使用最新原型建立AEM專案，並將下列程式碼新增至專案。
通常資料會從外部資料來源擷取，例如資料庫、Azure儲存體、S3儲存貯體或任何其他來源。 您必須撰寫程式碼才能擷取資料。

```java
package com.aemforms.prefill;

import com.adobe.forms.common.service.*;
import com.aemforms.saveandfetchfromazure.StoreAndFetchDataFromAzureStorage;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.forms.common.*;

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.UnsupportedEncodingException;
import java.util.List;
import java.util.Map;

@Component(
        service={DataProvider.class, CustomAFPrefillService.class},
        immediate = true
)
public class CustomAFPrefillService implements DataProvider {
    @Reference
    private DataManager dataManager;

    @Reference
    private StoreAndFetchDataFromAzureStorage storeandfetch;

    private transient Logger  logger = LoggerFactory.getLogger(CustomAFPrefillService.class);

    @Override
    public PrefillData getPrefillData(DataOptions dataOptions) throws FormsException {
        logger.info("Invoking AF custom prefill service");
        // get the value of the request parameter called guid from the url
        String guid = (String) dataOptions.getExtras().get("guid");
        
        // fetch the data associated with the guid using a custom OSGi service or in any other manner
        
        String data = storeandfetch.getBlobData(guid);
        InputStream dataInputStream = null;
        ContentType contentType = dataOptions.getContentType();

        Map<String, Object> extras = dataOptions.getExtras();
        List<FileAttachmentWrapper> fileAttachmentWrappers = null;
        PrefillData prefillData = null;
        Map<String, String> customContext = null;
        if(extras != null && extras.containsKey(DataManager.UNIQUE_ID) && dataManager != null) {
            String dataKey = extras.get(DataManager.UNIQUE_ID).toString();
            if(dataManager.get(dataKey) != null) {
                data = (String) dataManager.get(dataKey);
                fileAttachmentWrappers = (List<FileAttachmentWrapper>) dataManager.get(DataManager.getFileAttachmentMapKey(dataKey));
            }
            customContext = dataManager.getCustomContext(dataKey);
        }
        if(StringUtils.isNotBlank(data)) {
            dataInputStream = getDataInputStream(data);
            prefillData = new PrefillData(dataInputStream, contentType, fileAttachmentWrappers, customContext);
        }
        return prefillData;


    }

    @Override
    public String getServiceName() {
        return "Core Custom Pre-fill Service";
    }

    @Override
    public String getServiceDescription() {
         return "Core Custom Pre-fill Service";
    }
    private InputStream getDataInputStream(String data) {
        InputStream dataInputStream = null;
logger.info("got data  "+data);
        try {
            dataInputStream = new ByteArrayInputStream(data.getBytes("UTF-8"));
        } catch (UnsupportedEncodingException e) {
            logger.error("[Custom AF Prefill] Exception in custom af prefill service", e);
        }
        return dataInputStream;
    }
}
```

## 資料管理員

在您的專案中包含下列程式碼。 上面列出的程式碼會使用這個DataManager類別

```java
import java.io.Serializable;
import java.util.*;
import java.util.concurrent.ConcurrentHashMap;


import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;


@Component(
        service=DataManager.class,
        immediate = true
)
public class DataManager implements Serializable {
    private static int MAX_ENTRIES = 10000;
    private static int INITIAL_CAPACITY = 100;
    public final static String UNIQUE_ID = "prefillId";

    public static String getFileAttachmentMapKey(String uuid) {
        return uuid + "fileAttachmentMap";
    }

    private Map<Object, Object> cache;

    protected void activate() {
        cache = Collections.synchronizedMap(new LinkedHashMap<Object, Object>(INITIAL_CAPACITY + 1, 1.0f, true) {
            /**
             * Returns <tt>true</tt> if this <code>GuideCache</code> has more entries than the maximum specified when it was
             * created.
             *
             * <p>
             * This method <em>does not</em> modify the underlying <code>Map</code>; it relies on the implementation of
             * <code>LinkedHashMap</code> to do that, but that behavior is documented in the JavaDoc for
             * <code>LinkedHashMap</code>.
             * </p>
             *
             * @param eldest
             *            the <code>Entry</code> in question; this implementation doesn't care what it is, since the
             *            implementation is only dependent on the size of the cache
             * @return <tt>true</tt> if the oldest
             * @see java.util.LinkedHashMap#removeEldestEntry(java.util.Map.Entry)
             */
            protected boolean removeEldestEntry(final Map.Entry eldest) {
                return size() > MAX_ENTRIES;
            }
        });
    }


    public Object get(String key) {
        Object value = null;
        if (cache.containsKey(key)) {
            value = cache.get(key);
        }
        return value;
    }

    public Map <String, String> getCustomContext(String id) {
        Map<String, String> customContext = new HashMap<>();
        customContext.put(UNIQUE_ID, id);
        return customContext;
    }
    public Object put(String key, Object value) {
        return cache.put(key, value);
    }
}
```

## 部署程式碼

您可以在本機雲端就緒環境中部署程式碼，或使用Cloud Manager將程式碼部署到雲端例項。

### 後續步驟

[測試解決方案](./test-solution.md)
