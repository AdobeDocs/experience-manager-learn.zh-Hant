---
title: 為元件建立Sling模型
description: 為元件建立Sling模型
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# 為元件建立Sling模型

AEM中的Sling模型是Java型架構，用來簡化元件後端邏輯的開發。 它可讓開發人員使用註解，將資料從AEM資源（JCR節點）對應至Java物件，提供簡潔有效的方法處理元件的動態資料。
此類別CountriesDropDownImpl是AEM (Adobe Experience Manager)專案中CountriesDropDown介面的實作。 這會提供下拉式元件，讓使用者可以根據所選大陸選取國家/地區。 下拉式清單資料會從儲存在AEM DAM (Digital Asset Manager)中的JSON檔案動態載入。

類別&#x200B;**中的**&#x200B;欄位

* **multiSelect**：指出下拉式清單是否允許多個選項。
使用預設值為false的@ValueMapValue從元件屬性插入。
* **request**：代表目前的HTTP要求。 對於存取特定內容的資訊很有用。
* **大陸**：儲存下拉式清單的選定大陸（例如，「亞洲」、「歐洲」）。
從元件的屬性對話方塊插入，若未提供任何內容，預設值為「asia」。
* **resourceResolver**：用於存取和控制AEM存放庫中的資源。
* **jsonData**： JSONObject會從與所選大陸相對應的JSON檔案中儲存剖析的資料。

類別&#x200B;**中的**&#x200B;方法

* **getContinent()**&#x200B;傳回大陸欄位值的簡單方法。
記錄傳回的值以供偵錯。
* **init()**&#x200B;以@PostConstruct註解的生命週期方法，在建構類別並插入相依性之後執行。根據大陸值動態建構JSON檔案的路徑。
使用resourceResolver從AEM DAM擷取JSON檔案。
將檔案調整至資產、讀取其內容，並將其剖析為JSONObject。
記錄此程式期間的任何錯誤或警告。
* **getEnums()**&#x200B;從剖析的JSON資料中擷取所有索引鍵（國家/地區代碼）。
將索引鍵依字母順序排序，並傳回為陣列。
記錄要傳回的國家代碼。
* **getEnumNames()**&#x200B;從剖析的JSON資料中擷取所有國家/地區名稱。
依字母順序排序名稱，並將它們以陣列傳回。
記錄國家/地區總數和每個擷取的國家/地區名稱。
* **isMultiSelect()**&#x200B;傳回multiSelect欄位的值，以指出下拉式清單是否允許多重選取。



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## 後續步驟

[建置、部署和測試](./build.md)
