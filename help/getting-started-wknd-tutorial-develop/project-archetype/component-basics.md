---
title: AEM Sites快速入門 — 元件基本概念
description: 透過簡單的「HelloWorld」範例，了解Adobe Experience Manager(AEM)Sites元件的基礎技術。 探討HTL、Sling模型、用戶端程式庫和作者對話方塊的主題。
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心元件、開發人員工具
topic: 內容管理、開發
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 1%

---


# 元件基本知識 {#component-basics}

本章將透過簡單的`HelloWorld`範例，探討Adobe Experience Manager(AEM)Sites元件的基礎技術。 對現有元件進行微幅修改，涵蓋製作、HTL、Sling模型、用戶端程式庫等主題。

## 必備條件 {#prerequisites}

查看設定[本地開發環境](./overview.md#local-dev-environment)所需的工具和說明。

影片中使用的IDE為[Visual Studio Code](https://code.visualstudio.com/)和[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)外掛程式。

## 目標 {#objective}

1. 了解HTL範本和Sling模型在動態轉譯HTML方面的角色。
1. 了解如何使用對話方塊來協助編寫內容。
1. 了解用戶端程式庫的基本知識，以包含CSS和JavaScript以支援元件。

## 您將建置的 {#what-you-will-build}

在本章中，您將對非常簡單的`HelloWorld`元件執行幾項修改。 在更新`HelloWorld`元件的過程中，您將了解AEM元件開發的關鍵領域。

## 章節入門項目 {#starter-project}

本章以[AEM專案原型](https://github.com/adobe/aem-project-archetype)產生的一般專案為基礎。 觀看以下影片並檢閱[必要條件](#prerequisites)以開始使用！

>[!NOTE]
>
> 如果成功完成上一章，則可以重新使用項目，並跳過簽出入門項目的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

開啟新的命令行終端機，並執行以下操作。

1. 在空白目錄中，複製[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 您可以視需要繼續使用前一章[專案設定](./project-setup.md)中產生的專案。

1. 導覽至`aem-guides-wknd`資料夾。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用下列命令建立專案並部署至AEM的本機執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 按照設定[本地開發環境](overview.md#local-dev-environment)的說明將項目導入首選IDE。

## 元件編寫 {#component-authoring}

元件可視為網頁的小型模組化建置模組。 要重新使用元件，元件必須是可配置的。 這可透過製作對話方塊完成。 接下來，我們將製作簡單元件，並檢查對話方塊中的值如何保存在AEM中。

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

以下是上述影片中執行的高階步驟。

1. 在&#x200B;**WKND Site** `>` **US** `>` **en**&#x200B;下建立名為&#x200B;**元件基礎**&#x200B;的新頁面。
1. 將&#x200B;**Hello World元件**&#x200B;新增至新建立的頁面。
1. 開啟元件的對話方塊，然後輸入一些文字。 儲存變更以查看頁面上顯示的訊息。
1. 切換至開發人員模式，在CRXDE-Lite中檢視內容路徑，並檢查元件例項的屬性。
1. 使用CRXDE-Lite查看位於`/apps/wknd/components/content/helloworld`的`cq:dialog`和`helloworld.html`指令碼。

## HTL（HTML範本語言）和對話方塊 {#htl-dialogs}

HTML範本語言或&#x200B;**[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)**&#x200B;是AEM元件用來轉譯內容的輕量型伺服器端範本語言。

**** 對話方塊定義可供元件使用的設定。

接下來，我們會更新`HelloWorld` HTL指令碼，在文字訊息之前顯示其他問候語。

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

以下是上述影片中執行的高階步驟。

1. 切換到IDE並開啟項目到`ui.apps`模組。
1. 開啟`helloworld.html`檔案，並對HTML標籤進行更改。
1. 使用[VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)等IDE工具，將檔案更改與本地AEM實例同步。
1. 返回瀏覽器，並觀察元件呈現已變更。
1. 開啟`.content.xml`檔案，該檔案在以下位置定義`HelloWorld`元件的對話框：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話方塊以新增名為&#x200B;**Title**&#x200B;的其他文字欄位，其名稱為`./title`:

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
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
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

1. 重新開啟檔案`helloworld.html`，此檔案代表負責轉譯`HelloWorld`元件的主要HTL指令碼，位於：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新`helloworld.html`以將&#x200B;**Greeting**&#x200B;文字欄位的值轉譯為`H1`標籤的一部分：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 使用開發人員外掛程式或使用您的Maven技能，將變更部署至AEM的本機例項。

## Sling 模型 {#sling-models}

Sling模型是註解導向的Java &quot;POJO&#39;s&quot;（純舊Java物件），可方便將資料從JCR對應至Java變數，並在以AEM進行開發時提供許多其他細節。

接下來，我們會對`HelloWorldModel` Sling模型進行一些更新，以在將某些商業邏輯輸出至頁面之前，套用至JCR中儲存的值。

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. 開啟檔案`HelloWorldModel.java`，此檔案是與`HelloWorld`元件搭配使用的Sling模型。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 新增下列匯入陳述式：

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新`@Model`注釋以使用`DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 在`HelloWorldModel`類中添加以下行，以將元件的JCR屬性`title`和`text`的值映射到Java變數：

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. 將以下方法`getTitle()`添加到`HelloWorldModel`類，該類返回名為`title`的屬性的值。 此方法新增其他邏輯，以傳回「此處的預設值！」字串值 如果屬性`title`為null或空白：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 將以下方法`getText()`添加到`HelloWorldModel`類，該類返回名為`text`的屬性的值。 此方法會將字串轉換為所有大寫字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 從`core`模組建立並部署套件組合：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 若使用AEM 6.4/6.5，請使用`mvn clean install -PautoInstallBundle -Pclassic`

1. 更新位於`aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`的檔案`helloworld.html`，以使用`HelloWorld`模型的新建立方法：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld"
   data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. 使用Eclipse開發人員外掛程式或使用您的Maven技能，將變更部署至AEM的本機例項。

## 用戶端資源庫 {#client-side-libraries}

用戶端資料庫（簡稱clientlibs）提供一種機制，可組織及管理AEM Sites實作所需的CSS和JavaScript檔案。 用戶端程式庫是在AEM的頁面上包含CSS和JavaScript的標準方式。

接下來，我們將包含`HelloWorld`元件的一些CSS樣式，以了解用戶端程式庫的基本知識。

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

以下是上述影片中執行的高階步驟。

1. 在`/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs`下方建立名為`clientlib-helloworld`的新資料夾。
1. 建立資料夾和檔案結構，如下所示`clientlibs`下

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. 將以下內容填入`helloworld.css`:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
       color: red;
   }
   ```

1. 將以下內容填入`helloworld/clientlibs/css.txt`:

   ```plain
   #base=css
   helloworld.css
   ```

1. 將以下內容填入`helloworld/clientlibs/js/helloworld.js`:

   ```js
   console.log("Hello World from Javascript!");
   ```

1. 將以下內容填入`helloworld/clientlibs/js.txt`:

   ```plain
   #base=js
   helloworld.js
   ```

1. 更新`clientlib-helloworld/.content.xml`檔案以包含下列屬性：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. 將`clientlibs/clientlib-base/.content.xml`檔案更新為&#x200B;**embed** `wknd.helloworld`類別：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. 使用開發人員外掛程式或使用您的Maven技能，將變更部署至AEM的本機例項。

   >[!NOTE]
   >
   > 由於效能原因，瀏覽器經常快取CSS和JavaScript。 如果您沒有立即看到用戶端程式庫的變更，請執行硬式重新整理，並清除瀏覽器的快取。 使用無痕窗口可能有助於確保新快取。

## 恭喜！ {#congratulations}

恭喜，您剛剛在Adobe Experience Manager中了解元件開發的基本知識！

### 後續步驟 {#next-steps}

在下一章[頁面和範本](pages-templates.md)中熟悉Adobe Experience Manager頁面和範本。 了解如何將核心元件代理至專案，並了解可編輯範本的進階政策設定，以建立結構良好的文章頁面範本。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git分支`tutorial/component-basics-solution`上檢閱並將程式碼部署於本機。
