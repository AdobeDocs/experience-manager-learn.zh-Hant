---
title: AEM Sites快速入門 — 元件基本知識
description: 透過簡單的「HelloWorld」範例瞭解Adobe Experience Manager (AEM) Sites元件的基礎技術。 探討HTL、Sling模型、使用者端資料庫和作者對話方塊等主題。
version: 6.5, Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
duration: 1715
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 0%

---

# 元件基本知識 {#component-basics}

本章中，讓我們透過簡單的`HelloWorld`範例來探索Adobe Experience Manager (AEM) Sites元件的基礎技術。 對現有元件進行微幅修改，其中涵蓋編寫、HTL、Sling模型、使用者端資料庫等主題。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](./overview.md#local-dev-environment)所需的工具和指示。

視訊中使用的IDE是[Visual Studio Code](https://code.visualstudio.com/)和[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)外掛程式。

## 目標 {#objective}

1. 瞭解HTL範本和Sling模型以動態方式呈現HTML的作用。
1. 瞭解如何使用對話方塊來促進內容的製作。
1. 瞭解使用者端資料庫的基礎知識，以包含CSS和JavaScript以支援元件。

## 您即將建置的內容 {#what-build}

在本章中，您對簡單的`HelloWorld`元件執行了數個修改。 更新`HelloWorld`元件時，您將瞭解AEM元件開發的主要領域。

## 章節起始專案 {#starter-project}

本章以[AEM專案原型](https://github.com/adobe/aem-project-archetype)產生的一般專案為基礎。 觀看以下影片並檢閱[必要條件](#prerequisites)以開始使用！

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並略過出庫入門專案的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

開啟新的命令列終端機，然後執行下列動作。

1. 在空白目錄中，複製[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 您可以選擇繼續使用上一章[專案設定](./project-setup.md)中產生的專案。

1. 瀏覽至`aem-guides-wknd`資料夾。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用以下命令建置專案並將其部署到AEM的本機執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請將`classic`設定檔附加至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 依照指示將專案匯入您偏好的IDE，以設定[本機開發環境](overview.md#local-dev-environment)。

## 元件製作 {#component-authoring}

元件可視為網頁的小型模組建置區塊。 為了重複使用元件，元件必須可設定。 這是透過作者對話方塊完成。 接下來，讓我們編寫簡單元件，並檢查對話方塊中的值如何儲存在AEM中。

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

以下是上述影片中執行的高層級步驟。

1. 在&#x200B;**WKND網站** `>` **US** `>` **en**&#x200B;下方建立名為&#x200B;**元件基本知識**&#x200B;的頁面。
1. 將&#x200B;**Hello World元件**&#x200B;新增至新建立的頁面。
1. 開啟元件的對話方塊並輸入一些文字。 儲存變更以檢視頁面上顯示的訊息。
1. 切換到開發人員模式，並在CRXDE-Lite中檢視內容路徑並檢查元件例項的屬性。
1. 使用CRXDE-Lite檢視`/apps/wknd/components/content/helloworld`中的`cq:dialog`和`helloworld.html`指令碼。

## HTL (HTML範本語言)和對話方塊 {#htl-dialogs}

HTML範本語言或&#x200B;**[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)**&#x200B;是AEM元件用來呈現內容的輕量型伺服器端範本語言。

**對話方塊**&#x200B;定義元件可用的設定。

接下來，讓我們更新`HelloWorld` HTL指令碼，以在文字訊息前顯示額外的問候語。

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

以下是上述影片中執行的高層級步驟。

1. 切換至IDE並開啟專案至`ui.apps`模組。
1. 開啟`helloworld.html`檔案並更新HTML標示。
1. 使用[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)等IDE工具，將檔案變更與本機AEM執行個體同步化。
1. 返回瀏覽器並觀察元件轉譯器已變更。
1. 開啟定義`HelloWorld`元件之對話方塊的`.content.xml`檔案：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話方塊以新增名稱為&#x200B;**Title**&#x200B;且名稱為`./title`的額外文字欄位：

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

1. 重新開啟檔案`helloworld.html`，該檔案代表負責從以下路徑轉譯`HelloWorld`元件的主要HTL指令碼：

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

1. 使用開發人員外掛程式或您的Maven技能，將變更部署到AEM的本機執行個體。

## Sling 模型 {#sling-models}

Sling模型是註釋驅動的Java™ 「POJO」(Plain Old Java™物件)，可方便將資料從JCR對應至Java™變數。 在AEM環境中開發時，這些範本也提供幾個其他細節。

接下來，讓我們對`HelloWorldModel` Sling模型做一些更新，以便在將儲存於JCR中的值輸出到頁面之前，先將它們套用一些商業邏輯。

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. 開啟檔案`HelloWorldModel.java`，這是搭配`HelloWorld`元件使用的Sling模型。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 新增下列匯入陳述式：

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新`@Model`註解以使用`DefaultInjectionStrategy`：

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 將下列行新增至`HelloWorldModel`類別，以將元件的JCR屬性`title`和`text`的值對應至Java™變數：

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

1. 將下列方法`getTitle()`新增至`HelloWorldModel`類別，這會傳回名為`title`的屬性值。 此方法會新增其他邏輯，傳回「此處的預設值！」的字串值。 如果屬性`title`為null或空白：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 將下列方法`getText()`新增至`HelloWorldModel`類別，這會傳回名為`text`的屬性值。 此方法會將字串轉換為所有大寫字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 從`core`模組建置並部署套件：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 若為AEM 6.4/6.5，請使用`mvn clean install -PautoInstallBundle -Pclassic`

1. 更新位於`aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`的檔案`helloworld.html`以使用`HelloWorld`模型的新建立方法。

   已透過HTL指示詞為此元件執行個體具現化`HelloWorld`模型： `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"`，將執行個體儲存到變數`model`。

   `HelloWorld`模型執行個體現在可以透過`model`變數，使用`HelloWord`在HTL中使用。 這些方法引動可以使用縮短的方法語法，例如： `${model.getTitle()}`可以縮短為`${model.title}`。

   同樣地，所有HTL指令碼都插入了[全域物件](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html)，您可以使用與Sling模型物件相同的語法來存取這些物件。

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
   </div>
   ```

1. 使用Eclipse開發人員外掛程式或您的Maven技能，將變更部署到AEM的本機執行個體。

## 用戶端資源庫 {#client-side-libraries}

使用者端資料庫（簡稱`clientlibs`）提供整理和管理AEM Sites實作所需CSS和JavaScript檔案的機制。 使用者端資料庫是在AEM的頁面上包含CSS和JavaScript的標準方式。

[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)模組是整合至建置程式的解耦[webpack](https://webpack.js.org/)專案。 如此可讓您使用熱門的前端資料庫，例如Sass、LESS和TypeScript。 `ui.frontend`模組在[使用者端程式庫章節](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md)中有更深入的探討。

接下來，更新`HelloWorld`元件的CSS樣式。

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

以下是上述影片中執行的高層級步驟。

1. 開啟終端機視窗並導覽至`ui.frontend`目錄

1. 在`ui.frontend`目錄中執行`npm install npm-run-all --save-dev`命令以安裝[npm-run-all](https://www.npmjs.com/package/npm-run-all)節點模組。 此步驟是Archetype 39產生的AEM專案&#x200B;**上的**&#x200B;必要步驟，在即將發行的Archetype版本中，此為不必要步驟。

1. 接下來，執行`npm run watch`命令：

   ```shell
   $ npm run watch
   ```

1. 切換至IDE並開啟專案至`ui.frontend`模組。
1. 開啟檔案`ui.frontend/src/main/webpack/components/_helloworld.scss`。
1. 更新檔案以顯示紅色標題：

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 在終端機中，您應該會看到顯示`ui.frontend`模組正在編譯變更並將其與AEM的本機執行個體同步的活動。

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. 返回瀏覽器，並觀察標題顏色已變更。

   ![元件基本資訊更新](assets/component-basics/color-update.png)

## 恭喜！ {#congratulations}

恭喜，您已瞭解Adobe Experience Manager中元件開發的基本知識！

### 後續步驟 {#next-steps}

在下一章[頁面和範本](pages-templates.md)中熟悉Adobe Experience Manager頁面和範本。 瞭解如何將核心元件代理至專案，並瞭解可編輯範本的進階原則設定，以建置結構良好的文章頁面範本。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上檢視完成的程式碼，或在Git分支`tutorial/component-basics-solution`上檢閱並部署本機的程式碼。
