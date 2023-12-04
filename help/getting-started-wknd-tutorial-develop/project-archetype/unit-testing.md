---
title: 單元測試
description: 實作單元測試，以驗證在自訂元件教學課程中建立的Byline元件的Sling模型的行為。
version: 6.5, Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 955
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# 單元測試 {#unit-testing}

本教學課程涵蓋實作單元測試，以驗證Byline元件Sling模型(建立於 [自訂元件](./custom-component.md) 教學課程。

## 先決條件 {#prerequisites}

檢閱設定所需的工具和指示 [本機開發環境](overview.md#local-dev-environment).

_如果系統上同時安裝了Java™ 8和Java™ 11，VS Code測試執行程式可能會在執行測試時挑選較低的Java™執行階段，從而導致測試失敗。 如果發生這種狀況，請解除安裝Java™ 8。_

### 入門專案

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並略過出庫入門專案的步驟。

檢視教學課程建置的基底程式碼：

1. 檢視 `tutorial/unit-testing-start` 分支來源 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. 使用您的Maven技能將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請附加 `classic` 設定檔至任何Maven指令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以於檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) 或切換至分支以在本機簽出程式碼 `tutorial/unit-testing-start`.

## 目標

1. 瞭解單元測試的基本概念。
1. 瞭解測試AEM程式碼常用的架構和工具。
1. 瞭解在編寫單元測試時模擬或模擬AEM資源的選項。

## 背景 {#unit-testing-background}

在本教學課程中，我們將探索如何撰寫 [單元測試](https://en.wikipedia.org/wiki/Unit_testing) 針對署名元件的 [Sling模型](https://sling.apache.org/documentation/bundles/models.html) (建立於 [建立自訂AEM元件](custom-component.md))。 單元測試是以Java™撰寫的建置時間測試，可驗證Java™程式碼的預期行為。 每個單元測試通常都很小，並且會根據預期結果來驗證方法（或工作單位）的輸出。

我們採用AEM最佳實務，並採用：

* [JUnit 5](https://junit.org/junit5/)
* [Mockito測試架構](https://site.mockito.org/)
* [wcm.io測試架構](https://wcm.io/testing/) (建立在 [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## 單元測試和AdobeCloud Manager {#unit-testing-and-adobe-cloud-manager}

[AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html) 整合單元測試執行和 [程式碼涵蓋範圍報表](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html) 放入其CI/CD管道，以協助鼓勵並推廣單元測試AEM程式碼的最佳做法。

雖然單元測試計畫碼是任何計畫碼庫的良好做法，但在使用Cloud Manager時，透過提供單元測試讓Cloud Manager執行來利用其計畫碼品質測試和報告設施很重要。

## 更新測試Maven依賴項 {#inspect-the-test-maven-dependencies}

第一步是檢查Maven依賴項以支援寫入和執行測試。 需要四個相依性：

1. JUnit5
1. Mockito測試架構
1. Apache Sling Mocks
1. AEM Mocks Test Framework （由io.wcm）

此 **JUnit5**、 **Mockito和 **AEM Mocks** 測試相依性會在安裝期間使用自動新增到專案 [AEM Maven原型](project-setup.md).

1. 若要檢視這些相依性，請開啟Parent Reactor POM，位於 **aem-guides-wknd/pom.xml**，導覽至 `<dependencies>..</dependencies>` 並檢視下io.wcm的JUnit、Mockito、Apache Sling Mocks和AEM Mock Tests相依性 `<!-- Testing -->`.
1. 確定 `io.wcm.testing.aem-mock.junit5` 設為 **4.1.0**：

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
   > 原型 **35** 產生專案，使用 `io.wcm.testing.aem-mock.junit5` 版本 **4.1.8**. 請降級為 **4.1.0** 以依照本章的其餘部分進行。

1. 開啟 **aem-guides-wknd/core/pom.xml** 並檢視對應的測試相依性是否可用。

   中的平行來源資料夾 **核心** 專案將包含單元測試和任何支援的測試檔案。 這個 **測試** 資料夾提供將測試類別與原始程式碼分隔的功能，但允許測試就像它們位在與原始程式碼相同的套件中一樣。

## 建立JUnit測試 {#creating-the-junit-test}

單元測試通常使用Java™類別對應1對1。 在本章中，我們將為 **BylineImpl.java**，此元件為支援Byline元件的Sling模型。

![單元測試src資料夾](assets/unit-testing/core-src-test-folder.png)

*儲存Unit測試的位置。*

1. 為以下專案建立單元測試 `BylineImpl.java` 藉由建立新的Java™類別於 `src/test/java` (在Java™套件資料夾結構中，該結構會映象要測試的Java™類別的位置)。

   ![建立新的BylineImplTest.java檔案](assets/unit-testing/new-bylineimpltest.png)

   由於我們正在測試

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   在下列位置建立對應的單元測試Java™類別

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   此 `Test` 單元測試檔案的後置字元， `BylineImplTest.java` 是慣例，可讓我們

   1. 輕鬆識別為測試檔案 _的_ `BylineImpl.java`
   1. 但也需區分測試檔案 _從_ 被測試的類別， `BylineImpl.java`

## 檢閱BylineImplTest.java {#reviewing-bylineimpltest-java}

此時，JUnit測試檔案是空的Java™類別。

1. 使用以下程式碼更新檔案：

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

1. 第一個方法 `public void setUp() { .. }` 已使用JUnit註釋 `@BeforeEach`，會指示JUnit測試執行程式先執行此方法，然後再執行此類別中的每個測試方法。 這為初始化所有測試所需的通用測試狀態提供了一個方便的位置。

1. 後續的方法是測試方法，其名稱會加上前置詞 `test` 依慣例，並標示 `@Test` 註解。 請注意，根據預設，我們所有的測試都會設為失敗，因為我們尚未實作這些測試。

   首先，我們先對我們測試的類別上的每個公用方法使用單一測試方法，因此：

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 測試者 | testGetName() |
   | getOccupations() | 測試者 | testGetOccupations() |
   | isEmpty() | 測試者 | testIsEmpty() |

   您可以視需要展開這些方法，我們將在本章的稍後章節中看到。

   執行此JUnit測試類別（也稱為JUnit測試案例）時，每個方法都標有 `@Test` 會以測試方式執行，測試結果可能通過或失敗。

![generated BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 以滑鼠右鍵按一下 `BylineImplTest.java` 檔案，並點選 **執行**.
如預期，所有測試都會失敗，因為它們尚未實作。

   ![執行為junit測試](assets/unit-testing/run-junit-tests.png)

   *在BylineImplTests.java >執行上按一下滑鼠右鍵*

## 檢閱BylineImpl.java {#reviewing-bylineimpl-java}

編寫單元測試時，有兩個主要方法：

* [TDD或測試驅動開發](https://en.wikipedia.org/wiki/Test-driven_development)，包括以增量方式編寫單元測試，緊接在開發實作之前；編寫測試，編寫實作以讓測試通過。
* 實作優先開發，包括先開發工作程式碼，然後撰寫測試以驗證該程式碼。

在本教學課程中，我們將使用後一種方法(因為我們已建立一個 **BylineImpl.java** （例如在上一章中）。 因此，我們必須檢閱並瞭解其公開方法的行為，但也要瞭解其部分實作細節。 這聽起來可能恰恰相反，因為良好的測試應該只關心輸入和輸出，但是當在AEM中工作時，為了建構有效的測試，需要理解各種實施考量。

在AEM的情境下，TDD需要一定程度的專業知識，且最能被精通AEM開發和AEM程式碼的單元測試的AEM開發人員採用。

## 設定AEM測試內容  {#setting-up-aem-test-context}

大部分針對AEM撰寫的程式碼都仰賴JCR、Sling或AEM API，而這又需要執行AEM的內容才能正確執行。

由於單元測試是在建置時執行，因此在正在執行的AEM執行個體的上下文之外，沒有這樣的上下文。 為了加速這項工作， [wcm.io的AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) 建立模擬內容，讓這些API可以 _大部分_ 就好像它們在AEM中執行一樣。

1. 建立AEM內容，使用 **wcm.io的** `AemContext` 在 **BylineImplTest.java** 將其新增為裝飾有的JUnit擴充功能 `@ExtendWith` 至 **BylineImplTest.java** 檔案。 擴充功能會處理所有必要的初始化和清理工作。 建立類別變數，用於 `AemContext` 可用於所有測試方法。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   此變數， `ctx`，會公開提供一些AEM和Sling抽象概念的模擬AEM上下文：

   * BylineImpl Sling模型已登入至此內容
   * 模擬JCR內容結構會在此內容中建立
   * 可在此內容中註冊自訂OSGi服務
   * 提供各種常見的必要模擬物件和協助程式，例如SlingHttpServletRequest物件、各種模擬Sling和AEM OSGi服務，例如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *並非這些物件的所有方法都已實作！*
   * 與 [更多內容](https://wcm.io/testing/aem-mock/usage.html)！

   此 **`ctx`** 物件將做為大部分模擬前後關聯的進入點。

1. 在 `setUp(..)` 方法，會在每一個 `@Test` 方法，定義常見的模擬測試狀態：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 將待測試的Sling模型註冊到模擬AEM Context中，以便在 `@Test` 方法。
   * **`load().json`** 將資源結構載入模擬內容中，讓程式碼與這些資源互動，就像它們是由真正的存放庫提供一樣。 檔案中的資源定義 **`BylineImplTest.json`** 載入到下的模擬JCR內容中 **/content**.
   * **`BylineImplTest.json`** 還沒有，存在，所以讓我們建立它並定義測試所需的JCR資源結構。

1. 代表模擬資源結構的JSON檔案儲存在 **core/src/test/resources** 會遵循與JUnit Java™測試檔案相同的套件路徑。

   在建立JSON檔案 `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` 已命名 **BylineImplTest.json** 包含下列內容：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON定義了Byline元件單元測試的模擬資源（JCR節點）。 此時，JSON具有代表Byline元件內容資源所需的最低屬性集， `jcr:primaryType` 和 `sling:resourceType`.

   使用單元測試時，一般規則是建立滿足每個測試所需的最小模擬內容、前後關聯和程式碼集合。 避免在撰寫測試之前建置完整模擬內容的誘惑，因為這通常會導致不需要的成品。

   現在有了 **BylineImplTest.json**，時間 `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` 執行時，模擬資源定義會載入到路徑的前後關聯中 **/content.**

## 測試getName() {#testing-get-name}

現在我們已有基本的模擬上下文設定，接下來讓我們編寫第一個測試 **BylineImpl的getName()**. 此測試必須確定方法 **getName()** 傳回正確編寫的名稱，並儲存在資源的&quot;**name」** 屬性。

1. 更新 **testGetName**()方法於 **BylineImplTest.java** 如下所示：

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

   * **`String expected`** 設定預期值。 我們會將此專案設為&quot;**Jane完成**「。
   * **`ctx.currentResource`** 設定模擬資源的前後關聯以評估程式碼，因此設定為 **/content/byline** 因為這是載入模擬署名內容資源的位置。
   * **`Byline byline`** 從模擬請求物件改寫並具現化署名Sling模型。
   * **`String actual`** 叫用我們正在測試的方法， `getName()`，在Byline Sling模型物件上。
   * **`assertEquals`** 斷言預期值與署名Sling模型物件傳回的值相符。 如果這些值不相等，測試就會失敗。

1. 執行測試……但失敗並出現 `NullPointerException`.

   此測試不會失敗，因為我們從未定義 `name` 屬性，即使測試尚未執行完畢，測試JSON模型中的屬性仍會失敗！ 此測試失敗的原因為 `NullPointerException` 署名物件本身。

1. 在 `BylineImpl.java`， if `@PostConstruct init()` 擲回一個例外狀況，阻止Sling模型具現化，並造成該Sling模型物件為空。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   原來，雖然ModelFactory OSGi服務是透過 `AemContext` （透過Apache Sling Context），並非所有方法都已實作，包括 `getModelFromWrappedRequest(...)` 這會在BylineImpl的 `init()` 方法。 這會導致 [抽象方法錯誤](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)，其詞語會導致 `init()` 會失敗，而產生的 `ctx.request().adaptTo(Byline.class)` 是Null物件。

   由於提供的模擬無法容納我們的程式碼，因此我們必須自行實作模擬前後關聯。為此，我們可以使用Mockito來建立模擬ModelFactory物件，這樣會傳回模擬Image物件，當 `getModelFromWrappedRequest(...)` 會在上面叫用。

   由於為了將署名Sling模型例項化，這個模擬上下文必須就位，我們可以將其新增到 `@Before setUp()` 方法。 我們還需要新增 `MockitoExtension.class` 至 `@ExtendWith` 註解在上 **BylineImplTest** 類別。

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 標籤要搭配執行的測試案例類別 [Mockito JUnit Jupiter延伸模組](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) 這允許使用@Mock註解來定義類別層級的模擬物件。
   * **`@Mock private Image`** 建立型別的模擬物件 `com.adobe.cq.wcm.core.components.models.Image`. 這是在類別層級定義，因此如有需要， `@Test` 方法可依需要變更其行為。
   * **`@Mock private ModelFactory`** 建立ModelFactory型別的模擬物件。 這是純粹的Mockito模擬，而且沒有實作任何方法。 這是在類別層級定義，因此如有需要， `@Test`方法可依需要變更其行為。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 註冊模擬行為的時機 `getModelFromWrappedRequest(..)` 會在模擬ModelFactory物件上呼叫。 中定義的結果 `thenReturn (..)` 是傳回模擬影像物件。 唯有在下列情況才會叫用此行為：第一個引數等於 `ctx`的請求物件，第二個引數是任何Resource物件，第三個引數必須是核心元件影像類別。 我們接受任何資源，因為在整個測試中，我們都會設定 `ctx.currentResource(...)` 至中定義的各種模擬資源 **BylineImplTest.json**. 請注意，我們新增 **lenient()** 嚴格性，因為我們稍後會想要覆寫ModelFactory的這個行為。
   * **`ctx.registerService(..)`。** 將模擬ModelFactory物件註冊到AemContext中，具有最高的服務排名。 這是必要的，因為BylineImpl的 `init()` 是透過 `@OSGiService ModelFactory model` 欄位。 供AemContext插入 **我們的** 模擬物件，可處理對的呼叫 `getModelFromWrappedRequest(..)`，我們必須將其註冊為該型別的最高等級(ModelFactory)服務。

1. 重新執行測試，再次失敗，但這次訊息已清楚說明失敗的原因。

   ![測試名稱失敗宣告](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()失敗，因為判斷提示*

   我們會收到 **AssertionError** 這表示測試中的判斷提示條件失敗，它告訴我們 **預期值為「Jane Doe」** 但是 **實際值為null**. 這是有道理的，因為「**name」** 尚未將屬性新增到模型 **/content/byline** 中的資源定義 **BylineImplTest.json**，所以讓我們將其新增：

1. 更新 **BylineImplTest.json** 以定義 `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 重新執行測試，並且 **`testGetName()`** 現在通過！

   ![測試名稱通過](assets/unit-testing/testgetname-pass.png)


## 測試getOccupations() {#testing-get-occupations}

很好！ 已通過第一個測試！ 讓我們繼續並測試 `getOccupations()`. 因為模擬內容的初始化是在 `@Before setUp()`方法，這適用於所有 `@Test` 此測試案例中的方法，包括 `getOccupations()`.

請記住，此方法必須傳回儲存在occupations屬性中的按字母順序排序的職業清單（降序）。

1. 更新 **`testGetOccupations()`** 如下所示：

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
   * **`ctx.currentResource`** 設定目前資源，以針對/content/byline處的模擬資源定義評估內容。 這可確保 **BylineImpl.java** 會在模擬資源的內容中執行。
   * **`ctx.request().adaptTo(Byline.class)`** 從模擬請求物件改寫並具現化署名Sling模型。
   * **`byline.getOccupations()`** 叫用我們正在測試的方法， `getOccupations()`，在Byline Sling模型物件上。
   * **`assertEquals(expected, actual)`** 主張預期清單與實際清單相同。

1. 記住，就像 **`getName()`** 上圖為 **BylineImplTest.json** 不會定義職業，因此如果執行，這項測試將會失敗，因為 `byline.getOccupations()` 將傳回空白清單。

   更新 **BylineImplTest.json** 以納入職業清單，並以非字母順序設定，以確保我們的測試驗證職業是否以字母順序排序 **`getOccupations()`**.

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

1. 執行測試，再次通過測試！ 取得已排序的職業似乎可以運作！

   ![取得職位通過](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations()通過*

## 測試isEmpty() {#testing-is-empty}

最後測試方法 **`isEmpty()`**.

測試 `isEmpty()` 很有趣，因為它需要針對各種條件進行測試。 檢閱 **BylineImpl.java**&#x200B;的 `isEmpty()` 方法必須測試下列條件：

* 當名稱為空時傳回true
* 當職業為空值或空白時傳回true
* 當影像為null或沒有src URL時傳回true
* 出現名稱、佔用和影像（具有src URL）時，傳回false

為此，我們需要建立測試方法，每個方法都會在中測試特定條件和新的模擬資源結構 `BylineImplTest.json` 以推動這些測試。

此檢查可讓我們略過以下時間的測試： `getName()`， `getOccupations()` 和 `getImage()` 空白，因為該狀態的預期行為是透過進行測試的 `isEmpty()`.

1. 第一個測試將會測試全新元件的狀態，該元件沒有設定屬性。

   將新的資源定義新增至 `BylineImplTest.json`，為其提供語意名稱」**空白**&quot;

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

   **`"empty": {...}`** 定義名稱為「empty」且僅具有 `jcr:primaryType` 和 `sling:resourceType`.

   記得要載入 `BylineImplTest.json` 到 `ctx` 執行中的每個測試方法之前 `@setUp`，因此我們可以在測試中立即使用這個新的資源定義 **/content/empty.**

1. 更新 `testIsEmpty()` 如下所述，將目前資源設定為新的&quot;**空白**&quot;模擬資源定義。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   執行測試並確保測試通過。

1. 接下來，建立一組方法，以確保如果任何必要的資料點（名稱、職業或影像）是空的， `isEmpty()` 傳回true。

   對於每項測試，都會使用分散式模型資源定義，請更新 **BylineImplTest.json** 「 」的其他資源定義 **without-name** 和 **不佔用職位**.

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

   建立下列測試方法來測試每種狀態。

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

   **`testIsEmpty()`** 針對空白模擬資源定義進行測試，並斷言 `isEmpty()` 為true。

   **`testIsEmpty_WithoutName()`** 針對具有職務但沒有名稱的模擬資源定義進行測試。

   **`testIsEmpty_WithoutOccupations()`** 針對名稱為但無佔用位置的模擬資源定義進行測試。

   **`testIsEmpty_WithoutImage()`** 針對具有名稱和佔用情況的模擬資源定義進行測試，但將模擬影像設定為傳回null。 請注意，我們想要覆寫 `modelFactory.getModelFromWrappedRequest(..)`行為定義於 `setUp()` 以確保此呼叫傳回的影像物件為Null。 Mockito stubs功能非常嚴格，並且不想要重複的程式碼。 因此，我們將模型設定為 **`lenient`** 設定以明確說明我們正在覆寫 `setUp()` 方法。

   **`testIsEmpty_WithoutImageSrc()`** 會針對具有名稱和佔用情況的模擬資源定義進行測試，但設定模擬影像以在下列情況下傳回空白字串： `getSrc()` 叫用的是。

1. 最後，撰寫測試以確保 **isEmpty()** 正確設定元件後，會傳回false。 針對此情況，我們可以重複使用 **/content/byline** 代表完整設定的Byline元件。

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 現在執行BylineImplTest.java檔案中的所有單元測試，並檢閱Java™測試報告輸出。

![所有測試均通過](./assets/unit-testing/all-tests-pass.png)

## 在建置過程中執行單元測試 {#running-unit-tests-as-part-of-the-build}

單元測試會執行，並需要作為Maven組建的一部分通過。 這可確保在部署應用程式之前成功通過所有測試。 執行Maven目標（例如封裝或安裝）會自動叫用，並需要傳遞專案中的所有單元測試。

```shell
$ mvn package
```

![mvn封裝成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同樣地，如果我們將測試方法變更為失敗，建置將失敗並報告哪些測試失敗及原因。

![mvn封裝失敗](assets/unit-testing/mvn-package-fail.png)

## 檢閱程式碼 {#review-the-code}

檢視完成的程式碼： [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上檢閱並部署程式碼至本機 `tutorial/unit-testing-solution`.
