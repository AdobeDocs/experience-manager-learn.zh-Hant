---
title: 單元測試
description: 實現一個單位test，該單元驗證在「定制元件」教程中建立的Byline元件的Sling模型的行為。
sub-product: sites
version: 6.5, Cloud Service
type: Tutorial
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '3020'
ht-degree: 0%

---

# 設備測試 {#unit-testing}

本教程介紹了一個單位test的實現，該單元驗證在 [自定義元件](./custom-component.md) 教程。

## 必備條件 {#prerequisites}

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。

_如果系統上同時安裝了Java 8和Java 11，則VS代碼test運行程式在執行test時可能會選擇較低的Java運行時，從而導致test失敗。 如果發生這種情況，請卸載Java 8。_

### 入門項目

>[!NOTE]
>
> 如果成功完成了上一章，則可以重新使用項目，並跳過簽出啟動程式項目的步驟。

檢查本教程所基於的基線代碼：

1. 查看 `tutorial/unit-testing-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. 使用Maven技能將代碼AEM庫部署到本地實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使AEM用6.5或6.4，則追加 `classic` 配置檔案。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始終可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) 或通過切換到分支本地檢出代碼 `tutorial/unit-testing-start`。

## 目標

1. 瞭解設備測試的基本知識。
1. 瞭解常用於test代碼的框架和工AEM具。
1. 瞭解編寫單元test時AEM用於模擬或模擬資源的選項。

## 背景 {#unit-testing-background}

在本教程中，我們將探討如何編寫 [設備Test](https://en.wikipedia.org/wiki/Unit_testing) 我們的Byline元件 [吊具模型](https://sling.apache.org/documentation/bundles/models.html) (在 [建立自定義組AEM件](custom-component.md))。 單位test是用Java編寫的生成時test，用於驗證Java代碼的預期行為。 每個單位test通常較小，並根據預期結果驗證方法（或工作單位）的輸出。

我們將使用最AEM佳做法，並使用：

* [JUnit 5](https://junit.org/junit5/)
* [莫基托測試框架](https://site.mockito.org/)
* [wcm.ioTest框架](https://wcm.io/testing/) (在 [阿帕奇吊床](https://sling.apache.org/documentation/development/sling-mock.html))

## 設備測試和Adobe雲管理器 {#unit-testing-and-adobe-cloud-manager}

[Adobe雲管理器](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) 整合單元test執行和 [代碼覆蓋率報告](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) CI/CD管道，幫助鼓勵和推廣單元測試代碼的最佳AEM實踐。

雖然單元測試代碼是任何代碼庫的良好做法，但在使用Cloud Manager時，通過為Cloud Manager提供單元test來運行，利用其代碼質量測試和報告設施非常重要。

## 更新testMaven依賴項 {#inspect-the-test-maven-dependencies}

第一步是檢查Maven依賴項，以支援寫入和運行test。 需要四個依賴項：

1. JUnit5
1. 莫基托Test框架
1. 阿帕奇吊床
1. AEM吊床Test框架(by io.wcm)

的 **JUnit5**。 **莫基托** 和 **吊AEM床** test相關性在使用 [馬AEM文原型](project-setup.md)。

1. 要查看這些依賴關係，請在 **aem-guides-wknd/pom.xml**，導航至 `<dependencies>..</dependencies>` 並查看io.wcm下JUnit、Mockito、Apache Sling Mocks和AEMMockTest的依賴項 `<!-- Testing -->`。
1. 確保 `io.wcm.testing.aem-mock.junit5` 設定為 **4.1.0**:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > 原型 **35** 生成項目 `io.wcm.testing.aem-mock.junit5` 版本 **4.1.8**。 請降級到 **4.1.0** 以遵循本章的其餘部分。

1. 開啟 **aem-guides-wknd/core/pom.xml** 並查看相應的測試依賴關係是否可用。

   中的並行源資料夾 **核** 項目將包含設備test和任何支援test檔案。 此 **test** folder將test類與原始碼分離，但允許test使用與原始碼位於同一包中的方式。

## 建立JUnittest {#creating-the-junit-test}

設備test通常使用Java類將1到1映射。 在本章中，我們將為 **BylineImpl.java**，即支援Byline元件的Sling模型。

![設備testsrc資料夾](assets/unit-testing/core-src-test-folder.png)

*儲存設備test的位置。*

1. 為建立設備test `BylineImpl.java` 通過在 `src/test/java` 在鏡像要測試的Java類位置的Java包資料夾結構中。

   ![建立新的BylineImplTest.java檔案](assets/unit-testing/new-bylineimpltest.png)

   既然我們在測試

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   建立相應的單元testJava類，位於

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   的 `Test` 單元test檔案的尾碼， `BylineImplTest.java` 是一項公約，它允許我們

   1. 輕鬆將其標識為test檔案 _為_ `BylineImpl.java`
   1. 同時，區分test檔案 _從_ 正在測試的這門課， `BylineImpl.java`



## 查看BylineImplTest.java {#reviewing-bylineimpltest-java}

此時，JUnittest檔案是空Java類。

1. 使用以下代碼更新檔案：

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   
   public class BylineImplTest {
   
       @BeforeEach
       void setUp() throws Exception {
   
       }
   
       @Test 
       void testGetName() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testGetOccupations() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testIsEmpty() { 
           fail("Not yet implemented");
       }
   }
   ```

1. 第一種方法 `public void setUp() { .. }` 用JUnit的 `@BeforeEach`，它指示JUnittest運行程式在運行此類中的每個test方法之前執行此方法。 這為初始化所有test所需的公用測試狀態提供了一個方便的位置。

1. 後續方法是test方法，其名稱前置詞為 `test` 按公約，並標有 `@Test` 注釋。 請注意，預設情況下，我們的所有test都將失敗，因為我們尚未實施它們。

   首先，我們從我們測試的類上每個公共方法的單個test方法開始，這樣：

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 由 | testGetName() |
   | getSchrops() | 由 | testGetSchrops() |
   | isEmpty() | 由 | testIsEmpty() |

   這些方法可以根據需要進行擴展，我們將在本章的後面部分看到。

   運行此JUnittest類(也稱為JUnitTest實例)時，每個方法都標有 `@Test` 將作為test執行，該語句可以通過或失敗。

![生成的BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 按一下右鍵「JUnitTest」 `BylineImplTest.java` 檔案和點擊 **運行**。
如預期，所有test都會失敗，因為它們尚未得到執行。

   ![作為junittest](assets/unit-testing/run-junit-tests.png)

   *按一下右鍵BylineImplTests.java >運行*

## 查看BylineImpl.java {#reviewing-bylineimpl-java}

編寫單元test時，主要有兩種方法：

* [TDD或Test驅動開發](https://en.wikipedia.org/wiki/Test-driven_development)即在實施開始之前，逐步編寫單位test;編寫test，編寫實現，使test通過。
* 實施優先開發，包括先開發工作代碼，然後編寫驗證該代碼的test。

在本教程中，使用了後一種方法(因為我們已經建立了一個 **BylineImpl.java** )。 因此，既要回顧和瞭解其公開方法的行為，又要瞭解其實施細節。 這聽起來可能相反，因為良好test只應關心投入和產出，但在工作時AEM，需要理解各種執行考慮因素，以建立工作test。

TDD在需要一AEM定專業水準，最能被精通代碼開發和單位測試AEM的開AEM發商採用AEM.

## 設定AEMtest上下文  {#setting-up-aem-test-context}

為編寫的AEM大多數代碼都依賴於JCR、SlingAEM或API，而API又要求運行的上AEM下文正確執行。

由於設備test是在生成時執行的，因此在運行實例的上AEM下文之外，不存在此類上下文。 為了方便， [wcm.io的吊AEM床](https://wcm.io/testing/aem-mock/usage.html) 建立允許這些API _大部分_ 就好像他們在跑AEM。

1. 使用建立上AEM下文 **wcm.io** `AemContext` 在 **BylineImplTest.java** 將其添加為JUnit擴展 `@ExtendWith` 到 **BylineImplTest.java** 的子菜單。 該擴展將處理所有所需的初始化和清理任務。 建立類變數 `AemContext` 可用於所有test方法。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   這個變數， `ctx`，顯示提供AEM多個和Sling抽象AEM的模擬上下文：

   * BylineImpl Sling模型將註冊到此上下文中
   * 在此上下文中建立模擬JCR內容結構
   * 可在此上下文中註冊自定義OSGi服務
   * 提供各種常見所需的模擬對象和幫助程式，如SlingHttpServletRequest對象、各種模擬Sling和AEMOSGi服務，如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *請注意，並非這些對象的所有方法都已實現！*
   * 和 [更多](https://wcm.io/testing/aem-mock/usage.html)!

   的 **`ctx`** 對象將作為我們大多數模擬上下文的入口點。

1. 在 `setUp(..)` 方法，每個方法 `@Test` 方法，定義通用的模擬測試狀態：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 將要測試的Sling模型註冊到模AEM擬上下文中，以便在 `@Test` 的雙曲餘切值。
   * **`load().json`** 將資源結構載入到模擬上下文中，使代碼能夠與這些資源交互，就像它們是由真正的儲存庫提供的一樣。 檔案中的資源定義 **`BylineImplTest.json`** 裝入模擬JCR上下文 **/內容**。
   * **`BylineImplTest.json`** 尚不存在，因此我們建立它並定義test所需的JCR資源結構。

1. 表示模擬資源結構的JSON檔案儲存在 **核心/src/test/資源** 與JUnit Javatest檔案遵循相同的包路徑。

   在下面建立新JSON檔案 `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` 命名 **BylineImplTest.json** 內容：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON為Byline元件單元test定義模擬資源（JCR節點）。 此時，JSON具有表示Byline元件內容資源所需的最小屬性集， `jcr:primaryType` 和 `sling:resourceType`。

   使用單元test時的一般規則是建立滿足每個test所需的最小模擬內容、上下文和代碼集。 避免在寫test之前建立完整的模擬環境的誘惑，因為這往往會產生不需要的文物。

   現在隨著 **BylineImplTest.json**, `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` 執行，將模擬資源定義載入到路徑上的上下文 **/content。**

## 正在測試getName() {#testing-get-name}

既然我們有了基本的模擬上下文設定，讓我們為 **BylineImpl的getName()**。 此test必須確保 **getName()** 返回儲存在資源的「」中的正確創作名稱&#x200B;**名稱** 屬性。

1. 更新 **testGetName**()中的方法 **BylineImplTest.java** 如下：

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** 設定預期值。 我們將此設定為「**簡·多內**。
   * **`ctx.currentResource`** 設定模擬資源的上下文以評估代碼，因此將其設定為 **/內容/行** 也就是載入模擬的副行內容資源的位置。
   * **`Byline byline`** 通過將Byline Sling模型從模擬請求對象中修改來實例化它。
   * **`String actual`** 調用我們測試的方法， `getName()`，在Byline Sling Model對象上。
   * **`assertEquals`** 斷言預期值與byline Sling Model對象返回的值匹配。 如果這些值不相等，test將失敗。

1. 運行test...而且它失敗了 `NullPointerException`。

   請注意，此test不會失敗，因為我們從未定義 `name` 模擬JSON中的屬性，這將導致test失敗，但test執行尚未達到此點！ 此test因 `NullPointerException` 對象本身。

1. 在 `BylineImpl.java`。 `@PostConstruct init()` 引發異常，它阻止Sling Model實例化，並導致Sling Model對象為空。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   實際上，當ModelFactory OSGi服務通過 `AemContext` （通過Apache Sling Context），並未實現所有方法，包括 `getModelFromWrappedRequest(...)` 在BylineImpl&#39;s `init()` 的雙曲餘切值。 這將導致 [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)而從短期來看 `init()` 以及由此導致的 `ctx.request().adaptTo(Byline.class)` 是空對象。

   由於提供的吊床無法容納代碼，因此我們必須自己實現模擬上下文。為此，我們可以使用Mockito建立模擬ModelFactory對象，該對象在 `getModelFromWrappedRequest(...)` 被調用。

   因為為了甚至實例化Byline Sling模型，此模擬上下文必須就位，因此我們可以將其添加到 `@Before setUp()` 的雙曲餘切值。 我們還需要 `MockitoExtension.class` 到 `@ExtendWith` 注釋位於 **BylineImplTest** 類。

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 將TestCase類標籤為 [Mockito JUnit木星擴展](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) 允許使用@Mock注釋在類級別定義模型對象。
   * **`@Mock private Image`** 建立類型的模擬對象 `com.adobe.cq.wcm.core.components.models.Image`。 請注意，這是在類級別定義的，以便根據需要， `@Test` 方法可以根據需要改變其行為。
   * **`@Mock private ModelFactory`** 建立ModelFactory類型的模擬對象。 請注意，這是純Mockito的模擬，沒有在其上實施任何方法。 請注意，這是在類級別定義的，以便根據需要， `@Test`方法可以根據需要改變其行為。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 註冊模擬行為 `getModelFromWrappedRequest(..)` 調用到模型Factory對象。 在中定義的結果 `thenReturn (..)` 就是返回模擬影像對象。 請注意，僅在以下情況下才調用此行為：第1個參數等於 `ctx`&#39;s請求對象，第2個參數是任何資源對象，第3個參數必須是核心元件映像類。 我們接受任何資源，因為在整個test中，我們將設定 `ctx.currentResource(...)` 各種模擬資源 **BylineImplTest.json**。 請注意，我們 **從寬()** 嚴格，因為我們稍後將要覆蓋ModelFactory的此行為。
   * **`ctx.registerService(..)`。** 將模擬ModelFactory對象註冊到AemContext中，其服務級別最高。 由於BylineImpl中使用的ModelFactory，因此需要 `init()` 通過 `@OSGiService ModelFactory model` 的子菜單。 為了AemContext插入 **我們** mock對象，處理調用 `getModelFromWrappedRequest(..)`，必須將其註冊為該類型(ModelFactory)的最高級別服務。

1. 重新運行test，但再次失敗，但這次消息清楚其失敗的原因。

   ![test名失敗斷言](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()由於斷言而失敗*

   我們收到 **斷言錯誤** 這意味著test中的斷言條件失敗了，它告訴我們 **預期值為&quot;Jane Doe&quot;** 但 **實際值為空**。 這很合理，因為&quot;**名稱** 未在模擬中添加屬性 **/內容/行** 資源定義 **BylineImplTest.json**，因此，我們添加它：

1. 更新 **BylineImplTest.json** 定義 `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 重新運行test **`testGetName()`** 現在過關了！

   ![test名傳遞](assets/unit-testing/testgetname-pass.png)


## 正在測試getSchrops() {#testing-get-occupations}

很好！ 我們的第一個test已經過去了！ 我們繼續，test `getOccupations()`。 由於模擬上下文的初始化在 `@Before setUp()`方法，這將適用於所有 `@Test` 此Test中的方法，包括 `getOccupations()`。

請記住，此方法必須返回儲存在職業屬性中的按字母順序排序的職業（降序）清單。

1. 更新 **`testGetOccupations()`** 如下：

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** 定義預期結果。
   * **`ctx.currentResource`** 將當前資源設定為在/content/byline上根據mock資源定義評估上下文。 這確保 **BylineImpl.java** 在模擬資源的上下文中執行。
   * **`ctx.request().adaptTo(Byline.class)`** 通過將Byline Sling模型從模擬請求對象中修改來實例化它。
   * **`byline.getOccupations()`** 調用我們測試的方法， `getOccupations()`，在Byline Sling Model對象上。
   * **`assertEquals(expected, actual)`** 斷言預期清單與實際清單相同。

1. 記住，就像 **`getName()`** 上面， **BylineImplTest.json** 不定義職業，所以如果我們運行這個test就會失敗，因為 `byline.getOccupations()` 將返回一個空清單。

   更新 **BylineImplTest.json** 列出職業清單，並按非字母順序設定，以確保我們的test確認這些職業是按字母順序排序的 **`getOccupations()`**。

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. 跑test，我們又通過了！ 看來分類工作管用了！

   ![獲得職業通過](assets/unit-testing/testgetoccupations-pass.png)

   *testGetSchrops()通過*

## 測試isEmpty() {#testing-is-empty}

最後一種test **`isEmpty()`**。

測試 `isEmpty()` 很有趣，因為它需要測試各種條件。 審閱 **BylineImpl.java**`s `isEmpty()` 方法必須測試以下條件：

* 當名稱為空時返回true
* 當職業為空或為空時返回true
* 當影像為空或沒有源URL時返回True
* 當存在名稱、職業和影像（具有源URL）時返回false

為此，我們需要建立新的test方法，每個方法都測試特定的條件，以及 `BylineImplTest.json` 開這些test。

請注意，此檢查允許我們跳過測試的時間 `getName()`。 `getOccupations()` 和 `getImage()` 空，因為通過測試該狀態的預期行為 `isEmpty()`。

1. 第一個test將test沒有設定屬性的全新元件的條件。

   將新資源定義添加到 `BylineImplTest.json`，為其提供語義名稱&quot;**空**&quot;

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** 定義名為「empty」的新資源定義，該定義僅具有 `jcr:primaryType` 和 `sling:resourceType`。

   記住我們裝了 `BylineImplTest.json` 入 `ctx` 執行中的每個test方法之前 `@setUp`，因此，此新資源定義可立即在以下test中獲得： **/content/empty。**

1. 更新 `testIsEmpty()` 如下所示，將當前資源設定為新的「 」**空**&quot;模擬資源定義。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   運行test並確保通過。

1. 接下來，建立一組方法，以確保任何所需資料點（名稱、職業或影像）為空， `isEmpty()` 返回true。

   對於每個test，使用離散模擬資源定義，更新 **BylineImplTest.json** 與 **無名稱** 和 **無職業**。

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   建立以下test方法來test這些狀態中的每個。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** test空模擬資源定義，並斷言 `isEmpty()` 是真的。

   **`testIsEmpty_WithoutName()`** test反對有職業但沒有名字的模擬資源定義。

   **`testIsEmpty_WithoutOccupations()`** test反對有名無實的模擬資源定義。

   **`testIsEmpty_WithoutImage()`** test具有名稱和職業的模擬資源定義，但將模擬映像設定為NULL。 請注意，我們要覆蓋 `modelFactory.getModelFromWrappedRequest(..)`定義的行為 `setUp()` 以確保此調用返回的Image對象為null。 莫基托小作品的特徵是嚴格的，不需要重複的代碼。 因此我們用 **`lenient`** 要明確注意的設定我們正在覆蓋 `setUp()` 的雙曲餘切值。

   **`testIsEmpty_WithoutImageSrc()`** test具有名稱和職業的模擬資源定義，但將模擬映像設定為在 `getSrc()` 調用。

1. 最後，編寫test，確保 **isEmpty()** 正確配置元件時返回false。 對於這種情況，我們可以 **/內容/行** 表示完全配置的Byline元件。

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 現在，在BylineImplTest.java檔案中運行所有設備test，並查看JavaTest報告輸出。

![所有test通過](./assets/unit-testing/all-tests-pass.png)

## 作為生成的一部分運行設備test {#running-unit-tests-as-part-of-the-build}

執行設備test是作為主構建的一部分傳遞的要求。 這可確保在部署應用程式之前成功傳遞所有test。 執行Maven目標（如包或安裝）會自動調用並要求傳遞項目中的所有設備test。

```shell
$ mvn package
```

![mvn包成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同樣，如果我們將test方法更改為失敗，則生成將失敗並報告哪些test失敗以及原因。

![mvn包失敗](assets/unit-testing/mvn-package-fail.png)

## 查看代碼 {#review-the-code}

查看完成的代碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git框上本地查看和部署代碼 `tutorial/unit-testing-solution`。
