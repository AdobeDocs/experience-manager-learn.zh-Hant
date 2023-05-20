---
title: '&Java貿易；API最佳實踐AEM'
description: AEM構建在豐富的開源軟體棧上，可揭示許多Java&Trade;在開發過程中使用的API。 本文探討了主要API的使用時間、使用原因。
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '2079'
ht-degree: 2%

---

# Java™ API最佳實踐

Adobe Experience Manager(AEM)構建在豐富的開源軟體堆棧上，該堆棧公開了許多Java™ API，供開發過程使用。 本文探討了主要API的使用時間、使用原因。

基AEM於四個主Java™ API集。

* **Adobe Experience ManagerAEM(**

   * 產品抽象，如頁面、資產、工作流等。

* **Apache Sling Web框架**

   * REST和基於資源的抽象，如資源、值映射和HTTP請求。

* **JCR(Apache Jackrabbit Oak)**

   * 資料和內容抽象，如節點、屬性和會話。

* **OSGi(Apache Felix)**

   * OSGi應用容器抽象，如服務和(OSGi)元件。

## Java™ API首選項「經驗法則」

一般規則是優先使用API/抽象，順序如下：

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

如果API是由提供AEM的，則首選 [!DNL Sling]、JCR和OSGi。 如果AEM不提供API，則首選 [!DNL Sling] 超過JCR和OSGi

此訂單是一般規則，表示存在異常。 可以接受的違反此規則的理由有：

* 眾所周知的例外，如下所述。
* 在更高級別的API中不提供所需的功能。
* 在現有代碼(自定義或產AEM品代碼)的上下文中操作，而現有代碼本身使用的API較少，而移動到新API的成本是不合理的。

   * 與建立混合相比，始終使用較低級別的API更好。

## APIAEM

* [**API AEM JavaDocs**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

APIAEM提供特定於產品化使用案例的抽象和功能。

比如說AEM, [頁面管理器](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) 和 [頁面](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) API提供抽象 `cq:Page` 中AEM的節點。

當這些節點通過 [!DNL Sling] API（作為資源）和JCR API（作為節點）AEM(API)為常用案例提供抽象。 使用AEMAPI可確保產品之間AEM的一致行為以及自定義和擴展AEM。

### com.adobe.&#42; 與com.day。&#42; API

AEMAPI具有由以下Java™包按優先順序標識的包內首選項：

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

的 `com.adobe.cq` 軟體包支援產品使用案例 `com.adobe.granite` 支援跨產品平台使用案例，如工作流或任務(在產品之間使用：AEM Assets、網站等)。

的 `com.day.cq` 包包含「原始」API。 這些API解決在Adobe獲取之前和/或之前存在的核心抽象和功能 [!DNL Day CQ]。 這些API受支援，應避免，除非 `com.adobe.cq` 或 `com.adobe.granite` 包不提供（較新）的備選方案。

新抽象，如 [!DNL Content Fragments] 和 [!DNL Experience Fragments] 是在 `com.adobe.cq` 空間 `com.day.cq` 如下所述。

### 查詢API

支AEM持多種查詢語言。 這三種主要語言 [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html)、XPath和 [查AEM詢生成器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)。

最重要的問題是在整個代碼庫中維護一致的查詢語言，以降低理解的複雜性和成本。

所有查詢語言都有與 [!DNL Apache Oak] 將它們轉儲到JCR-SQL2以執行最終查詢，與查詢時間本身相比，轉換到JCR-SQL2的時間可忽略不計。

首選API是 [查AEM詢生成器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)，這是最高級別的抽象，為構造、執行和檢索查詢結果提供了強健的API，並提供了以下功能：

* 簡單、參數化的查詢構造（以映射形式建模的查詢參數）
* 本機 [Java™ API和HTTP API](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* [查AEM詢調試器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [謂AEM語](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) 支援通用查詢要求

* 可擴展API，允許開發自定義 [查詢謂語](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html)
* JCR-SQL2和XPath可以直接通過 [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) 和 [JCR API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)，返回結果 [[!DNL Sling] 資源](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 或 [JCR節點](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)的下界。

>[!CAUTION]
>
>AEMQueryBuilder API會洩漏ResourceResolver對象。 要緩解此漏洞，請遵循此 [代碼示例](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164)。

## [!DNL Sling] API

* [**阿帕奇 [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[阿帕奇 [!DNL Sling]](https://sling.apache.org/) 是REST風格的Web框AEM架。 [!DNL Sling] 提供HTTP請求路由、將JCR節點建模為資源、提供安全上下文等。

[!DNL Sling] API具有為擴展而構建的額外優勢，這意味著增強使用構建的應用程式的行為通常更簡單、更安全 [!DNL Sling] API比擴展性較差的JCR API小。

### 常用 [!DNL Sling] API

* 將JCR節點訪問為 [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) 並通過 [值映射](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)。

* 通過 [資源解析器](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)。
* 通過ResourceResolver建立和刪除資源 [建立/移動/複製/刪除方法](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)。
* 通過 [可修改值映射](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)。
* 生成請求處理構建塊

   * [Servlet](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet篩選器](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* 非同步工作處理構造塊

   * [事件和作業處理程式](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [調度程式](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)

* [服務用戶](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## JCR API

* **[JCR 2.0 JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

的 [JCR（Java™內容儲存庫）2.0 API](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) 是JCR實現規範的一部分(在以下情況下AEM, [阿帕奇長兔橡樹](https://jackrabbit.apache.org/oak/docs/))。 所有JCR實現都必須符合併實現這些API，因此是與內容交互的最低級APIAEM。

JCR本身是基於分層/樹的NoSQL資料儲存AEM庫，用作其內容儲存庫。 JCR有大量受支援的API，從內容CRUD到查詢內容。 儘管有這種強大的API，但很少有人比較高級別AEM和 [!DNL Sling] 抽象。

總是比Apache Jackrabbit Oak API更喜歡JCR API。 JCR API用於 ***交互*** 具有JCR儲存庫，而Oak API用於 ***實施*** JCR儲存庫。

### 關於JCR API的常見誤解

雖然JCR是內AEM容儲存庫，但其API不是與內容交互的首選方法。 相反，更AEM喜歡API（頁、資產、標籤等）或Sling資源API，因為它們提供了更好的抽象。

>[!CAUTION]
>
>JCR APIs的Session和Node介面在應用程式中AEM的廣泛應用是代碼氣味。 確保 [!DNL Sling] 應改用API。

### JCR API的常用用途

* [訪問控制管理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [可授權管理（用戶/組）](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR觀測（監聽JCR事件）
* 建立深節點結構

   * 雖然Sling API支援建立資源，但JCR API在 [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) 和 [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) 加速形成深層結構。

## OSGi API

* [**OSGi R6 JavaDocs**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi聲明性服務1.2元件注釋JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi聲明性服務1.2元類型注釋JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi框架JavaDocs**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

OSGi API與較高級API之間幾乎沒有重疊(AEM, [!DNL Sling]而使用OSGi API的需求很少，需要高級的開AEM發專業知識。

### OSGi與Apache Felix API

OSGi定義了所有OSGi容器必須實施並符合的規範。 AEMOSGi實現Apache Felix也提供了幾個自己的API。

* 首選OSGi API(`org.osgi`)，通過Apache Felix API(`org.apache.felix`)。

### OSGi API的常用用途

* 用於聲明OSGi服務和元件的OSGi注釋。

   * 首選 [OSGi聲明性服務(DS)1.2注釋](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) 超 [Felix SCR注釋](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) 聲明OSGi服務和元件

* 用於動態代碼內的OSGi API [註銷/註冊OSGi服務/元件](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)。

   * 當不需要條件OSGi服務/元件管理時（通常情況下），最好使用OSGi DS 1.2注釋。

## 規則的例外

以下是上述規則的常見例外。

### OSGi API

在處理低級OSGi抽象時，例如定義或讀取OSGi元件屬性， `org.osgi` 比較高級Sling抽象更為優選。 競爭的Sling抽象概念沒有標籤為 `@Deprecated` 建議 `org.osgi` 選項。

另請注意OSGi配置節點定義首選 `cfg.json` 在 `sling:OsgiConfig` 的子菜單。

### 資AEM產API

* 首選 [ `com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) 超 [ `com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html)。

   * 當 `com.day.cq` 資產API為資產管理使用案例提供更AEM多免費的工具。
   * Granite Assets API支援低級別資產管理使用案例（版本、關係）。

### 查詢API

* AEMQueryBuilder不支援某些查詢功能，如 [建議](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions)、拼寫檢查和索引提示等其他較不常見的函式。 要使用這些函式進行查詢，首選JCR-SQL2。

### [!DNL Sling] Servlet註冊 {#sling-servlet-registration}

* [!DNL Sling] servlet註冊，首選 [OSGi DS 1.2注釋，帶@SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) 超 `@SlingServlet`

### [!DNL Sling] 篩選器註冊 {#sling-filter-registration}

* [!DNL Sling] 過濾器註冊，首選 [OSGi DS 1.2注釋，帶@SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) 超 `@SlingFilter`

## 有用的代碼片段

以下是有用的Java™代碼片段，它們使用討論的API說明常見使用案例的最佳做法。 這些片段還說明了如何從較不首選的API移動到較首選的API。

### JCR會話至 [!DNL Sling] 資源解析器

#### 自動關閉Sling ResourceResolver

從AEM6.2開始， [!DNL Sling] 資源解析器為 `AutoClosable` 在 [試用資源](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) 的雙曲餘切值。 使用此語法， `resourceResolver .close()` 不需要。

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

必須在 `finally` 框，則不能使用上面所示的自動關閉技術。

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

### JCR節點到 [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] 至資AEM產

#### 建議的方法

的 `DamUtil.resolveToAsset(..)` 函式解析位於 `dam:Asset` 按需向樹上移動，即可到達Asset對象。

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### 替代辦法

使資源適應資產需要資源本身 `dam:Asset` 的下界。

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] 資源到頁AEM

#### 建議的方法

`pageManager.getContainingPage(..)` 解析位於 `cq:Page` 按需向樹上移動，即可到達Page對象。

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### 替代辦法 {#alternative-approach-1}

使資源適應頁面要求資源本身 `cq:Page` 的下界。

```java
Page page = resource.adaptTo(Page.class);
```

### 讀取AEM頁屬性

使用Page對象的getter獲取眾所周知的屬性(`getTitle()`。 `getDescription()`，等等) `page.getProperties()` 獲取 `[cq:Page]/jcr:content` 用於檢索其他屬性的ValueMap。

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### 讀取資AEM產元資料屬性

Asset API提供了從中讀取屬性的方法 `[dam:Asset]/jcr:content/metadata` 的下界。 這不是ValueMap，不支援第二個參數（預設值和自動類型轉換）。

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### 閱讀 [!DNL Sling] [!DNL Resource] 屬性 {#read-sling-resource-properties}

當屬性儲存在API（頁、資產）無法直AEM接訪問的位置（屬性或相對資源）中時， [!DNL Sling] 資源和值映射可用於獲取資料。

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

在這種情況下，AEM對象可能必須轉換為 [!DNL Sling] [!DNL Resource] 以有效地定位所需的屬性或子資源。

#### 頁AEM到 [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### 資AEM產至 [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### 使用 [!DNL Sling]的可修改值映射

使用 [!DNL Sling]`s [可修改值映射](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) 將屬性寫入節點。 這只能寫入即時節點（不支援相對屬性路徑）。

將呼叫記錄到 `.adaptTo(ModifiableValueMap.class)` 需要對資源具有寫權限，否則返回null。

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### 建立頁AEM面

始終使用PageManager在使用「頁面模板」時建立頁面，這是正確定義和初始化頁面所必需的AEM。

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### 建立 [!DNL Sling] 資源

ResourceResolver支援建立資源的基本操作。 在建立更高級別的抽象(頁AEM面、資產、標籤等)時，請使用其各自的經理提供的方法。

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### 刪除 [!DNL Sling] 資源

ResourceResolver支援刪除資源。 在建立更高級別的抽象(頁AEM面、資產、標籤等)時，請使用其各自的經理提供的方法。

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
