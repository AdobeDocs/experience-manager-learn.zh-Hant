---
title: AEM Sites 快速入門 - 元件基礎知識
description: 透過一個簡單的「HelloWorld」範例來了解 Adobe Experience Manager (AEM) Sites 元件的基礎技術。探討 HTL、Sling 模型、用戶端程式庫和作者對話框等主題。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1192'
ht-degree: 100%

---

# 元件基礎知識 {#component-basics}

在本章中，我們透過一個簡單的 `HelloWorld` 範例來探索 Adobe Experience Manager (AEM) Sites 元件的基礎技術。我們對某個現有元件進行小幅度的修改，涵蓋製作、HTL、Sling 模型、用戶端程式庫等主題。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](./overview.md#local-dev-environment)所需的工具與指示。

影片中使用的 IDE 是 [Visual Studio Code](https://code.visualstudio.com/) 和 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 外掛程式。

## 目標 {#objective}

1. 了解 HTL 範本和 Sling 模型在動態轉譯 HTML 時扮演的角色。
1. 了解如何使用對話框來協助內容的製作。
1. 了解用戶端程式庫的基礎知識，以包含用於支援元件的 CSS 和 JavaScript。

## 您將要建置的內容 {#what-build}

在本章中，您將對簡單的 `HelloWorld` 元件執行幾項修改。在對 `HelloWorld` 元件進行更新的過程中，您會了解 AEM 元件開發的關鍵領域。

## 本章入門專案 {#starter-project}

本章以 [AEM 專案原型](https://github.com/adobe/aem-project-archetype)所產生的通用專案為基礎。觀看以下影片並檢閱[先決條件](#prerequisites)以便開始使用！

>[!NOTE]
>
> 若已成功完成上一章，您可以重複使用該專案並略過摸索入門專案的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

開啟新的命令列終端機並執行以下操作。

1. 在空目錄中，原地複製 [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 您也可以選擇繼續使用上一章「[專案設定](./project-setup.md)」中產生的專案。

1. 導覽至 `aem-guides-wknd` 資料夾。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用以下命令建置專案並部署到 AEM 的本機實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 若是使用 AEM 6.5 或 6.4，請將 `classic` 設定檔附加到任何 Maven 命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 依照設定[本機開發環境](overview.md#local-dev-environment)的指示，將專案匯入您偏好的 IDE。

## 元件製作 {#component-authoring}

您可以把元件想像成網頁的小型模組化建構區塊。若要重複使用元件，元件必須可以設定。您可以透過作者對話框來完成元件設定。接下來，我們來製作一個簡單的元件，並檢查對話框中的值如何在 AEM 中保留。

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

以下是上述影片中執行的概括性步驟。

1. 在「**WKND 網站** `>` **US** `>` **EN**」之下，建立名為「**元件基礎知識**」的頁面。
1. 將 **Hello World 元件**&#x200B;新增至新建立的頁面。
1. 開啟元件的對話框並輸入一些文字。儲存變更，以便在頁面上看到該訊息。
1. 切換到開發者模式並檢視 CRXDE-Lite 中的內容路徑，然後檢查元件實例的屬性。
1. 使用 CRXDE-Lite 檢視來自 `/apps/wknd/components/content/helloworld` 的 `cq:dialog` 和 `helloworld.html`。

## HTL (HTML 範本語言) 與對話框 {#htl-dialogs}

HTML 範本語言或 **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=zh-Hant)** 是一種輕量級的伺服器端範本語言，供 AEM 元件轉譯內容時使用。

**對話框**&#x200B;定義元件可以進行哪些設定。

接著我們來更新 `HelloWorld` HTL 指令碼，以便在文字訊息顯示之前，先顯示額外的問候語。

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

以下是上述影片中執行的概括性步驟。

1. 切換到 IDE 並開啟專案中的 `ui.apps` 模組。
1. 開啟 `helloworld.html` 檔案並更新 HTML 標記。
1. 使用 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 等 IDE 工具將檔案變更同步到本機 AEM 實例。
1. 返回瀏覽器，觀察並確認元件轉譯已經變更。
1. 開啟定義 `HelloWorld` 元件對話框的 `.content.xml` 檔案，位於：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話框，以新增名為「**Title**」的額外文字欄位，其名稱為 `./title`：

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

1. 重新開啟檔案 `helloworld.html`，這是代表負責從下列路徑轉譯 `HelloWorld` 元件的主要 HTL 指令碼：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新 `helloworld.html`，以便將「**問候語**」文字欄位的值轉譯成為 `H1` 標記的一部分：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 使用開發人員外掛程式或您的 Maven 技能，將變更部署到 AEM 本機實例。

## Sling 模型 {#sling-models}

Sling 模型是由註解驅動的 Java™「POJO」(一般的 Java™ 物件)，有助於將 JCR 的資料對應到 Java™ 變數。在 AEM 環境中進行開發時，這些模型還擁有一些其他優點。

接著我們對 `HelloWorldModel` Sling 模型進行一些更新，對儲存在 JCR 的值套用一些商業邏輯，然後才能將這些值輸出到頁面。

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. 開啟檔案 `HelloWorldModel.java`，這是與 `HelloWorld` 元件搭配使用的 Sling 模型。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 新增以下匯入語句：

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新 `@Model` 註解以便使用 `DefaultInjectionStrategy`：

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 將下列數行程式碼新增到 `HelloWorldModel` 類別，以便將元件的 JCR 屬性 `title` 和 `text` 對應到 Java™ 變數：

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

1. 將以下方法 `getTitle()` 新增到 `HelloWorldModel` 類別，這樣會傳回名為 `title` 的屬性之值。這個方法會新增額外的邏輯，如果屬性 `title` 為 Null 或空白的話，傳回字串值「Default Value here!」：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 將以下方法 `getText()` 新增到 `HelloWorldModel` 類別，這樣會傳回名為 `text` 的屬性之值。此方法會將字串轉換成全部大寫字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 從 `core` 模組開始建置及部署套件：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 適用於 AEM 6.4/6.5`mvn clean install -PautoInstallBundle -Pclassic`

1. 更新位在 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 的檔案 `helloworld.html`，以使用 `HelloWorld` 模型的新建立方法。

   `HelloWorld` 模式是透過 HTL 指令 `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"` 針對這個元件實例進行初始化，並將實例儲存到變數 `model`

   現在，透過 `model` 變數並使用 `HelloWord`，即可在 HTL 中使用 `HelloWorld` 模型實例。這些方法叫用可以使用縮短的方法語法，例如：`${model.getTitle()}` 可以縮短成 `${model.title}`。

   同樣地，所有 HTL 指令碼均已注入[全域物件](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html?lang=zh-Hant)，您可以使用與 Sling 模型物件相同的語法存取這些物件。

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

1. 使用 Eclipse Developer 外掛程式或您的 Maven 技能，將變更部署到 AEM 本機實例。

## 用戶端程式庫 {#client-side-libraries}

用戶端程式庫，簡稱 `clientlibs`，是用來組織和管理 AEM Sites 實施所需的 CSS 和 JavaScript 檔案的一種機制。用戶端程式庫是在 AEM 中的頁面上包含 CSS 和 JavaScript 的標準方式。

[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hant) 模組是一個已解耦並整合到建置流程之中的 [webpack](https://webpack.js.org/) 專案，藉由這個模組，可使用常見的前端程式庫，如 Sass、LESS 和 TypeScript。在[用戶端程式庫章節](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md)會對 `ui.frontend` 模組進行深入探討。

接下來，更新 `HelloWorld` 元件的 CSS 樣式。

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

以下是上述影片中執行的概括性步驟。

1. 開啟終端機視窗並導覽到 `ui.frontend` 目錄

1. 在 `ui.frontend` 目錄中執行 `npm install npm-run-all --save-dev` 命令來安裝 [npm-run-all](https://www.npmjs.com/package/npm-run-all) 節點模組。**使用原型 39 產生的 AEM 專案必須執行**&#x200B;這個步驟，在即將推出的原型版本中，這個步驟並非必要。

1. 接著，執行 `npm run watch` 命令：

   ```shell
   $ npm run watch
   ```

1. 切換到 IDE 並開啟專案中的 `ui.frontend` 模組。
1. 開啟檔案 `ui.frontend/src/main/webpack/components/_helloworld.scss`。
1. 更新檔案以顯示紅色標題：

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 在終端機中，您應該看到活動表示 `ui.frontend` 模組正在進行編譯，並將變更同步到 AEM 本機實例。

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

1. 返回瀏覽器，觀察並確認標題顏色已經改變。

   ![元件基礎知識更新](assets/component-basics/color-update.png)

## 恭喜！ {#congratulations}

恭喜，您已經了解 Adobe Experience Manager 中元件開發的基礎知識！

### 後續步驟 {#next-steps}

透過下一章「[頁面和範本](pages-templates.md)」，熟悉 Adobe Experience Manager 各式頁面和範本。了解如何以代理方式將核心元件引入專案中，並了解可編輯範本的進階原則設定，以建立結構良好的文章頁面範本。

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上檢視已完成的程式碼，或在 Git 分支 `tutorial/component-basics-solution` 上本機檢閱與部署程式碼。
