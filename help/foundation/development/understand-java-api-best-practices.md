---
title: 了解AEM中的Java API最佳作法
description: AEM建置在豐富的開放原始碼軟體堆疊上，可公開許多Java API，以供開發期間使用。 本文探討主要API，以及其使用時機和原因。
version: 6.2, 6.3, 6.4, 6.5
sub-product: 基礎，資產，站點
feature: API
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: 開發
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2025'
ht-degree: 2%

---


# 了解Java API最佳作法

Adobe Experience Manager(AEM)建置在豐富的開放原始碼軟體堆疊上，可公開許多Java API，以便在開發期間使用。 本文探討主要API，以及其使用時機和原因。

AEM以4個主要Java API集為基礎。

* **Adobe Experience Manager(AEM)**

   * 產品抽象化，例如頁面、資產、工作流程等。

* **[!DNL Apache Sling]Web架構**

   * REST和資源型抽象化，例如資源、值映射和HTTP要求。

* **JCR([!DNL Apache Jackrabbit Oak])**

   * 資料和內容抽象化，例如節點、屬性和工作階段。

* **[!DNL OSGi (Apache Felix)]**

   * OSGi應用程式容器抽象化，例如服務和(OSGi)元件。

## Java API偏好設定「經驗法則」

一般規則是偏好API/抽象化，順序如下：

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

如果AEM提供API，請比[!DNL Sling]、JCR和OSGi更偏好。 如果AEM未提供API，則比JCR和OSGi偏好[!DNL Sling]。

此順序是一般規則，表示存在例外。 違反此規則的可接受原因如下：

* 眾所周知的例外，如下所述。
* 較高層級API不提供必要功能。
* 以現有程式碼(自訂或AEM產品程式碼)的情境運作，而程式本身使用較不偏好的API，且移至新API的成本無法正當。

   * 最好一致地使用較低層級的API，而非建立混合。

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API提供針對產品化使用案例的抽象化和功能。

例如，AEM [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html)和[Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) API為AEM中代表網頁的`cq:Page`節點提供抽象化功能。

雖然這些節點可透過[!DNL Sling] API以資源形式使用，而JCR API以節點形式使用，但AEM API提供常見使用案例的抽象化功能。 使用AEM API可確保產品之間的一致行為，以及AEM的自訂和擴充功能。

### com.adobe.*與com.day.* API

AEM API具有封裝內偏好設定，依偏好設定順序由下列Java封裝識別：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 支援產品使用案例， `com.adobe.granite` 但支援跨產品平台使用案例，例如工作流程或工作（用於不同產品）:AEM Assets、網站等)。

`com.day.cq` 包含「原始」API。這些API可解決Adobe贏取[!DNL Day CQ]之前及/或前後所存在的核心抽象化和功能。 除非`com.adobe.cq`或`com.adobe.granite`提供（較新的）替代方案，否則支援這些API，且不應加以避免。

[!DNL Content Fragments]和[!DNL Experience Fragments]等新抽象化建置在`com.adobe.cq`空間，而不是下面所述的`com.day.cq`。

### 查詢API

AEM支援多種查詢語言。 3種主要語言為[JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath和[AEM Query Builder](https://helpx.adobe.com/tw/experience-manager/6-5/sites/developing/using/querybuilder-api.html)。

最重要的顧慮是在程式碼庫中維持一致的查詢語言，以降低理解的複雜度和成本。

所有查詢語言都有效地具有相同的效能配置檔案，因為[!DNL Apache Oak]將它們轉入JCR-SQL2以進行最終查詢執行，而與查詢時間本身相比，轉換到JCR-SQL2的時間可以忽略不計。

慣用的API是[AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)，這是最高級別的抽象概念，提供強大的API來建構、執行和擷取查詢結果，並提供下列功能：

* 簡單、參數化的查詢構造（以映射建模的查詢參數）
* 原生[Java API和HTTP API](https://helpx.adobe.com/tw/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTB查詢偵錯工具](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOTB預測](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 值支援常見查詢需求

* 可擴充的API，允許開發自訂[查詢述詞](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2和XPath可以直接通過[[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)和[JCR APIs](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)執行，分別返回結果a [[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)或[JCR節點](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)。

>[!CAUTION]
>
>AEM QueryBuilder API會洩漏ResourceResolver物件。 若要緩解此洩漏，請依照以下程式碼範例[執行。](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)


## [!DNL Sling] API

* [**Apache  [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apache是支援AEM的RESTful Web架構。[!DNL Sling] 提供HTTP要求路由、將JCR節點模型為資源、提供安全性內容等。

[!DNL Sling] API具有為擴充功能建置的額外優點，這表示與延伸性較差的JCR API相比，使用API建置的應用程式 [!DNL Sling] 行為更簡單且安全。

### [!DNL Sling] API的常見用途

* 以[[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)存取JCR節點，並透過[ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)存取其資料。

* 透過[ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)提供安全性內容。
* 透過ResourceResolver的[create/move/copy/delete方法](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)建立和移除資源。
* 透過[ModiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)更新屬性。
* 建置請求處理建置區塊

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet篩選器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同步工作處理構建塊

   * [事件和作業處理程式](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [排程器](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)

* [服務使用者](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR(Java Content Repository)2.0 API](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)是JCR實作的規格(在AEM的案例中為[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))的一部分。 所有JCR實作都必須符合併實作這些API，因此是與AEM內容互動的最低層級API。

JCR本身是分層/樹型NoSQL資料儲存AEM用作其內容存放庫。 JCR提供大量支援的API，從內容CRUD到查詢內容。 儘管有這種健全的API，但相較於較高層級的AEM和[!DNL Sling]抽象化，仍很少有這類API更受青睞。

一律偏好使用JCR API，而非Apache Jackrabbit Oak API。 JCR API的用途為&#x200B;***與JCR存放庫互動***，而Oak API的用途為&#x200B;***實作*** JCR存放庫。

### 關於JCR API的常見誤解

雖然JCR是AEM內容存放庫，但其API並非與內容互動的慣用方法。 請改為使用AEM API（頁面、資產、標籤等） 或Sling資源API，因為它們提供更好的抽象化功能。

>[!CAUTION]
>
>AEM應用程式中廣泛使用JCR API的「工作階段」和「節點」介面是程式碼氣味。 請確定不應改用[!DNL Sling] API。

### JCR API的常見用途

* [存取控制管理](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [可授權管理（使用者/群組）](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR觀測（JCR事件的監聽）
* 建立深層節點結構

   * 雖然Sling API支援建立資源，但JCR API在[JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html)和[JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html)中有方便的方法，可加速建立深層結構。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declative Services 1.2元件註解JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declative Services 1.2元類型注釋JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API與較高層級的API(AEM、[!DNL Sling]和JCR)之間幾乎沒有重疊，使用OSGi API的需求很少，且需要高階的AEM開發專長。

### OSGi與Apache Felix API

OSGi定義了所有OSGi容器必須實施和遵循的規範。 AEM OSGi實作、Apache Felix也提供數個專屬的API。

* 偏好OSGi API(`org.osgi`)，而非Apache Felix API(`org.apache.felix`)。

### OSGi API的常見用途

* 用於聲明OSGi服務和元件的OSGi注釋。

   * 首選[OSGi聲明服務(DS)1.2注釋](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) ，而不是[Felix SCR注釋](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html)來聲明OSGi服務和元件

* 動態在程式碼[取消/註冊OSGi服務/元件](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)中使用的OSGi API。

   * 當不需要條件式OSGi服務/元件管理時（通常是這樣），偏好使用OSGi DS 1.2注釋。

## 規則的例外

以下是上述定義之規則的常見例外。

### AEM Asset API

* 偏好[ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html)，而非[ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)。

   * 而`com.day.cq` Assets API則為AEM資產管理使用案例提供更多免費工具。
   * Granite Assets API支援低階資產管理使用案例（版本、關係）。

### 查詢API

* AEM QueryBuilder不支援某些查詢函式，例如[sebclidations](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼字檢查和索引提示等較不常見的函式。 要使用這些函式進行查詢，建議使用JCR-SQL2。

### [!DNL Sling] Servlet註冊  {#sling-servlet-registration}

* [!DNL Sling] servlet註冊，首選 [OSGi DS 1.2注釋，帶@](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover  `@SlingServlet`

### [!DNL Sling] 篩選註冊  {#sling-filter-registration}

* [!DNL Sling] 篩選器註冊，偏 [好使用帶@SlingServletFilterover的OSGi DS 1.2](https://sling.apache.org/documentation/the-sling-engine/filters.html) 注釋  `@SlingFilter`

## 實用的程式碼片段

以下是實用的Java程式碼片段，說明使用已討論API常見使用案例的最佳實務。 這些片段也說明如何從較不慣用的API移至較偏好的API。

### [!DNL Sling] ResourceResolver的JCR工作階段

#### 自動關閉Sling ResourceResolver

自AEM 6.2起，[!DNL Sling] ResourceResolver在[try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)陳述式中為`AutoClosable`。 使用此語法時，不需要明確呼叫`resourceResolver .close()`。

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### 手動關閉Sling ResourceResolver

如果不能使用上述自動關閉技術，則`finally`塊中必須手動關閉ResourceResolver。

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### [!DNL Sling] [!DNL Resource]的JCR路徑

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR節點到[!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] 至AEM Asset

#### 建議的方法

`DamUtil.resolveToAsset(..)`視需要向上走 `dam:Asset` 樹狀結構，將資源解析至資產物件下。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代方法

將資源調整為資產需要資源本身為`dam:Asset`節點。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] 「資源到AEM」頁

#### 建議的方法

`pageManager.getContainingPage(..)` 視需要向上 `cq:Page` 走樹狀結構，解析位於Page物件下的任何資源。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代方法{#alternative-approach-1}

將資源調整為頁面需要資源本身為`cq:Page`節點。

```java
Page page = resource.adaptTo(Page.class);
```

### 讀取AEM頁面屬性

使用Page對象的getter獲取眾所周知的屬性（`getTitle()`、`getDescription()`等） 和`page.getProperties()`，以取得用於擷取其他屬性的`[cq:Page]/jcr:content` ValueMap。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 讀取AEM Asset中繼資料屬性

資產API提供從`[dam:Asset]/jcr:content/metadata`節點讀取屬性的便利方法。 請注意，這不是ValueMap，不支援第2個參數（預設值和自動類型轉換）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 讀取[!DNL Sling] [!DNL Resource]屬性{#read-sling-resource-properties}

當屬性儲存在AEM API（頁面、資產）無法直接存取的位置（屬性或相對資源）中時，可使用[!DNL Sling]資源和值圖來取得資料。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在這種情況下，AEM對象可能必須轉換為[!DNL Sling] [!DNL Resource]以有效地定位所需的屬性或子資源。

#### AEM頁面至[!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM資產至[!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 使用[!DNL Sling]的ModiableValueMap寫入屬性

使用[!DNL Sling]的[ModiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)將屬性寫入節點。 這只能寫入直屬節點（不支援相對屬性路徑）。

請注意，對`.adaptTo(ModifiableValueMap.class)`的呼叫需要資源的寫入權限，否則將傳回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 建立AEM頁面

一律要使用PageManager來建立頁面，就必須使用頁面範本，才能在AEM中正確定義和初始化頁面。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 建立[!DNL Sling]資源

ResourceResolver支援建立資源的基本操作。 建立較高層級的抽象化時(AEM頁面、資產、標籤等) 使用其各自經理提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 刪除[!DNL Sling]資源

ResourceResolver支援移除資源。 建立較高層級的抽象化時(AEM頁面、資產、標籤等) 使用其各自經理提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
