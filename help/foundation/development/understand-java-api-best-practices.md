---
title: '瞭解Java API的最佳實務AEM:'
description: 建AEM立在豐富的開放原始碼軟體堆疊上，可讓許多Java API在開發期間使用。 本文探討主要的API，以及使用它們的時機和原因。
version: 6.2, 6.3, 6.4, 6.5
sub-product: 基礎，資產，網站
feature: APIs
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 2%

---


# 瞭解Java API最佳實務

Adobe Experience Manager(AEM)以豐富的開放原始碼軟體堆疊為基礎，可公開許多Java API供開發期間使用。 本文探討主要的API，以及使用它們的時機和原因。

建AEM立在4個主要Java API集上。

* **Adobe Experience ManagerAEM()**

   * 產品抽象化，例如頁面、資產、工作流程等。

* **[!DNL Apache Sling]Web Framework**

   * REST和資源型抽象化，例如資源、值地圖和HTTP請求。

* **JCR([!DNL Apache Jackrabbit Oak])**

   * 資料和內容抽象化，例如節點、屬性和工作階段。

* **[!DNL OSGi (Apache Felix)]**

   * OSGi應用程式容器抽象化，例如服務和(OSGi)元件。

## Java API偏好設定「經驗法則」

一般規則是偏好API/抽象化順序：

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi**

如果API是由提供AEM，則偏好比[!DNL Sling]、JCR和OSGi更好。 如AEM果不提供API，則偏好[!DNL Sling]而非JCR和OSGi。

此訂單是一般規則，表示存在例外。 中斷此規則的可接受理由為：

* 眾所周知的例外，如下所述。
* 較高層級的API不提供所需的功能。
* 在現有程式碼(自訂或產AEM品程式碼)中運作，而其本身使用較不偏好的API，而移至新API的成本是不合理的。

   * 與建立混音相比，使用較低層級的API更好。

## AEMAPI

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

APIAEM提供針對分類使用案例的抽象化和功能。

例如，AEM[PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html)和[Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) API為代表網頁的`cq:Page`節點提AEM供抽象化。

雖然這些節點可透過[!DNL Sling] API（資源）和JCR API（節點）使用，但AEMAPI（資源）提供一般使用案例的抽象化。 使用AEMAPI可確保產品、自訂和擴充功AEM能之間的一致行為AEM。

### com.adobe.*與com.day比較。* API

AEMAPI具有由下列Java套件識別的套件內部偏好設定，依偏好設定順序排列：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 支援產品使用案例， `com.adobe.granite` 但支援跨產品平台使用案例，例如跨產品使用的工作流程或工作：AEM Assets、網站等)。

`com.day.cq` 包含「原始」API。這些API可解決在Adobe收購[!DNL Day CQ]之前和／或圍繞收購而存在的核心抽象和功能。 這些API受支援，不應避免，除非`com.adobe.cq`或`com.adobe.granite`提供（較新）的替代選項。

[!DNL Content Fragments]和[!DNL Experience Fragments]等新抽象是內建在`com.adobe.cq`空間中，而不是下面所述的`com.day.cq`。

### 查詢API

AEM支援多種查詢語言。 3種主要語言為[JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath和[AEM Query Builder](https://helpx.adobe.com/tw/experience-manager/6-5/sites/developing/using/querybuilder-api.html)。

最重要的問題是，在程式碼庫中維持一致的查詢語言，以降低理解的複雜性和成本。

所有查詢語言都有效地具有相同的效能配置檔案，因為[!DNL Apache Oak]將它們轉換到JCR-SQL2以進行最終查詢執行，而與查詢時間本身相比，轉換到JCR-SQL2的時間可忽略不計。

首選的API是[AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)，它是最高級抽象，提供強穩的API，用於構造、執行和檢索查詢結果，並提供以下功能：

* 簡單、參數化的查詢建構（以地圖為模型的查詢參數）
* 原生[Java API和HTTP API](https://helpx.adobe.com/tw/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTB查詢除錯程式](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOTB預測支](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) 持常見查詢要求

* 可擴充的API，允許開發自訂[查詢謂語](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2和XPath可直接通過[[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)和[JCR APIs](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)執行，分別返回結果a [[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)或[JCR Nodes](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)。

>[!CAUTION]
>
>AEMQueryBuilder API會洩漏ResourceResolver物件。 要緩解此漏洞，請遵循此[程式碼範例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)。


## [!DNL Sling] API

* [**Apache  [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apacheis  [!DNL Sling]](https://sling.apache.org/) REST風格的Web框架，它支AEM撐著。[!DNL Sling] 提供HTTP請求路由、將JCR節點建模為資源、提供安全上下文等。

[!DNL Sling] API為擴充而建立的額外優點，這表示與可擴充性較低的JCR API相比，使用 [!DNL Sling] API建立的應用程式增加行為通常更簡單且安全。

### [!DNL Sling] API的常用

* 以[[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html)的形式訪問JCR節點，並通過[ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)訪問其資料。

* 通過[ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)提供安全上下文。
* 通過資源解析器的[create/move/copy/delete方法](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)建立和刪除資源。
* 透過[ModifableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)更新屬性。
* 建立請求處理建置區塊

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet篩選器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同步工作處理構成塊

   * [事件和作業處理常式](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [計畫](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)

* [服務使用者](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR（Java內容儲存庫）2.0 API](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)是JCR實作的規格(例如AEM[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))的一部分。 所有JCR實作都必須符合併實作這些API，因此是與內容互動的最低AEM層級API。

JCR本身是基於層次／樹的NoSQL資料儲存AEM庫，用作其內容儲存庫。 JCR有大量支援的API，從內容CRUD到查詢內容。 儘管有這種強穩的API，但較之較高層級和[!DNL Sling]抽象AEM，它們仍很少被青睞。

永遠比Apache Jackrabbit Oak API更喜歡JCR API。 JCR API用於&#x200B;***與JCR儲存庫交互***，而Oak API用於&#x200B;***實施*** JCR儲存庫。

### JCR API的常見誤解

雖然JCR是內容AEM儲存庫，但其API並非與內容互動的偏好方法。 請改用AEMAPI（頁面、資產、標籤等） 或Sling Resource API，因為它們可提供更好的抽象化。

>[!CAUTION]
>
>在應用程式中廣泛使用JCR API的Session和Node介AEM面是程式碼味道。 請確定[!DNL Sling] API不應改用。

### JCR API的常見用途

* [存取控制管理](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [可授權管理（使用者／群組）](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR觀察（監聽JCR事件）
* 建立深層節點結構

   * 雖然Sling APIs支援建立資源，但JCR APIs在[JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html)和[JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html)中有方便的方法，可加速建立深層結構。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declative Services 1.2元件註解JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declative Services 1.2 Metatype註解JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API與較高層級的API(AEM、[!DNL Sling]和JCR)之間幾乎沒有重疊，而且使用OSGi API的需求很少，需要高階的開發專AEM業知識。

### OSGi與Apache Felix API

OSGi定義了所有OSGi容器必須實施和遵循的規範。 AEMOSGi實作Apache Felix也提供其數個API。

* 比起Apache Felix API(`org.apache.felix`)，偏好OSGi API(`org.osgi`)。

### OSGi API的常見用途

* 聲明OSGi服務和元件的OSGi注釋。

   * 比起[Felix SCR注釋](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html),[OSGi Declariative Services(DS)1.2 Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)更適合聲明OSGi服務和元件

* OSGi API，用於動態在程式碼[中取消／註冊OSGi服務／元件](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)。

   * 當不需要條件式OSGi服務／元件管理時（通常情況下），偏好使用OSGi DS 1.2注釋。

## 規則的例外

以下是上述定義的規則的常見例外。

### AEM資產API

* 偏好[ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html)，而非[ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)。

   * 而`com.day.cq`資產API則為資產管理使用案例提供更AEM多免費工具。
   * Granite Assets API支援低階資產管理使用案例（版本、關係）。

### 查詢API

* QueryBuilderAEM不支援某些查詢函式，例如[explications](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼字檢查和索引提示等其他較不常用的函式。 要使用這些函式進行查詢，首選JCR-SQL2。

### [!DNL Sling] Servlet註冊  {#sling-servlet-registration}

* [!DNL Sling] servlet註冊，偏好 [使用@](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover的OSGi DS 1.2註解  `@SlingServlet`

### [!DNL Sling] 篩選器註冊  {#sling-filter-registration}

* [!DNL Sling] 篩選器註冊，偏好 [使用@](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterover的OSGi DS 1.2註解  `@SlingFilter`

## 有用的程式碼片段

以下是有用的Java程式碼片段，可說明使用討論的API的常見使用案例最佳範例。 這些程式碼片段也說明如何從較不偏好的API移至較偏好的API。

### JCR與[!DNL Sling]資源解析器的會話

#### 自動關閉Sling ResourceResolver

從AEM6.2開始，[!DNL Sling]資源解析器在[try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)語句中是`AutoClosable`。 使用此語法時，不需要明確呼叫`resourceResolver .close()`。

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

#### 手動關閉的Sling ResourceResolver

如果不能使用上述自動關閉技術，則必須在`finally`塊中手動關閉資源解析器。

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

### JCR到[!DNL Sling] [!DNL Resource]的路徑

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR節點到[!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] 至資AEM產

#### 建議的方法

`DamUtil.resolveToAsset(..)`視需要向樹狀結 `dam:Asset` 構上移，解析Asset物件下的任何資源。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代方法

將資源改編為資產需要資源本身為`dam:Asset`節點。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] 資源到頁AEM面

#### 建議的方法

`pageManager.getContainingPage(..)` 根據需要向樹狀結 `cq:Page` 構上移，解析Page物件下的任何資源。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代方法{#alternative-approach-1}

將資源適配到頁面需要資源本身作為`cq:Page`節點。

```java
Page page = resource.adaptTo(Page.class);
```

### 讀取頁AEM面屬性

使用Page對象的getter獲取眾所周知的屬性（`getTitle()`、`getDescription()`等） 和`page.getProperties()`以取得`[cq:Page]/jcr:content` ValueMap以擷取其他屬性。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 讀取AEM資產中繼資料屬性

資產API提供從`[dam:Asset]/jcr:content/metadata`節點讀取屬性的方便方法。 請注意，這不是ValueMap，不支援第2個參數（預設值和自動文字轉換）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 讀取[!DNL Sling] [!DNL Resource]屬性{#read-sling-resource-properties}

當屬性儲存在API（頁面、資產）無法直接存取的AEM位置（屬性或相對資源）時，[!DNL Sling]資源和值地圖可用來取得資料。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在這種情況下，AEM該對象可能必須轉換為[!DNL Sling] [!DNL Resource] ，以便高效地定位所需的屬性或子資源。

#### 頁AEM面至[!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM資產至[!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 使用[!DNL Sling]的ModifableValueMap寫入屬性

使用[!DNL Sling]的[ ModifableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)將屬性寫入節點。 這只能寫入直接節點（不支援相對屬性路徑）。

請注意，對`.adaptTo(ModifiableValueMap.class)`的調用要求對資源具有寫權限，否則它將返回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 建立頁AEM面

請一律使用PageManager建立頁面，因為需要使用「頁面範本」，才能正確定義和初始化頁面AEM。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 建立[!DNL Sling]資源

資源解析器支援建立資源的基本操作。 建立更高層級的抽象AEM化（頁面、資產、標籤等） 使用其各自經理提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 刪除[!DNL Sling]資源

資源解析器支援刪除資源。 建立較高層級的抽象AEM化（頁面、資產、標籤等） 使用其各自經理提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
