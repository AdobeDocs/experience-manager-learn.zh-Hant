---
title: AEM Sites入門 — 元件基礎
description: 通過簡單的「HelloWorld」示例AEM瞭解Adobe Experience Manager()站點元件的底層技術。 探討了HTL、Sling模型、客戶端庫和作者對話的主題。
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 1%

---

# 元件基礎 {#component-basics}

在本章中，我們將通過一個簡單的IP，來探AEM討Adobe Experience Manager()站點元件的底層技術 `HelloWorld` 示例。 將對現有元件進行小量修改，內容涵蓋創作、HTL、Sling模型、客戶端庫等主題。

## 必備條件 {#prerequisites}

查看所需的工具和設定 [地方開發環境](./overview.md#local-dev-environment)。

視頻中使用的IDE是 [Visual Studio代碼](https://code.visualstudio.com/) 和 [VSCode同AEM步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 插件。

## 目標 {#objective}

1. 瞭解HTL模板和Sling模型的角色以動態渲染HTML。
1. 瞭解如何使用對話框來方便內容的創作。
1. 瞭解客戶端庫的基本知識，以包括支援元件的CSS和JavaScript。

## 您將構建的 {#what-you-will-build}

在本章中，您將對非常簡單的 `HelloWorld` 元件。 在更新 `HelloWorld` 元件將瞭解元件開發的AEM關鍵領域。

## 第一章啟動項目 {#starter-project}

本章基於由 [項AEM目原型](https://github.com/adobe/aem-project-archetype)。 觀看以下視頻並查看 [先決條件](#prerequisites) 開始！

>[!NOTE]
>
> 如果成功完成了上一章，則可以重新使用項目，並跳過簽出啟動程式項目的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

開啟新的命令行終端並執行以下操作。

1. 在空目錄中，克隆 [埃姆 — 吉德 — 溫德](https://github.com/adobe/aem-guides-wknd) 儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > （可選）您可以繼續使用上一章中生成的項目。 [項目設定](./project-setup.md)。

1. 導航到  `aem-guides-wknd` 的子菜單。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用以下命令生成項目並將其部署到AEM的本地實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使AEM用6.5或6.4，則追加 `classic` 配置檔案。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 按照說明將項目導入首選IDE，以設定 [地方開發環境](overview.md#local-dev-environment)。

## 元件創作 {#component-authoring}

元件可以被視為網頁的小模組化構造塊。 為了重新使用元件，元件必須是可配置的。 這是通過作者對話框完成的。 接下來，我們將編寫一個簡單的元件並檢查對話框中的值是如何保留AEM的。

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

下面是上述視頻中執行的高級步驟。

1. 建立名為 **元件基礎** 下 **WKND站點** `>` **美國** `>` **恩**。
1. 添加 **Hello World元件** 的子菜單。
1. 開啟元件的對話框並輸入一些文本。 保存更改以查看頁面上顯示的消息。
1. 切換到開發模式，在CRXDE-Lite中查看內容路徑並檢查元件實例的屬性。
1. 使用CRXDE-Lite查看 `cq:dialog` 和 `helloworld.html` 位於 `/apps/wknd/components/content/helloworld`。

## HTL(HTML模板語言)和對話框 {#htl-dialogs}

HTML模板語言或 **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)** 是元件用於呈現內容的輕量級伺服器端模AEM板語言。

**對話框** 定義可為元件建立的配置。

接下來，我們將更新 `HelloWorld` HTL指令碼，在文本消息前顯示附加問候語。

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

下面是上述視頻中執行的高級步驟。

1. 切換到IDE並將項目開啟到 `ui.apps` 中。
1. 開啟 `helloworld.html` 的子菜單。
1. 使用IDE工具，如 [VSCode同AEM步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 將檔案更改與本地實例同AEM步。
1. 返回到瀏覽器並觀察元件呈現已更改。
1. 開啟 `.content.xml` 定義對話框的檔案 `HelloWorld` 元件位於：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話框以添加名為 **標題** 名為 `./title`:

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

1. 重新開啟檔案 `helloworld.html`，它表示負責呈現的主HTL指令碼 `HelloWorld` 元件，位於：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新 `helloworld.html` 顯示 **問候語** 作為文本欄位的一部分 `H1` 標籤：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 將更改部署到使用開發人員插AEM件或使用Maven技能的本地實例。

## Sling 模型 {#sling-models}

Sling模型是注釋驅動的Java &quot;POJO&#39;s&quot;(Plain Old Java Objects)，它便於將資料從JCR映射到Java變數，並在上下文中開發時提供許多其他細AEM節。

接下來，我們將對 `HelloWorldModel` Sling Model（Sling模型），用於在將某些業務邏輯輸出到頁面之前，將其應用於JCR中儲存的值。

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. 開啟檔案 `HelloWorldModel.java`，即與 `HelloWorld` 元件。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 添加以下導入語句：

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新 `@Model` 注釋以使用 `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 將以下行添加到 `HelloWorldModel` 類以映射元件的JCR屬性的值 `title` 和 `text` 到Java變數：

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

1. 添加以下方法 `getTitle()` 到 `HelloWorldModel` 類，返回名為 `title`。 此方法添加附加邏輯以返回「此處為預設值！」的字串值 如果 `title` 為空或為空：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 添加以下方法 `getText()` 到 `HelloWorldModel` 類，返回名為 `text`。 此方法將字串轉換為全部大寫字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 從 `core` 模組：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 如果使用AEM6.4/6.5使用 `mvn clean install -PautoInstallBundle -Pclassic`

1. 更新檔案 `helloworld.html` 在 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 使用 `HelloWorld` 模型：

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

1. 將更改部署到使用Eclipse Developer插AEM件或使用Maven技能的本地實例。

## 用戶端資源庫 {#client-side-libraries}

客戶端庫（簡稱客戶端庫）提供了一種機制，用於組織和管理AEM Sites實現所需的CSS和JavaScript檔案。 客戶端庫是在中的頁面上包含CSS和JavaScript的標準方AEM法。

的 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模組是 [網路包](https://webpack.js.org/) 整合到生成流程中的項目。 這允許使用常用的前端庫，如Sass、LESS和TypeScript。 的 `ui.frontend` 將更深入地研究模組 [「客戶端庫」一章](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md)。

接下來，更新 `HelloWorld` 元件。

>[!VIDEO](https://video.tv.adobe.com/v/340750/?quality=12&learn=on)

下面是上述視頻中執行的高級步驟。

1. 開啟終端窗口並導航到 `ui.frontend` 目錄和

1. 在 `ui.frontend` 目錄運行 `npm run watch` 命令：

   ```shell
   $ npm run watch
   ```
1. 切換到IDE並將項目開啟到 `ui.frontend` 中。
1. 開啟檔案 `ui.frontend/src/main/webpack/components/_helloworld.scss`。
1. 更新檔案以顯示紅色標題：

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 在終端中，您應看到顯示 `ui.frontend` 模組正在編譯更改並與的本地實例同步AEM。

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

1. 返回到瀏覽器並觀察標題顏色已更改。

   ![元件基礎更新](assets/component-basics/color-update.png)

## 恭喜！ {#congratulations}

祝賀您，您剛剛在Adobe Experience Manager學到了元件開發的基礎知識！

### 後續步驟 {#next-steps}

熟悉下一章中的Adobe Experience Manager頁面和模板 [頁面和模板](pages-templates.md)。 瞭解如何將核心元件代理到項目中，並瞭解可編輯模板的高級策略配置以構建結構良好的文章頁面模板。

查看完成的代碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上本地查看和部署代碼 `tutorial/component-basics-solution`。
