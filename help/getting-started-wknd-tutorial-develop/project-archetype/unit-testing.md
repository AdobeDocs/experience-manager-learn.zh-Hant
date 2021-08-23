---
title: 單元測試
description: 本教學課程涵蓋實作單位測試，可驗證在自訂元件教學課程中建立之署名元件的Sling模型的行為。
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: API、AEM專案原型
topic: 內容管理、開發
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3013'
ht-degree: 0%

---


# 單元測試 {#unit-testing}

本教學課程涵蓋實作單位測試，以驗證在[自訂元件](./custom-component.md)教學課程中建立之Byline元件的Sling模型的行為。

## 必備條件 {#prerequisites}

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。

_如果系統上同時安裝了Java 8和Java 11，則VS代碼測試運行程式在執行測試時可能會選擇較低的Java運行時，從而導致測試失敗。如果發生此情況，請卸載Java 8._

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重新使用項目，並跳過簽出入門項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)的`tutorial/unit-testing-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. 使用您的Maven技能，將程式碼基底部署至本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start)上檢視完成的程式碼，或切換至分支`tutorial/unit-testing-start`在本機檢出程式碼。

## 目標

1. 了解單元測試的基本知識。
1. 了解常用於測試AEM程式碼的架構和工具。
1. 了解撰寫單元測試時模擬或模擬AEM資源的選項。

## 背景 {#unit-testing-background}

在本教學課程中，我們將探討如何為署名元件的[Sling Model](https://sling.apache.org/documentation/bundles/models.html)(在[建立自訂AEM元件](custom-component.md)中建立)撰寫[單元測試](https://en.wikipedia.org/wiki/Unit_testing)。 單元測試是以Java編寫的建置時間測試，用於驗證Java代碼的預期行為。 每個單元測試通常都較小，並根據預期結果來驗證方法（或工作單元）的輸出。

我們將使用AEM最佳實務，並使用：

* [JUnit 5](https://junit.org/junit5/)
* [莫基托測試框架](https://site.mockito.org/)
* [wcm.io測試架構](https://wcm.io/testing/) (以Apache Sling Cocks為 [基礎](https://sling.apache.org/documentation/development/sling-mock.html))

## 單元測試和AdobeCloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe雲](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=zh-Hant) 端管理器將單元測試執行和 [程式碼](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) 涵蓋範圍報表整合至其CI/CD管道，以協助鼓勵和推廣單元測試AEM程式碼的最佳實務。

雖然單位測試程式碼是任何程式碼基底的理想作法，但使用Cloud Manager時，請務必利用其程式碼品質測試和報告功能，為Cloud Manager執行單元測試。

## Inspect測試Maven相依性 {#inspect-the-test-maven-dependencies}

第一步是檢查Maven相依性，以支援編寫和執行測試。 需要四個相依性：

1. JUnit5
1. 莫基托測試框架
1. Apache Sling Mocks
1. AEM Mocks Test Framework（由io.wcm提供）

在使用[AEM Maven原型](project-setup.md)設定期間，會自動將&#x200B;**JUnit5**、**Mockito**&#x200B;和&#x200B;**AEM Mocks**&#x200B;測試相依性新增至專案。

1. 若要檢視這些相依性，請在&#x200B;**aem-guides-wknd/pom.xml**&#x200B;開啟父Reactor POM，導覽至`<dependencies>..</dependencies>`並確認已定義下列相依性：

   ```xml
   <dependencies>
       ...       
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.6.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>3.0.2</version>
           <scope>test</scope>
       </dependency>        
       ...
   </dependencies>
   ```

1. 開啟&#x200B;**aem-guides-wknd/core/pom.xml**&#x200B;並檢視對應的測試相依性可用：

   ```xml
   ...
   <!-- Testing -->
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <exclusions>
           <exclusion>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.models.impl</artifactId>
           </exclusion>
           <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
           </exclusion>
       </exclusions>
       <scope>test</scope>
   </dependency>
   <!-- Required to be able to support injection with @Self and @Via -->
   <dependency>
       <groupId>org.apache.sling</groupId>
       <artifactId>org.apache.sling.models.impl</artifactId>
       <version>1.4.4</version>
       <scope>test</scope>
   </dependency>
   ...
   ```

   **core**&#x200B;項目中的並行源資料夾將包含單元測試和任何支援的測試檔案。 此&#x200B;**test**&#x200B;資料夾提供了與原始碼分離的測試類，但允許測試的作用如同它們與原始碼位於相同的包中。

## 建立JUnit測試 {#creating-the-junit-test}

單元測試通常將1對1與Java類映射。 在本章中，我們將編寫&#x200B;**BylineImpl.java**&#x200B;的JUnit測試，該測試是支援Byline元件的Sling模型。

![單元測試src資料夾](assets/unit-testing/core-src-test-folder.png)

*儲存單元測試的位置。*

1. 通過在Java包資料夾結構中在`src/test/java`下建立新的Java類，為`BylineImpl.java`建立單元測試，該結構鏡像要測試的Java類的位置。

   ![建立新的BylineImplTest.java檔案](assets/unit-testing/new-bylineimpltest.png)

   因為我們在測試

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   在

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   單位測試檔案`BylineImplTest.java`上的`Test`尾碼是慣例，可讓我們

   1. 輕鬆將其識別為&#x200B;_`BylineImpl.java`的測試檔案_
   1. 但同時，將測試檔案&#x200B;_與要測試的類_&#x200B;區分為`BylineImpl.java`



## 檢閱BylineImplTest.java {#reviewing-bylineimpltest-java}

此時，JUnit測試檔案是空的Java類。 使用下列程式碼更新檔案：

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

1. 第一個方法`public void setUp() { .. }`用JUnit的`@BeforeEach`進行注釋，指示JUnit測試運行程式在運行此類中的每個測試方法之前執行此方法。 這為初始化所有測試所需的常見測試狀態提供了便利的位置。

2. 後續方法是測試方法，其名稱以約定的`test`為前置詞，並以`@Test`注釋標籤。 請注意，依預設，我們的所有測試都會設為失敗，因為我們尚未實作這些測試。

   首先，我們從測試類別上每個公用方法的單一測試方法開始，因此：

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | 由測試 | testGetName() |
   | getSchropies() | 由測試 | testGetSchropies() |
   | isEmpty() | 由測試 | testIsEmpty() |

   如本章稍後所述，我們可視需要擴展這些方法。

   運行此JUnit測試類（也稱為JUnit測試案例）時，標有`@Test`的每個方法都將作為可通過或失敗的測試執行。

![生成的BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. 按一下右鍵`BylineImplTest.java`檔案，然後點選&#x200B;**運行**，以運行「JUnit測試案例」。
如預期，所有測試都會失敗，因為這些測試尚未實作。

   ![以junit測試的形式運行](assets/unit-testing/run-junit-tests.png)

   *按一下右鍵BylineImplTests.java >運行*

## 檢閱BylineImpl.java {#reviewing-bylineimpl-java}

編寫單元測試時，有兩種主要方法：

* [TDD或測試驅動開發](https://en.wikipedia.org/wiki/Test-driven_development)，即在實現開發之前，逐步編寫單元測試；撰寫測試，撰寫實施以讓測試通過。
* 實作優先開發，包括先開發工作程式碼，然後撰寫驗證該程式碼的測試。

在本教學課程中，會使用後一種方法（因為我們已在上一章中建立有效的&#x200B;**BylineImpl.java**）。 因此，我們必須審視和了解其公用方法的行為，並了解其實施細節。 這聽起來可能相反，因為良好的測試只應關心輸入和輸出，但在AEM中工作時，需要理解各種實施考量，以便構建工作測試。

TDD在AEM領域需要一定的專業知識，最能被精通AEM程式碼的AEM開發和單元測試的AEM開發人員所採用。

## 設定AEM測試內容  {#setting-up-aem-test-context}

大部分為AEM編寫的程式碼都仰賴JCR、Sling或AEM API，而這反過來又需要執行中AEM的內容才能正確執行。

由於單位測試是在組建時執行，因此在執行中AEM例項的內容之外，就沒有這類內容。 為此，[wcm.io的AEM Mocks](https://wcm.io/testing/aem-mock/usage.html)會建立模擬內容，讓這些API對&#x200B;_大多_&#x200B;的運作如同在AEM中執行。

1. 在&#x200B;**BylineImplTest.java**&#x200B;中使用&#x200B;**wcm.io的** `AemContext`建立AEM內容，將其新增為以`@ExtendWith`裝飾的JUnit擴充功能至&#x200B;**BylineImplTest.java**&#x200B;檔案。 擴充功能會處理所有必要的初始化和清除工作。 為`AemContext`建立可用於所有測試方法的類變數。

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   此變數`ctx`會公開模擬AEM內容，提供一些AEM和Sling抽象化：

   * BylineImpl Sling模型將註冊到此內容中
   * 在此內容中建立模擬JCR內容結構
   * 可在此內容中註冊自訂OSGi服務
   * 提供多種常見的必要模擬物件和輔助工具，例如SlingHttpServletRequest物件、各種模擬Sling和AEM OSGi服務，例如ModelFactory、PageManager、Page、Template、ComponentManager、Component、TagManager、Tag等。
      * *請注意，並非所有這些物件的方法都已實作！*
   * 和[更多的](https://wcm.io/testing/aem-mock/usage.html)!

   **`ctx`**&#x200B;物件將作為我們大部分模擬內容的入口點。

1. 在每個`@Test`方法之前執行的`setUp(..)`方法中，定義通用的模擬測試狀態：

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** 將要測試的Sling模型註冊至模擬AEM內容，以便在方法中具現 `@Test` 化。
   * **`load().json`** 將資源結構載入模擬內容中，讓程式碼可像由實際存放庫提供一樣與這些資源互動。檔案&#x200B;**`BylineImplTest.json`**&#x200B;中的資源定義將載入到&#x200B;**/content**&#x200B;下的模擬JCR上下文中。
   * **`BylineImplTest.json`** 尚不存在，因此請建立它，並定義測試所需的JCR資源結構。

1. 代表模擬資源結構的JSON檔案儲存在&#x200B;**core/src/test/resources**&#x200B;下，其路徑與JUnit Java測試檔案的路徑相同。

   在&#x200B;**core/test/resources/com/adobe/aem/guides/wknd/core/models/impl**&#x200B;建立新的JSON檔案，名為&#x200B;**BylineImplTest.json**，其內容如下：

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   此JSON會定義署名元件單元測試的模擬資源（JCR節點）。 此時，JSON具有表示署名元件內容資源、 `jcr:primaryType`和`sling:resourceType`所需的最小屬性集。

   處理單元測試時，一般規則是建立滿足每個測試所需的最小模擬內容、內容和程式碼集。 撰寫測試前，請避免建立完整的模擬內容的誘惑，因為這通常會導致不需要的成品。

   現在，由於&#x200B;**BylineImplTest.json**&#x200B;存在，當執行`ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`時，模擬資源定義會載入至路徑&#x200B;**/content的內容中。**

## 測試getName() {#testing-get-name}

現在我們已有基本的模擬內容設定，接下來就為&#x200B;**BylineImpl&#39;s getName()**&#x200B;編寫第一個測試。 此測試必須確保方法&#x200B;**getName()**&#x200B;返回儲存在資源的&quot;**name&quot;**&#x200B;屬性中的正確創作名稱。

1. 按如下方式更新&#x200B;**BylineImplTest.java**&#x200B;中的&#x200B;**testGetName**()方法：

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
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

   * **`String expected`** 設定預期值。我們會將此值設為「**Jane Done**」。
   * **`ctx.currentResource`** 設定模擬資源的上下文以據以評估程式碼，因此這會設為 **/content/** bylineas，即載入模擬byline內容資源的位置。
   * **`Byline byline`** 從模擬要求物件中調整以實例化Byline Sling模型。
   * **`String actual`** 叫用我們正在測試的方 `getName()`法，位於Byline Sling Model物件上。
   * **`assertEquals`** 斷言預期值與署名Sling模型物件傳回的值相符。如果這些值不相等，則測試會失敗。

1. 運行測試……並且該測試失敗，出現`NullPointerException`。

   請注意，此測試「不會」失敗，因為我們從未在模擬JSON中定義`name`屬性，這會導致測試失敗，但測試執行尚未達到該點！ 此測試因署名物件本身上的`NullPointerException`而失敗。

1. 在`BylineImpl.java`中，如果`@PostConstruct init()`擲回例外狀況，會防止Sling模型具現化，並導致該Sling模型物件為Null。

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   事實證明，雖然ModelFactory OSGi服務是透過`AemContext`提供（透過Apache Sling Context），但並非所有方法都已實作，包括在BylineImpl的`init()`方法中呼叫的`getModelFromWrappedRequest(...)`。 這會導致[AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html)，這一術語導致`init()`失敗，因此`ctx.request().adaptTo(Byline.class)`的適配為空對象。

   由於提供的吊床無法容納代碼，因此我們必須自己實施模擬上下文。為此，我們可以使用Mockito建立模擬ModelFactory對象，當調用`getModelFromWrappedRequest(...)`時，該對象將返回模擬影像對象。

   因為若要實例化署名Sling模型，此模擬內容必須已就緒，我們可將其新增至`@Before setUp()`方法。 我們還需要將`MockitoExtension.class`添加到&#x200B;**BylineImplTest**&#x200B;類之上的`@ExtendWith`注釋中。

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** 標籤要使用Mockito JUnit Jupiter擴展 [運行的Test ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Case類，該擴展允許使用@Mock注釋來定義類級別上的模擬對象。
   * **`@Mock private Image`** 建立類型的模擬物 `com.adobe.cq.wcm.core.components.models.Image`件。請注意，這是在類別層級定義的，這樣`@Test`方法就可以根據需要更改其行為。
   * **`@Mock private ModelFactory`** 建立ModelFactory類型的模擬對象。請注意，這是純Mockito模擬，沒有實作方法。 請注意，這會在類別層級定義，以便讓`@Test`方法可視需要變更其行為。
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** 在模擬ModelFactory物件上 `getModelFromWrappedRequest(..)` 呼叫時，註冊模擬行為。在`thenReturn (..)`中定義的結果是傳回模擬影像物件。 請注意，只有在下列情況下才會叫用此行為：第1個參數等於`ctx`的要求物件，第2個參數是任何資源物件，第3個參數必須是核心元件影像類別。 我們接受任何資源，因為在整個測試中，我們會將`ctx.currentResource(...)`設為&#x200B;**BylineImplTest.json**&#x200B;中定義的各種模擬資源。 請注意，我們添加了&#x200B;**enlighte()**&#x200B;嚴格，因為我們以後將要覆蓋ModelFactory的此行為。
   * **`ctx.registerService(..)`。** 在AemContext中註冊模擬的ModelFactory物件，並排名最高。這是必需的，因為BylineImpl的`init()`中使用的ModelFactory是通過`@OSGiService ModelFactory model`欄位插入的。 為了讓AemContext插入&#x200B;**我們的**&#x200B;模擬物件（可處理對`getModelFromWrappedRequest(..)`的呼叫），我們必須將其註冊為該類型(ModelFactory)的最高排名服務。

1. 重新執行測試，但再次失敗，但這次的訊息清楚說明失敗的原因。

   ![測試名稱失敗聲明](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()因斷言而失敗*

   我們收到&#x200B;**AssertionError**，這表示測試中的斷言條件失敗，它告訴我們&#x200B;**預期值為&quot;Jane Doe&quot;**，但&#x200B;**實際值為null**。 這很合理，因為尚未新增&quot;**name&quot;**&#x200B;屬性來模擬&#x200B;**/content/byline**&#x200B;中的資源定義(**BylineImplTest.json**)，所以，我們來新增：

1. 更新&#x200B;**BylineImplTest.json**&#x200B;以定義`"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. 重新執行測試，現在&#x200B;**`testGetName()`**&#x200B;已通過！

   ![測試名稱通過](assets/unit-testing/testgetname-pass.png)


## 測試getSchropies() {#testing-get-occupations}

很好！ 我們的第一次測試通過了！ 讓我們繼續測試`getOccupations()`。 由於模擬上下文的初始化是在`@Before setUp()`方法中進行，因此此測試案例中的所有`@Test`方法都能使用，包括`getOccupations()`。

請記住，此方法必須返回職業屬性中儲存的職業（降序）清單。

1. 按如下方式更新&#x200B;**`testGetOccupations()`**:

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
   * **`ctx.currentResource`** 在/content/byline設定當前資源，以根據模擬資源定義評估上下文。這可確保在模擬資源的內容中執行&#x200B;**BylineImpl.java**。
   * **`ctx.request().adaptTo(Byline.class)`** 從模擬要求物件中調整以實例化Byline Sling模型。
   * **`byline.getOccupations()`** 叫用我們正在測試的方 `getOccupations()`法，位於Byline Sling Model物件上。
   * **`assertEquals(expected, actual)`** 斷言預期清單與實際清單相同。

1. 請記住，就像上述的&#x200B;**`getName()`**&#x200B;一樣，**BylineImplTest.json**&#x200B;不定義職業，因此如果執行職業，此測試會失敗，因為`byline.getOccupations()`會傳回空白清單。

   更新&#x200B;**BylineImplTest.json**&#x200B;以包含職業清單，這些職業將按非字母順序設定，以確保我們的測試驗證職業是按&#x200B;**`getOccupations()`**&#x200B;的字母順序排序的。

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

1. 執行測試，我們再次通過！ 看來分類的職業有用！

   ![獲得職業通過](assets/unit-testing/testgetoccupations-pass.png)

   *testGetSchropies()通過*

## 測試isEmpty() {#testing-is-empty}

測試&#x200B;**`isEmpty()`**&#x200B;的最後一個方法。

測試`isEmpty()`很有趣，因為它需要測試各種條件。 檢閱&#x200B;**BylineImpl.java**&#x200B;的`isEmpty()`方法，必須測試下列條件：

* 名稱為空時傳回true
* 職業為null或為空時返回true
* 影像為null或沒有src URL時傳回true
* 名稱、職業和影像（含src URL）存在時，傳回false

為此，我們需要建立新的測試方法，每個測試都要測試特定條件，以及`BylineImplTest.json`中的新模擬資源結構，以推動這些測試。

請注意，此檢查可讓我們略過`getName()`、`getOccupations()`和`getImage()`為空白時的測試，因為該狀態的預期行為是透過`isEmpty()`測試。

1. 第一個測試將測試未設定屬性的全新元件的條件。

   將新資源定義添加到`BylineImplTest.json`，並為其提供語義名稱&quot;**empty**&quot;

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

   **`"empty": {...}`** 定義名為「empty」的新資源定義，該定義只包含 `jcr:primaryType` 和 `sling:resourceType`。

   請記住，在`@setUp`中執行每個測試方法之前，我們將`BylineImplTest.json`載入`ctx`中，因此在&#x200B;**/content/empty的測試中，我們可立即使用此新資源定義。**

1. 請依照以下方式更新`testIsEmpty()`，將目前資源設定為新的「**empty**」模擬資源定義。

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   執行測試並確保通過。

1. 接下來，建立一組方法，以確保如果任何必要的資料點（名稱、職業或影像）為空，`isEmpty()`會傳回true。

   對於每項測試，都使用離散的模擬資源定義，使用&#x200B;**without-name**&#x200B;和&#x200B;**without-schriptions**&#x200B;的其他資源定義更新&#x200B;**BylineImplTest.json**。

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

   建立下列測試方法以測試這些狀態的每一個。

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

   **`testIsEmpty()`** 測試空的模擬資源定義，並斷言 `isEmpty()` 為true。

   **`testIsEmpty_WithoutName()`** 針對有職業但沒有名字的模擬資源定義進行測試。

   **`testIsEmpty_WithoutOccupations()`** 測試有名字但沒有職業的模擬資源定義。

   **`testIsEmpty_WithoutImage()`** 以名稱和職業來測試模擬資源定義，但將模擬影像設為null。請注意，我們想覆寫`setUp()`中定義的`modelFactory.getModelFromWrappedRequest(..)`行為，以確保此呼叫傳回的影像物件為null。 莫基托小作品的特徵是嚴格的，不要有重複的代碼。 因此，我們使用&#x200B;**`lenient`**&#x200B;設定來設定模擬，以明確地注意我們正在覆寫`setUp()`方法中的行為。

   **`testIsEmpty_WithoutImageSrc()`** 使用名稱和職業來測試模擬資源定義，但會設定模擬影像，在叫用時傳回 `getSrc()` 空白字串。

1. 最後，撰寫測試以確保在正確配置元件時&#x200B;**isEmpty()**&#x200B;傳回false。 對於此條件，我們可以重新使用&#x200B;**/content/byline**，它表示完全配置的Byline元件。

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. 現在，在BylineImplTest.java檔案中運行所有單元測試，並查看Java測試報告輸出。

![所有測試都通過](./assets/unit-testing/all-tests-pass.png)

## 在組建中執行單元測試 {#running-unit-tests-as-part-of-the-build}

執行單元測試時，必須在maven組建中通過。 這可確保在部署應用程式之前成功通過所有測試。 執行Maven目標（如包或安裝）會自動調用，並要求通過項目中的所有單元測試。

```shell
$ mvn package
```

![mvn套件成功](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

同樣地，如果我們將測試方法變更為失敗，組建會失敗，並回報哪些測試失敗及原因。

![mvn套件失敗](assets/unit-testing/mvn-package-fail.png)

## 檢閱程式碼 {#review-the-code}

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git列`tutorial/unit-testing-solution`上檢閱並將程式碼部署於本機。
