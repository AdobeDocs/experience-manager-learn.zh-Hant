---
title: Web優化的影像傳輸Java&trade;API
description: 學習如何使用AEMas a Cloud Service的Web優化影像傳輸Java&trade;開發高效能Web體驗的API。
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 2%

---

# Web優化的映像傳遞Java™ API

瞭解如何AEM使用as a Cloud Service的Web優化映像交付Java™ API開發高效能Web體驗。

AEMas a Cloud Service支援 [Web優化的影像傳送](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html) 自動生成優化的資產影像Web呈現形式。 Web優化的映像交付可以使用三種主要方法：

1. [使用核AEM心WCM元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
2. 建立自定義元件 [擴展AEM核心WCM元件影像元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html#tackling-the-image-problem)
3. 建立使用AssetDelivery Java™ API生成Web優化映像URL的自定義元件。

本文探討在自定義元件中使用Web優化映像Java™ API的方法，該方法允許基於代碼在AEMas a Cloud Service和SDK上AEM運行。

## Java™ API

的 [資產交付API](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) 是一種OSGi服務，它為影像資產生成Web優化的傳遞URL。 `AssetDelivery.getDeliveryURL(...)` 允許的選項 [此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html#can-i-use-web-optimized-image-delivery-with-my-own-component%3F)。

的 `AssetDelivery` OSGi服務僅在as a Cloud Service中運行時AEM才滿足。 在AEMSDK上，對 `AssetDelivery` OSGi服務返回 `null`。 在as a Cloud Service上運行時，最好有條件地使用Web優化AEMURL，並在SDK上使用回退映像AEMURL。 通常，資產的Web格式副本就足夠了。


### OSGi服務中的API使用

標籤`AssetDelivery` 在自定義OSGi服務中作為可選引用，因此自定義OSGi服務在AEMSDK中仍然可用。

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### API在Sling模型中的應用

標籤`AssetDelivery` 參考（在自定義Sling模型中為可選），因此自定義Sling模型在AEMSDK中仍然可用。

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### API的條件使用

根據Web優化的影像URL或備用URL `AssetDelivery` OSGi服務的可用性。 條件使用允許代碼在SDK上運行代碼時AEM起作用。

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## 示例代碼

下面的代碼建立一個示例元件，該元件使用Web優化的影像URL顯示影像資產清單。

當代碼在as a Cloud Service上運AEM行時，在自定義元件中使用Web優化的Web影像格式副本。

![Web優化的圖AEM像](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEMas a Cloud Service支援AssetDelivery API，因此使用Web優化的Webp格式副本_

當代碼在SDK上運AEM行時，使用的靜態Web格式副本不太理想，允許元件在本地開發期間運行。

![SDK上Web優化的回退映AEM像](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEMSDK不支援AssetDelivery API，因此使用回退靜態Web格式副本(PNG或JPEG)_

實現分為三個邏輯部分：

1. 的 `WebOptimizedImage` OSGi服務充當提供的OSGi的「智慧代AEM理」 `AssetDelivery` OSGi服務，可以處理在AEMas a Cloud Service和AEMSDK中運行。
2. 的 `ExampleWebOptimizedImages` Sling模型提供收集影像資產清單及其Web優化URL以進行顯示的業務邏輯。
3. 的 `example-web-optimized-images` 組AEM件，實現HTL以顯示Web優化影像清單。

下面的示例代碼可以在代碼庫中複製，並根據需要進行更新。

### OSGi服務

的 `WebOptimizedImage` OSGi服務被分成可定址的公共介面(`WebOptimizedImage`)和內部實現(`WebOptimizedImageImpl`)。 的 `WebOptimizedImageImpl` 在as a Cloud Service上運行時返回Web優化AEM的映像URL，在AEMSDK上返回靜態Web格式副本URL，使元件在AEMSDK上保持正常工作。

#### 介面

該介面定義了OSGi服務合同，其它代碼（如Sling Models）可以與該合同交互。

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### 實施

OSGi服務實施包括對 `AssetDelivery` OSGi服務和回退邏輯，用於在 `AssetDelivery` 是 `null` SDK中AEM的。 可根據要求更新回退邏輯。

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### 吊具模型

的 `ExampleWebOptimizedImages` Sling Model被拆分為可定址的公共介面(`ExampleWebOptimizedImages`)和內部實現(`ExampleWebOptimizedImagesImpl`);

的 `ExampleWebOptimizedImagesImpl` Sling模型收集要顯示的影像資產清單並調用自定義 `WebOptimizedImage` OSGi服務，獲取Web優化的影像URL。 由於此Sling模型表示AEM一個元件，因此它有常用方法，如 `isEmpty()`。 `getId()`, `getData()` 但這些方法與使用網路優化影像並不直接相關。

#### 介面

該介面定義了其它代碼（如HTL）可以與之交互的Sling Model合同。

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### 實施

Sling模型使用自定義 `WebOptimizeImage` OSGi服務，用於收集其元件顯示的影像資產的Web優化影像URL。

在此示例中，使用簡單查詢來收集影像資產。

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### 組AEM件

組AEM件綁定到的Sling資源類型 `WebOptimizedImagesImpl` Sling模型實現，負責顯示影像清單。



元件接收 `Img` 對象 `getImages()` 包括在as a Cloud Service上運行時對web進行優化的WEBPAEM影像。 元件接收 `Img` 對象 `getImages()` 在SDK上運行時包括靜態PNG/JPEGWebAEM影像。

#### HTL

HTL使用 `WebOptimizedImages` Sling模型，並呈現清單  `Img` 對象返回 `getImages()`。

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```
