---
title: 瞭解AEM中的Java API最佳實務
description: AEM建立在豐富的開放原始碼軟體堆疊上，可公開許多Java API，以供開發期間使用。 本文探討主要的API，以及使用它們的時機和原因。
version: 6.2, 6.3, 6.4, 6.5
sub-product: 基礎，資產，網站
feature: null
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
translation-type: tm+mt
source-git-commit: fcb47ee3878f6a789b2151e283431c4806e12564
workflow-type: tm+mt
source-wordcount: '2028'
ht-degree: 0%

---


# 瞭解Java API最佳實務

Adobe Experience Manager(AEM)建立在豐富的開放原始碼軟體堆疊上，可公開許多Java API供開發期間使用。 本文探討主要的API，以及使用它們的時機和原因。

AEM以4個主要Java API集為基礎。

* **Adobe Experience Manager(AEM)**

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

如果AEM提供API，則偏好它，而 [!DNL Sling]非JCR和OSGi。 如果AEM未提供API，則偏好 [!DNL Sling] JCR和OSGi。

此訂單是一般規則，表示存在例外。 中斷此規則的可接受理由為：

* 眾所周知的例外，如下所述。
* 較高層級的API不提供所需的功能。
* 在現有程式碼（自訂或AEM產品程式碼）的環境中運作，而其本身使用較不偏好的API，而移至新API的成本是不合理的。

   * 與建立混音相比，使用較低層級的API更好。

## AEM API

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API提供針對分類使用案例的抽象化和功能。

例如，AEM的 [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) 和 [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) API為AEM中代表網頁的 `cq:Page` 節點提供抽象化。

雖然這些節點可透過 [!DNL Sling] API（資源）和JCR API（節點）取得，但AEM的API提供一般使用案例的抽象化。 使用AEM API可確保AEM與產品之間的一致行為，以及AEM的自訂和擴充功能。

### com.adobe.*與com.day比較。* API

AEM API具有以下Java套件所識別的套件內部偏好設定，依偏好設定順序排列：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` 支援產品使用案例， `com.adobe.granite` 但支援跨產品平台使用案例，例如工作流程或工作(在產品間使用：AEM Assets、Sites等)。

`com.day.cq` 包含「原始」API。 這些API可解決Adobe收購前及／或收購前後所存在的核心抽象化和功能 [!DNL Day CQ]。 這些API受支援，除非或提供（較新） `com.adobe.cq` 的替 `com.adobe.granite` 代項目，否則不應避免。

新的抽象， [!DNL Content Fragments] 例如 [!DNL Experience Fragments] 和，是在空間中建立， `com.adobe.cq` 而不是在下面 `com.day.cq` 描述。

### 查詢API

AEM支援多種查詢語言。 3種主要語言是 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath和 [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)。

最重要的問題是，在程式碼庫中維持一致的查詢語言，以降低理解的複雜性和成本。

所有查詢語言都有效地具有相同的效能配置檔案，將它們轉存到JCR-SQL2中以執行最終查詢，與查詢時間本身相比，轉換到JCR-SQL2的時間可以忽略不計。 [!DNL Apache Oak]

偏好的API是 [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html)，它是最高層級的抽象，提供強穩的API，可建立、執行和擷取查詢結果，並提供下列功能：

* 簡單、參數化的查詢建構（以地圖為模型的查詢參數）
* 原生 [Java API和HTTP API](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTB查詢除錯程式](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOTB謂語](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) ，支援常見查詢需求

* 可擴充的API，可讓您開發自訂查詢謂 [語](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2和XPath可直接透過 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-)[和](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html)JCR API [[!DNL Sling] ，分別傳回結果](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) Resources [或](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)JCR Nodes Adobes

>[!CAUTION]
>
>AEM QueryBuilder API會洩漏ResourceResolver物件。 為避免此漏洞，請依照此程 [式碼範例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)。


## [!DNL Sling] API

* [**Apache[!DNL Sling]API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) 是支撐AEM的REST風格Web架構。 [!DNL Sling] 提供HTTP請求路由、將JCR節點建模為資源、提供安全上下文等。

[!DNL Sling] API為擴充而建立的額外優點，這表示與可擴充性較低的JCR API相比，使用API建立的應用程式增加行為 [!DNL Sling] 通常更簡單且安全。

### API的常見使 [!DNL Sling] 用

* 以 [[!DNL Sling Resources]的身分存取JCR節點](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) ，並透過 [ValueMaps存取其資料](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)。

* 通過ResourceResolver提供安全 [上下文](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)。
* 通過ResourceResolver的建立／移動／復 [制／刪除方法建立和刪除資源](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)。
* 透過 [ModifableValueMap更新屬性](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)。
* 建立請求處理建置區塊

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet篩選器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同步工作處理構成塊

   * [事件和作業處理常式](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [計畫](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling Models](https://sling.apache.org/documentation/bundles/models.html)

* [服務使用者](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

[JCR(Java Content Repository)2.0 API](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) ，是JCR實作的規格(在AEM中為 [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/))的一部分。 所有JCR實作都必須符合併實作這些API，因此是與AEM內容互動的最低層級API。

JCR本身是一個基於層次／樹的NoSQL資料儲存庫，AEM將其用作其內容儲存庫。 JCR有大量支援的API，從內容CRUD到查詢內容。 儘管有這種強穩的API，但較高階的AEM和抽象化版本更偏好的API還是很 [!DNL Sling] 少。

永遠比Apache Jackrabbit Oak API更喜歡JCR API。 JCR API用於與JCR存 ***儲庫互動*** ，而Oak API用於實 ***施*** JCR儲存庫。

### JCR API的常見誤解

雖然JCR是AEM的內容儲存庫，但其API並非與內容互動的偏好方法。 請改用AEM API（頁面、資產、標籤等） 或Sling Resource API，因為它們可提供更好的抽象化。

>[!CAUTION]
>
>在AEM應用程式中廣泛使用JCR API的「工作階段」和「節點」介面是程式碼味道。 請確 [!DNL Sling] 定不應改用API。

### JCR API的常見用途

* [存取控制管理](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [可授權管理（使用者／群組）](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR觀察（監聽JCR事件）
* 建立深層節點結構

   * 雖然Sling APIs支援建立資源，但JCR APIs在 [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) 和 [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 中有方便的方法，可加速建立深層結構。

## OSGi API

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declative Services 1.2元件註解JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declative Services 1.2 Metatype註解JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API和較高層級的API(AEM、 [!DNL Sling]和JCR)之間幾乎沒有重疊，而且使用OSGi API的需求很少，而且需要具備高階的AEM開發專業知識。

### OSGi與Apache Felix API

OSGi定義了所有OSGi容器必須實施和遵循的規範。 AEM的OSGi實作Apache Felix也提供數種其專屬的API。

* 偏好OSGi API(`org.osgi`)而非Apache Felix API(`org.apache.felix`)。

### OSGi API的常見用途

* 聲明OSGi服務和元件的OSGi注釋。

   * 首選 [OSGi Declariative Services(DS)1.2 Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Felix SCR Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) for allibing OSGi services and components

* OSGi API，用於動態地在程式碼 [中取消／註冊OSGi服務／元件](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)。

   * 當不需要條件式OSGi服務／元件管理時（通常情況下），偏好使用OSGi DS 1.2注釋。

## 規則的例外

以下是上述定義的規則的常見例外。

### AEM Asset API

* 比 [ 較喜 `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) 歡 [`com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html)。

   * 雖然 `com.day.cq` Assets API為AEM的資產管理使用案例提供更多免費工具。
   * Granite Assets API支援低階資產管理使用案例（版本、關係）。

### 查詢API

* AEM QueryBuilder不支援某些查詢功能，例如 [建議](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼字檢查和索引提示等其他較不常見的功能。 要使用這些函式進行查詢，首選JCR-SQL2。

### [!DNL Sling] Servlet註冊 {#sling-servlet-registration}

* [!DNL Sling] servlet註冊，偏好 [使用OSGi DS 1.2註解(含@SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) ) `@SlingServlet`

### [!DNL Sling] 篩選器註冊 {#sling-filter-registration}

* [!DNL Sling] 篩選器註冊，偏好 [使用OSGi DS 1.2註解(含@SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) )，而非 `@SlingFilter`

## 有用的程式碼片段

以下是有用的Java程式碼片段，可說明使用討論的API的常見使用案例最佳範例。 這些程式碼片段也說明如何從較不偏好的API移至較偏好的API。

### JCR與資源解析器的 [!DNL Sling] 會話

#### 自動關閉Sling ResourceResolver

自從AEM 6.2起， [!DNL Sling] ResourceResolver就會 `AutoClosable` 在 [try-with-resources陳述式中](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 。 使用此語法時，不需要明確 `resourceResolver .close()` 的呼叫。

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

如果不能使用上述自動關閉技術， `finally` 則必須手動關閉塊中的資源解析器。

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

### JCR到 [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR節點至 [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] 至AEM Asset

#### 建議的方法

`DamUtil.resolveToAsset(..)`視需要向樹狀結構 `dam:Asset` 上移動，解析Asset物件下的任何資源。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代方法

將資源調整為資產需要資源本身作為節 `dam:Asset` 點。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] 資源至AEM頁面

#### 建議的方法

`pageManager.getContainingPage(..)` 根據需要向樹狀結 `cq:Page` 構上移，解析Page物件下的任何資源。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代方法 {#alternative-approach-1}

將資源調整為頁面需要資源本身作為節 `cq:Page` 點。

```java
Page page = resource.adaptTo(Page.class);
```

### 閱讀AEM頁面屬性

使用Page物件的getter來取得已知的`getTitle()`屬 `getDescription()`性（、等） 並獲 `page.getProperties()` 取用於檢索其 `[cq:Page]/jcr:content` 他屬性的ValueMap。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 閱讀AEM Asset中繼資料屬性

資產API提供從節點讀取屬性的方便方 `[dam:Asset]/jcr:content/metadata` 法。 請注意，這不是ValueMap，不支援第2個參數（預設值和自動文字轉換）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 讀取屬 [!DNL Sling][!DNL Resource] 性 {#read-sling-resource-properties}

當屬性儲存在AEM API（頁面、資產）無法直接存取的位置（屬性或相對資源）時，可使用 [!DNL Sling] Resources和ValueMaps來取得資料。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在這種情況下，AEM物件可能必須轉換為AEM物件， [!DNL Sling] 以有 [!DNL Resource] 效率地找出所要的屬性或子資源。

#### AEM頁面至 [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset to [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 使用的ModifableValueMap [!DNL Sling]寫入屬性

使用 [!DNL Sling]的 [ModifableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) ，將屬性寫入節點。 這只能寫入直接節點（不支援相對屬性路徑）。

請注意，對的調 `.adaptTo(ModifiableValueMap.class)` 用要求對資源具有寫權限，否則將返回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 建立AEM頁面

在使用「頁面範本」時，請一律使用PageManager來建立頁面，這是在AEM中正確定義和初始化頁面的必要條件。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Create a [!DNL Sling] Resource

資源解析器支援建立資源的基本操作。 建立更高層級的抽象化（AEM頁面、資產、標籤等） 使用其各自經理提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 刪除資 [!DNL Sling] 源

資源解析器支援刪除資源。 建立更高層級的抽象化（AEM頁面、資產、標籤等） 使用其各自經理提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
