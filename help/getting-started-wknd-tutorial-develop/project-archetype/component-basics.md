---
title: 開始使用AEM Sites-元件基礎知識
description: 透過簡單的「HelloWorld」AEM範例，瞭解Adobe Experience Manager()網站元件的基礎技術。 HTL、Sling Models、用戶端程式庫和作者對話方塊的主題已經探索。
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
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 1%

---


# 元件基本資訊{#component-basics}

在本章中，我們將通過一個簡單的`HelloWorld`示例來探AEM索Adobe Experience Manager()Sites元件的基本技術。 對現有元件進行小幅修改，涵蓋編寫、HTL、Sling Models、用戶端程式庫等主題。

## 必備條件 {#prerequisites}

檢閱設定[本機開發環境](./overview.md#local-dev-environment)所需的工具和指示。

視訊中使用的IDE是[Visual Studio代碼](https://code.visualstudio.com/)和[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)外掛程式。

## 目標 {#objective}

1. 瞭解HTL範本和Sling Models的角色，以動態呈現HTML。
1. 瞭解如何使用對話方塊來協助製作內容。
1. 瞭解用戶端程式庫的基本概念，以包含CSS和JavaScript來支援元件。

## 您將建立的{#what-you-will-build}

在本章中，您將對非常簡單的`HelloWorld`元件執行幾項修改。 在更新`HelloWorld`元件的過程中，您將瞭解元件開發的主要AEM領域。

## 第15章起始項目{#starter-project}

本章以[Project Archetype](https://github.com/adobe/aem-project-archetype)生AEM成的通用項目為基礎。 觀看以下影片並檢視[必要條件](#prerequisites)以開始使用！

>[!NOTE]
>
> 如果您成功完成上一章，可以重新使用項目，並跳過簽出起始項目的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

開啟新的命令列終端並執行下列動作。

1. 在空目錄中，複製[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)資料庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 或者，您可以繼續使用上一章[Project Setup](./project-setup.md)中生成的項目。

1. 導覽至`aem-guides-wknd`資料夾。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用下列命令建立專案並將其部署AEM至本機執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM6.5或6.4，請將`classic`描述檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 按照說明將項目導入首選IDE，以設定[本地開發環境](overview.md#local-dev-environment)。

## 元件編寫{#component-authoring}

元件可視為網頁的小型模組化建置區塊。 若要重新使用元件，元件必須是可設定的。 這是透過作者對話方塊完成的。 接下來，我們將編寫一個簡單的元件，並檢查對話方塊中的值的保存方式AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 在&#x200B;**WKND Site** `>`**US** `>` **en**&#x200B;下方，建立名為&#x200B;**Component Basics**&#x200B;的新頁面。
1. 將&#x200B;**Hello World元件**&#x200B;添加到新建立的頁面。
1. 開啟元件的對話框並輸入一些文本。 儲存變更，以查看頁面上顯示的訊息。
1. 切換至開發人員模式，並檢視CRXDE-Lite中的內容路徑，並檢查元件例項的屬性。
1. 使用CRXDE-Lite來檢視位於`/apps/wknd/components/content/helloworld`的`cq:dialog`和`helloworld.html`指令碼。

## HTL（HTML範本語言）和對話方塊{#htl-dialogs}

HTML範本語言或&#x200B;**[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)**&#x200B;是元件用來轉換內容的輕量級伺服器端范AEM本語言。

**對** 話框定義可為元件建立的配置。

接下來，我們將更新`HelloWorld` HTL指令碼，在文字訊息之前顯示其他問候語。

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 切換到IDE並開啟項目至`ui.apps`模組。
1. 開啟`helloworld.html`檔案並變更HTML標籤。
1. 使用[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)等IDE工具，將檔案更改與本地實例同AEM步。
1. 返回瀏覽器並觀察元件演算已變更。
1. 開啟`.content.xml`檔案，該檔案定義`HelloWorld`元件的對話框，位置為：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話方塊，以新增名為&#x200B;**Title**&#x200B;的其他文字欄位，名稱為`./title`:

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

1. 重新開啟檔案`helloworld.html`，此檔案代表負責轉譯`HelloWorld`元件的主HTL指令碼，位址為：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新`helloworld.html`，將&#x200B;**Greeting**&#x200B;文字欄位的值轉換為`H1`標籤的一部分：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 將變更部署至使用開發人員外掛程式AEM或使用您Maven技能的本機例項。

## Sling 模型 {#sling-models}

Sling Models是註解導向的Java &quot;POJO&#39;s&quot;(Plain Old Java Objects)，可協助將資料從JCR對應至Java變數，並在開發時提供許多其他細節AEM。

接下來，我們會對`HelloWorldModel` Sling Model進行一些更新，以便在將其輸出至頁面之前，將一些商業邏輯套用至JCR中儲存的值。

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. 開啟檔案`HelloWorldModel.java`，此檔案是與`HelloWorld`元件一起使用的Sling Model。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 添加以下導入語句：

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新`@Model`註解以使用`DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 將以下行添加到`HelloWorldModel`類，將元件的JCR屬性`title`和`text`的值映射到Java變數：

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

1. 將以下方法`getTitle()`添加到`HelloWorldModel`類中，該類返回名為`title`的屬性值。 此方法新增其他邏輯，以傳回「此處是預設值！」的字串值 如果屬性`title`為null或空：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 將以下方法`getText()`添加到`HelloWorldModel`類中，該類返回名為`text`的屬性值。 此方法會將字串轉換為所有大寫字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 從`core`模組構建和部署包：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 如果使用AEM6.4/6.5，請使用`mvn clean install -PautoInstallBundle -Pclassic`

1. 更新`aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`處的檔案`helloworld.html`，以使用`HelloWorld`模型的新建立方法：

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

1. 將變更部署至使用Eclipse Developer外掛程式或AEM使用您的Maven技能的本機例項。

## 用戶端資源庫 {#client-side-libraries}

用戶端程式庫（簡稱clientlibs）提供一種機制來組織和管理AEM Sites實作所需的CSS和JavaScript檔案。 用戶端程式庫是在中的頁面上加入CSS和JavaScript的標準方AEM式。

接下來，我們將為`HelloWorld`元件加入一些CSS樣式，以瞭解用戶端程式庫的基本功能。

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

以下是上述視訊中執行的高階步驟。

1. 在`/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs`下方，建立名為`clientlib-helloworld`的新資料夾。
1. 建立`clientlibs`下方的資料夾和檔案結構，如下所示

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. 將`helloworld.css`填入以下內容：

   ```css
   .cmp-helloworld .cmp-helloworld__title {
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
   console.log("Hello World from Javascript!");
   ```

1. 將`helloworld/clientlibs/js.txt`填入以下內容：

   ```plain
   #base=js
   helloworld.js
   ```

1. 更新`clientlib-helloworld/.conten.xml`檔案以包含下列屬性：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. 將`clientlibs/clientlib-base/.content.xml`檔案更新為&#x200B;**embed**`wknd.helloworld`類別：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. 將變更部署至使用開發人員外掛程式AEM或使用您Maven技能的本機例項。

   >[!NOTE]
   >
   > CSS和JavaScript經常由瀏覽器快取，以利效能。 如果您未立即看到客戶端庫的更改，請執行硬刷新並清除瀏覽器的快取。 使用Incognito窗口可以確保新快取。

## 恭喜！{#congratulations}

恭喜您，您剛在Adobe Experience Manager學到元件開發的基本知識！

### 後續步驟{#next-steps}

在下一章[頁面和範本](pages-templates.md)中熟悉Adobe Experience Manager頁面和範本。 瞭解核心元件如何在專案中代管，並瞭解可編輯範本的進階原則設定，以建立結構良好的文章頁面範本。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git分支`tutorial/component-basics-solution`上，在本機檢視並部署程式碼。
