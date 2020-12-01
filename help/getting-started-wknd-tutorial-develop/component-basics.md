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


# 元件基本資訊{#component-basics}

在本章中，我們將透過簡單的`HelloWorld`範例來探索Adobe Experience Manager(AEM)Sites元件的基礎技術。 對現有元件進行小幅修改，涵蓋編寫、HTL、Sling Models、用戶端程式庫等主題。

## 必備條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

## 目標 {#objective}

1. 瞭解HTL範本和Sling Models的角色，以動態呈現HTML。
1. 瞭解如何使用對話方塊來協助製作內容。
1. 瞭解用戶端程式庫的基本概念，以包含CSS和JavaScript來支援元件。

## 您將建立的{#what-you-will-build}

在本章中，您將對非常簡單的`HelloWorld`元件執行幾項修改。 在更新`HelloWorld`元件的過程中，您將瞭解AEM元件開發的主要部份。

## 第15章起始項目{#starter-project}

本章以[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)產生的一般專案為基礎。 觀看以下影片並檢視[必要條件](#prerequisites)以開始使用！

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

開啟新的命令列終端並執行下列動作。

1. 在空目錄中，複製[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)資料庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > 或者，您可以直接下載[`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip)分支。

1. 導覽至`aem-guides-wknd`目錄：

   ```shell
   $ cd aem-guides-wknd
   ```

1. 切換到`component-basics/start`分支：

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

1. 按照說明將項目導入首選IDE，以設定[本地開發環境](overview.md#local-dev-environment)。

## 元件編寫{#component-authoring}

元件可視為網頁的小型模組化建置區塊。 若要重新使用元件，元件必須是可設定的。 這是透過作者對話方塊完成的。 接下來，我們將製作簡單的元件，並檢查對話方塊中的值在AEM中的保存方式。

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 在&#x200B;**WKND Site** `>`**US** `>` **en**&#x200B;下方，建立名為&#x200B;**Component Basics**&#x200B;的新頁面。
1. 將&#x200B;**Hello World元件**&#x200B;添加到新建立的頁面。
1. 開啟元件的對話框並輸入一些文本。 儲存變更，以查看頁面上顯示的訊息。
1. 切換至開發人員模式，並檢視CRXDE-Lite中的內容路徑，並檢查元件例項的屬性。
1. 使用CRXDE-Lite來檢視位於`/apps/wknd/components/content/helloworld`的`cq:dialog`和`helloworld.html`指令碼。

## HTML範本語言(HTL){#htl-templates}

HTML範本語言或[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)是AEM元件用來轉換內容的輕量級伺服器端範本語言。

接下來，我們將更新`HelloWorld` HTL指令碼，在文字訊息之前顯示其他問候語。

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 切換到Eclipse IDE並開啟項目至`ui.apps`模組。
1. 開啟`.content.xml`檔案，該檔案定義`HelloWorld`元件的對話框，位置為：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話方塊，以新增名為&#x200B;**Greeting**&#x200B;的其他文字欄位，其名稱為`./greeting`:

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

1. 開啟檔案`helloworld.html`，此檔案代表負責轉譯`HelloWorld`元件的主HTL指令碼，位址為：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. 更新`helloworld.html`，將&#x200B;**Greeting**&#x200B;文字欄位的值轉換為`H1`標籤的一部分：

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

接下來，我們會對`HelloWorldModel` Sling Model進行一些更新，以便在將其輸出至頁面之前，將一些商業邏輯套用至JCR中儲存的值。

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. 開啟檔案`HelloWorldModel.java`，此檔案是與`HelloWorld`元件一起使用的Sling Model。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 將以下行添加到`HelloWorldModel`類，將元件的JCR屬性`greeting`和`text`的值映射到Java變數：

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

1. 將以下方法`getGreeting()`添加到`HelloWorldModel`類中，該類返回名為`greeting`的屬性值。 如果屬性`greeting`為null或空白，此方法會新增其他邏輯以傳回字串值&quot;Hello&quot;:

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. 將以下方法`getTextUpperCase()`添加到`HelloWorldModel`類中，該類返回名為`text`的屬性值。 此方法會將字串轉換為所有upperCase字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 更新`aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`處的檔案`helloworld.html`，以使用`HelloWorld`模型的新建立方法：

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

接下來，我們將為`HelloWorld`元件加入一些CSS樣式，以瞭解用戶端程式庫的基本功能。

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 在`/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld`下面建立一個名為`clientlibs`的新節點，節點類型為`cq:ClientLibraryFolder`。
1. 建立`clientlibs`下方的資料夾和檔案結構，如下所示

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. 將`helloworld/clientlibs/css/helloworld.css`填入以下內容：

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. 將`helloworld/clientlibs/css.txt`填入以下內容：

   ```plain
   #base=css
   helloworld.css
   ```

1. 將`helloworld/clientlibs/js/helloworld.js`填入以下內容：

   ```js
   console.log("hello world!");
   ```

1. 將`helloworld/clientlibs/js.txt`填入以下內容：

   ```plain
   #base=js
   helloworld.js
   ```

1. 更新`clientlibs`節點屬性以包含下列兩個屬性：

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

## 恭喜！{#congratulations}

恭喜您，您剛在Adobe Experience Manager中學習了元件開發的基本知識！

### 後續步驟{#next-steps}

在下一章[頁面和範本](pages-templates.md)中熟悉Adobe Experience Manager頁面和範本。 瞭解核心元件如何在專案中代管，並瞭解可編輯範本的進階原則設定，以建立結構良好的文章頁面範本。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在`component-basics/solution`的Git位置上檢視並部署程式碼。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)儲存庫。
1. 查看`component-basics/solution`分支
