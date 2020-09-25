---
title: AEM Sites快速入門——元件基本資訊
description: 透過簡單的「HelloWorld」範例，瞭解Adobe Experience Manager(AEM)Sites元件的基礎技術。 HTL、Sling Models、用戶端程式庫和作者對話方塊的主題已經探索。
sub-product: sites
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 1%

---


# 元件基礎 {#component-basics}

在本章中，我們將透過簡單的範例來探索Adobe Experience Manager(AEM)Sites元件的基本技 `HelloWorld` 術。 對現有元件進行小幅修改，涵蓋編寫、HTL、Sling Models、用戶端程式庫等主題。

## 必備條件 {#prerequisites}

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。

## 目標 {#objective}

1. 瞭解HTL範本和Sling Models的角色，以動態呈現HTML。
1. 瞭解如何使用對話方塊來協助製作內容。
1. 瞭解用戶端程式庫的基本概念，以包含CSS和JavaScript來支援元件。

## 您將建立的 {#what-you-will-build}

在本章中，您將對非常簡單的元件執行幾項 `HelloWorld` 修改。 在更新元件的過程中，您 `HelloWorld` 將瞭解AEM元件開發的主要領域。

## 第一章創業項目 {#starter-project}

本章以 [AEM Project Archetype產生的一般專案為基礎](https://github.com/adobe/aem-project-archetype)。 觀看以下影片並檢視開始使 [用的](#prerequisites) 必要條件！

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

開啟新的命令列終端並執行下列動作。

1. 在空目錄中，複製 [aem-guides-wknd儲存庫](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > 或者，您可以直接下 [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) 載分支。

1. 導覽至目 `aem-guides-wknd` 錄：

   ```shell
   $ cd aem-guides-wknd
   ```

1. 切換到分 `component-basics/start` 支：

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. 使用下列命令，建立專案並部署至AEM的本機例項：

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. 按照說明將項目導入首選IDE以設定本地 [開發環境](overview.md#local-dev-environment)。

## 元件編寫 {#component-authoring}

元件可視為網頁的小型模組化建置區塊。 若要重新使用元件，元件必須是可設定的。 這是透過作者對話方塊完成的。 接下來，我們將製作簡單的元件，並檢查對話方塊中的值在AEM中的保存方式。

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 在 **WKND Site** **WKND Site** US `>` JounLight中，建立名為Component Basics的新頁 ****`>`****&#x200B;面。
1. 將「 **** Hello World」元件新增至新建立的頁面。
1. 開啟元件的對話框並輸入一些文本。 儲存變更，以查看頁面上顯示的訊息。
1. 切換至開發人員模式，並檢視CRXDE-Lite中的內容路徑，並檢查元件例項的屬性。
1. 使用CRXDE-Lite檢視位於 `cq:dialog` 的 `helloworld.html` 和指令碼 `/apps/wknd/components/content/helloworld`。

## HTML Template Language (HTL) {#htl-templates}

HTML範本語言或 [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) 是AEM元件用來轉換內容的輕量型伺服器端範本語言。

接下來，我們將更新 `HelloWorld` HTL指令碼，在文字訊息之前顯示其他問候語。

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 切換至Eclipse IDE，然後開啟專案至模 `ui.apps` 組。
1. 開啟定 `.content.xml` 義元件對話框的檔案， `HelloWorld` 位於：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話方塊，新增名為 **Greeting** 的其他文字欄位 `./greeting`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. 開啟檔案 `helloworld.html`，此檔案代表負責轉譯元件的主HTL指令碼，位 `HelloWorld` 於：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. 更 `helloworld.html` 新以呈現 **Greeting** textfield的值作為標籤的一 `H1` 部分：

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. 使用Eclipse Developer外掛程式或使用您的Maven技巧，將變更部署至AEM的本機例項。

## Sling Models {#sling-models}

Sling Models是註解導向的Java &quot;POJO&#39;s&quot;(Plain Old Java Objects)，可協助將資料從JCR對應至Java變數，並在AEM中進行開發時提供許多其他細節。

接下來，我們會對 `HelloWorldModel` Sling Model進行一些更新，以便在將其輸出至頁面之前，將一些商業邏輯套用至JCR中儲存的值。

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. 開啟檔 `HelloWorldModel.java`案，此為與元件搭配使用的Sling Model `HelloWorld` 。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 在類中添加以 `HelloWorldModel` 下行以映射元件的JCR屬性值 `greeting` 和 `text` Java變數：

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. 將下列方 `getGreeting()` 法新 `HelloWorldModel` 增至類別，傳回名為的屬性值 `greeting`。 如果屬性為null或空白，此方法會新增其他邏輯以傳回字串值&quot;Hello&quot; `greeting` :

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. 將下列方 `getTextUpperCase()` 法新 `HelloWorldModel` 增至類別，傳回名為的屬性值 `text`。 此方法會將字串轉換為所有upperCase字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 更新位 `helloworld.html` 置的 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 檔案，使用新建立的模型方 `HelloWorld` 法：

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. 使用Eclipse Developer外掛程式或使用您的Maven技巧，將變更部署至AEM的本機例項。

## 用戶端資源庫 {#client-side-libraries}

用戶端程式庫（簡稱clientlibs）提供一種機制來組織和管理AEM Sites實作所需的CSS和JavaScript檔案。 用戶端程式庫是在AEM中將CSS和JavaScript納入頁面的標準方式。

接下來，我們將包含一些元件的CSS `HelloWorld` 樣式，以瞭解用戶端程式庫的基本功能。

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 在下 `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` 面建立名為的 `clientlibs` 節點類型為的新節點 `cq:ClientLibraryFolder`。
1. 建立如下的資料夾和檔案結構 `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. 填 `helloworld/clientlibs/css/helloworld.css` 入下列：

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. 填 `helloworld/clientlibs/css.txt` 入下列：

   ```plain
   #base=css
   helloworld.css
   ```

1. 填 `helloworld/clientlibs/js/helloworld.js` 入下列：

   ```js
   console.log("hello world!");
   ```

1. 填 `helloworld/clientlibs/js.txt` 入下列：

   ```plain
   #base=js
   helloworld.js
   ```

1. 更新節 `clientlibs` 點屬性以包含下列兩個屬性：

   | 名稱 | 類型 | 值 |
   |------|------|-------|
   | 類別 | 字串 | wknd.base |
   | allowProxy | 布林值 (Boolean) | true |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. 使用Eclipse Developer外掛程式或使用您的Maven技巧，將變更部署至AEM的本機例項。

## 恭喜！ {#congratulations}

恭喜您，您剛在Adobe Experience Manager中學習了元件開發的基本知識！

### 後續步驟 {#next-steps}

在下一章的「頁面與範本」中，熟悉Adobe Experience Manager頁 [面與範本](pages-templates.md)。 瞭解核心元件如何在專案中代管，並瞭解可編輯範本的進階原則設定，以建立結構良好的文章頁面範本。

在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd) ，或在Git筆刷上在本機檢視並部署程式碼 `component-basics/solution`。

1. 克隆github.com/adobe/aem-wknd-guides [儲存庫](https://github.com/adobe/aem-guides-wknd) 。
1. 查看分 `component-basics/solution` 支
