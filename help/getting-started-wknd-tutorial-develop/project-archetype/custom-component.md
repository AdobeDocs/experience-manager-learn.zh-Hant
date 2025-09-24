---
title: 自訂元件
description: 涵蓋自訂署名元件的端到端建立過程，而此元件會顯示所製作的內容。包括開發一個 Sling 模型來封裝商業邏輯，以便填入署名元件和對應的 HTL 來轉譯該元件。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
duration: 1039
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '3883'
ht-degree: 100%

---

# 自訂元件 {#custom-component}

本教學課程涵蓋自訂 `Byline`AEM 元件的端到端建立過程，而這個元件會顯示在對話框中製作的內容，也會探索如何開發 Sling 模型來封裝填入元件 HTL 的商業邏輯。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具與指示。

### 入門專案

>[!NOTE]
>
> 若已成功完成上一章，您可以重複使用該專案並略過摸索入門專案的步驟。

查看作為本教學課程基礎內容的基準程式碼：

1. 查看來自 [GitHub](https://github.com/adobe/aem-guides-wknd) 的 `tutorial/custom-component-start` 分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. 使用您的 Maven 技能將程式碼基底部署到本機 AEM 實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 若是使用 AEM 6.5 或 6.4，請將 `classic` 設定檔附加到任何 Maven 命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您隨時可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) 上檢視完成的程式碼，或透過切換到分支 `tutorial/custom-component-solution` 在本機查看程式碼。

## 目標

1. 了解如何建置自訂 AEM 元件
1. 了解如何使用 Sling 模型封裝商業邏輯
1. 了解如何在 HTL 指令碼中使用 Sling 模型

## 您將要建置的內容 {#what-build}

在 WKND 教學課程的這一部分，我們建立一個署名元件，用於顯示所製作的有關文章投稿人的資訊。

![署名元件範例](assets/custom-component/byline-design.png)

*署名元件*

署名元件的實施包括一個收集署名內容的對話框，以及一個檢索以下詳細資訊的自訂 Sling 模型：

* 名稱
* 影像
* 職業

## 建立署名元件 {#create-byline-component}

首先，建立署名元件節點結構並定義一個對話框。這代表 AEM 中的元件，並透過其在 JCR 中的位置用隱含的方式定義元件的資源類型。

對話框提供作者可以提供內容的介面。為了此一實施，使用 AEM WCM 核心元件的&#x200B;**影像**&#x200B;元件來處理署名影像之製作和轉譯，因此必須將其設定為此元件的 `sling:resourceSuperType`。

### 新增元件定義 {#create-component-definition}

1. 在&#x200B;**ui.apps** 模組中，導覽至 `/apps/wknd/components` 並建立名為 `byline` 的資料夾。
1. 在 `byline` 資料夾中，新增名為 `.content.xml` 的檔案

   ![用來建立節點的對話框](assets/custom-component/byline-node-creation.png)

1. 使用以下內容填入 `.content.xml` 檔案：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上述 XML 檔案提供元件的定義，包括標題、描述和群組。`sling:resourceSuperType` 指向 `core/wcm/components/image/v2/image`，即[核心影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html?lang=zh-Hant)。

### 建立 HTL 指令碼 {#create-the-htl-script}

1. 在 `byline` 資料夾中，新增檔案 `byline.html`，而此檔案負責元件的 HTML 呈現。檔案名稱與資料夾名稱相同是非常重要的事，因為 Sling 就是使用這個檔案作為預設指令碼來轉譯這個資源類型。

1. 將下列程式碼新增至 `byline.html`。

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

建立 Sling 模型之後，便會[重新瀏覽](#byline-htl) `byline.html`。HTL 檔案的目前狀態，讓元件在 AEM Sites 的頁面編輯器中，當被拖放到頁面上時，可以顯示空狀態。

### 建立對話框定義 {#create-the-dialog-definition}

接下來，使用以下欄位為署名元件定義一個對話框：

* **名稱**：投稿人名稱的文字欄位。
* **影像**：參照至投稿人簡歷照片。
* **職業**：列出投稿人被歸類的職業。職業應按字母順序以升序 (a 到 z) 排序。

1. 在 `byline` 資料夾中建立一個名為 `_cq_dialog` 的資料夾。
1. 在 `byline/_cq_dialog` 中，新增名為 `.content.xml` 的檔案。這是對話框的 XML 定義。新增下列 XML：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
                       <properties
                               jcr:primaryType="nt:unstructured"
                               jcr:title="Properties"
                               sling:resourceType="granite/ui/components/coral/foundation/container"
                               margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                       jcr:primaryType="nt:unstructured"
                                       sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                       margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                               jcr:primaryType="nt:unstructured"
                                               sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   這些對話框節點定義使用 [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) 來控制哪些對話框標籤是從 `sling:resourceSuperType` 元件繼承而來，在這個案例中是&#x200B;**核心元件的影像元件**。

   ![署名的已完成對話框](assets/custom-component/byline-dialog-created.png)

### 建立原則對話框 {#create-the-policy-dialog}

依照與建立對話框相同的方法，建立一個原則對話框 (之前稱為設計對話框)，以隱藏從核心元件的影像元件繼承而來的原則設定中不需要的欄位。

1. 在 `byline` 資料夾中建立一個名為 `_cq_design_dialog` 的資料夾。
1. 在 `byline/_cq_design_dialog` 中，建立一個名為 `.content.xml` 的檔案。使用下列 XML 更新檔案。最簡單的方法是開啟 `.content.xml` 並將下方的 XML 複製/貼上到其中。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   前述&#x200B;**原則對話框** XML 的基礎是從[核心元件的影像元件](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)取得。

   與對話框設定類似，我們使用 [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) 來隱藏以其他方式從 `sling:resourceSuperType`繼承而來的不相關欄位，如具有 `sling:hideResource="{Boolean}true"` 屬性的節點定義所示。

### 部署程式碼 {#deploy-the-code}

1. 透過您的 IDE 或使用您的 Maven 技能將 `ui.apps` 中的變更同步。

   ![匯出到 AEM 伺服器署名元件](assets/custom-component/export-byline-component-aem.png)

## 將元件新增至頁面 {#add-the-component-to-a-page}

為了簡化操作並專注於開發 AEM 元件，我們會依其目前狀態將署名元件新增到元件頁面，以驗證 `cq:Component` 節點定義是否正確。此外亦驗證 AEM 能夠辨識新元件定義而且元件的對話框可供製作使用。

### 將影像新增至 AEM Assets

首先，將範例頭像上傳到 AEM Assets，以便填入署名元件中的影像。

1. 導覽至 AEM Assets 中的洛杉磯滑板場資料夾：[http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks)。

1. 將 **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** 的頭像上傳到資料夾。

   ![頭像已上傳至 AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### 製作元件 {#author-the-component}

接下來，將署名元件新增至 AEM 中的頁面。由於署名元件已透過 `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 定義新增至 **WKND 網站專案 - 內容**&#x200B;元件群組，任何&#x200B;**容器**&#x200B;只要其&#x200B;**原則**&#x200B;允許 **WKND 網站專案 - 內容**&#x200B;元件群組，此署名元件便會自動提供使用。因此文章頁面的版本容器中可以使用此元件。

1. 導覽至洛杉磯滑板場文章：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 從左側邊欄中，將&#x200B;**署名元件**&#x200B;拖放到已開啟的文章頁面的版面容器&#x200B;**底部**。

   ![將署名元件新增到頁面](assets/custom-component/add-to-page.png)

1. 確保左側邊欄已開啟&#x200B;**且可見，並且已選取**&#x200B;資產尋找器**。

1. 選取「**署名元件預留位置**」，便會顯示動作列，然後點選&#x200B;**扳手**&#x200B;圖示開啟對話框。

1. 在開啟對話框而且第一個標籤 (資產) 使用中的情況下，開啟左側邊欄，然後從資產尋找器將影像拖曳到影像放置區。搜尋「stacey」以尋找 WKND ui.content 封裝所提供的 Stacey Roswells 簡歷照片。

   ![將影像新增至對話框](assets/custom-component/add-image.png)

1. 新增影像後，按一下「**屬性**」標籤來輸入「**名稱**」和「**職業**」。

   輸入職業時，請按&#x200B;**反向字母順序**&#x200B;輸入，以便驗證 Sling 模型中實施的字母順序商業邏輯。

   點選右下角的「**完成**」按鈕來儲存變更。

   ![填入署名元件的屬性](assets/custom-component/add-properties.png)

   AEM 作者透過對話框設定和製作元件。此時，在開發署名元件的過程中，已包含用於收集資料的對話框，但尚未新增用於轉譯製作內容的邏輯。因此只有顯示預留位置。

1. 儲存對話框後，導覽至 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) 並檢閱元件的內容如何儲存在 AEM 頁面下的署名元件內容節點上。

   找到洛杉磯滑板場頁面下方的署名元件內容節點，即 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`。

   請注意，屬性名稱 `name`、`occupations`和 `fileReference` 儲存在&#x200B;**署名節點**&#x200B;上。

   另外，請注意節點的 `sling:resourceType` 設定為 `wknd/components/content/byline`，這個內容節點就是這樣繫結到署名元件實施。

   ![CRXDE 中的署名屬性](assets/custom-component/byline-properties-crxde.png)

## 建立署名 Sling 模型 {#create-sling-model}

接下來，我們會建立一個 Sling 模型作為資料模型，並存放署名元件的商業邏輯。

Sling 模型是由註解驅動的 Java™「POJO」(一般的 Java™ 物件)，有助於將 JCR 的資料對應到 Java™ 變數，並可提高在 AEM 環境中開發時的效率。

### 檢閱 Maven 相依性 {#maven-dependency}

署名 Sling 模型依賴 AEM 提供的多個 Java™ API。透過在 `core` 模組的 POM 檔案中列出的 `dependencies`，便可以使用這些 API。此教學課程使用的專案是針對 AEM as a Cloud Service 所建置。然而其獨特之處在於能夠向後相容於 AEM 6.5/6.4。因此同時包含 Cloud Service 和 AEM 6.x 的相依性。

1. 開啟 `<src>/aem-guides-wknd/core/pom.xml` 下方的 `pom.xml` 檔案。
1. 尋找 `aem-sdk-api` 的相依性 - **僅限 AEM as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=zh-hant) 包含 AEM 公開的所有公共 Java™ API。建置這個專案時，預設使用 `aem-sdk-api`。版本是在專案根目錄之下的父系子模組 POM (`aem-guides-wknd/pom.xml`) 中進行維護。

1. 尋找 `uber-jar` 的相依性 - **僅限 AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   唯有叫用 `classic` 設定檔 (即 `mvn clean install -PautoInstallSinglePackage -Pclassic`) 時才會包含 `uber-jar`。同樣地，這是此專案所獨有的。在現實世界的專案中，如果指定的 AEM 版本為 6.5 或 6.4，則由 AEM 專案原型產生的 `uber-jar` 為預設值。

   [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=zh-Hant#experience-manager-api-dependencies) 包含由 AEM 6.x 提供的所有公開 Java™ API。其版本是在專案根目錄之下的父系子模組 POM (`aem-guides-wknd/pom.xml`) 中進行維護。

1. 尋找 `core.wcm.components.core` 的相依性：

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   這是 AEM 核心元件提供的完全公開的 Java™ API。AEM 核心元件是在 AEM 之外另行維護的專案，因此擁有獨立的發行週期。因此，這是需要另外包含的相依性，而且&#x200B;**並未**&#x200B;包含在 `uber-jar` 和 `aem-sdk-api` 當中。

   和 uber-jar 一樣，此相依性的版本是在 `aem-guides-wknd/pom.xml` 之下的父系子模組 POM 檔案中進行維護。

   在本教學課程的後段，我們會使用核心元件影像類別來顯示署名元件中的影像。要建置和編譯 Sling 模型，必須具有核心元件相依性。

### 署名介面 {#byline-interface}

為署名建立一個公開 Java™ 介面。`Byline.java` 定義驅動 `byline.html` HTL 指令碼所需的公開方法。

1. 在其中，`core/src/main/java/com/adobe/aem/guides/wknd/core/models` 資料夾內的 `core` 模組建立一個名為 `Byline.java` 的檔案

   ![建立署名介面](assets/custom-component/create-byline-interface.png)

1. 使用以下方法更新 `Byline.java`：

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   前兩種方法提供署名元件的&#x200B;**名稱**&#x200B;和&#x200B;**職業**&#x200B;的值。

   我們使用 `isEmpty()` 方法來判斷元件是否具有要轉譯的內容，或是元件是否等待被設定。

   請注意，沒有適用於影像的方法；[這部分稍後再檢閱](#tackling-the-image-problem)。

1. 包含 Java™ 類別的 Java™ 封裝 (在本案例中是 Sling 模型) 必須使用封裝的 `package-info.java` 檔案進行版本控制。

   由於 WKND 原始碼的 Java™ 封裝`com.adobe.aem.guides.wknd.core.models`宣告 `1.0.0` 的版本，而且新增非破壞性的介面和方法，所以版本必須增加至 `1.1.0`。開啟在 `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` 的檔案並更新 `@Version("1.0.0")` 成為 `@Version("2.1.0")`。

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

每次對此封裝中的檔案進行變更時，[都必須在語意上調整封裝版本](https://semver.org/)。若不做調整，Maven 專案的 [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd) 會偵測到無效的封裝版本並中斷建置過程。所幸在建置失敗時，Maven 外掛程式會報告無效的 Java™ 封裝版本以及原本應屬的版本。在違反規則的 Java™ 封裝的 `package-info.java` 中，更新其 `@Version("...")` 宣告，改成外掛程式建議的版本以便修正問題。

### 署名實施 {#byline-implementation}

`BylineImpl.java` 是 Sling 模型的實施，且實施前面內容所定義的 `Byline.java` 介面。如需 `BylineImpl.java` 的完整程式碼，請參閱本區段底部。

1. 在 `core/src/main/java/com/adobe/aem/guides/core/models` 之下建立名為 `impl` 的資料夾。
1. 在 `impl` 資料夾中，建立檔案 `BylineImpl.java`。

   ![署名實施檔案](assets/custom-component/byline-impl-file.png)

1. 開啟 `BylineImpl.java`。指定此檔案實施 `Byline` 介面。使用 IDE 的自動完成功能或手動更新檔案，以包含實施 `Byline` 介面所需的方法：

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. 使用以下類別層級的註解來更新 `BylineImpl.java`，即可新增 Sling 模型註解。就是這個 `@Model(..)` 註解讓類別轉變為 Sling 模型。

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   我們來檢閱這個註解及其參數：

   * 把 BylineImpl 部署到 AEM 時，`@Model` 註解會把 BylineImpl 註冊為 Sling 模型。
   * `adaptables` 參數指定此模型可以根據請求進行適配。
   * `adapters` 參數允許將實施類別註冊到署名介面之下。這樣，HTL 指令碼便可以透過介面 (而不是直接透過實施) 呼叫 Sling 模型。[有關適配器的更多詳細資訊請參閱這裡](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)。
   * `resourceType` 指向署名元件資源類型 (先前已建立)，如果有多個實施，可協助解析正確的模型。[有關模型類別與資源類型相關聯的更多詳細資訊，請參閱這裡](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)。

### 實施 Sling 模型方法 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

所實施的第一個方法是 `getName()`，其直接回傳儲存在屬性 `name` 之下的署名 JCR 內容節點的值。

為此，我們使用 `@ValueMapValue` Sling 模型註解，以便利用請求資源的 ValueMap 將值注入 Java™ 欄位。


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

由於 JCR 屬性與 Java™ 欄位共用名稱 (兩者都是「名稱」)，`@ValueMapValue` 會自動解析此關聯並將屬性的值注入 Java™ 欄位。

#### getOccupations() {#implementing-get-occupations}

下一個要實施的方法是 `getOccupations()`。此方法載入儲存在 JCR 屬性 `occupations` 中的職業，並傳回 (按字母順序) 排序的職業集合。

使用在 `getName()` 中探索的同一種技術，可以將屬性值注入到 Sling 模型的欄位中。

只要透過被注入的 Java™ 欄位 `occupations` 讓 Sling 模型可以使用 JCR 屬性值，`getOccupations()` 方法即可套用排序的商業邏輯。


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

最後一個公開方法是 `isEmpty()`，其決定元件何時應認定自己「已經充分製作」可進行轉譯。

對於此元件，業務要求是所有三個欄位 `name, image and occupations` 必須&#x200B;*先*&#x200B;填寫，然後才可以轉譯元件。


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### 解決「影像問題」 {#tackling-the-image-problem}

檢查名稱和職業的條件是很簡單的事，而 Apache Commons Lang3 會提供方便使用的 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) 類別。但是並不清楚要如何驗證&#x200B;**影像存在**，因為核心元件的影像元件是用來顯示影像的。

有兩種方法可以解決這個問題：

確認 `fileReference` JCR 屬性是否解析為資產。*或者* 將此資源轉換為核心元件影像 Sling 模型並確保 `getSrc()` 方法不是空的。

我們來使用&#x200B;**第二種**&#x200B;方法。第一種方法很可能就足夠解決問題，但在本教學課程中，我們使用後一種方法以便探索 Sling 模型的其他功能。

1. 建立一個取得影像的私有方法。因為不需要對 HTL 本身公開影像物件，而且僅用於驅動 `isEmpty().`，所以此方法可以保持私有狀態。

   新增下列執行 `getImage()` 的私有方法：

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，還有兩種方法可以取得&#x200B;**影像 Sling 模型**：

   第一種方法是使用 `@Self` 註解，自動將目前請求適配到核心元件的 `Image.class`

   第二種方法使用 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi 服務，這項服務很便利，可以幫助我們在 Java™ 程式碼中建立其他類型的 Sling 模型。

   我們來使用第二種方法。

   >[!NOTE]
   >
   >在現實世界的實施中，按照方法「一」使用 `@Self` 是首選，因為那是更簡單、更優雅的解決方案。在本教學課程中，我們使用第二種方法，因為它需要探索 Sling 模型的更多面向，而這在處理較複雜的元件時十分實用！

   由於 Sling 模型是 Java™ POJO 而不是 OSGi 服務，因此&#x200B;**不能**&#x200B;使用慣常的 OSGi 注入註解 `@Reference`，反而是 Sling 模型提供具備相同功能的特殊 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 註解。

1. 更新 `BylineImpl.java` 以便包含 `OSGiService` 註解來注入 `ModelFactory`：

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   在 `ModelFactory` 可用的情況下，可以使用以下方法建立核心元件影像 Sling 模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   然而，這種方法同時需要請求和資源，而在 Sling 模型中目前兩者皆不可用。為了取得這些，要使用更多的 Sling 模型註解！

   若要取得目前請求，可以使用 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 註解將 `adaptable` (在 `@Model(..)` 中定義為 `SlingHttpServletRequest.class`) 注入到 Java™ 類別欄位中。

1. 新增 **@Self** 註解以取得 **SlingHttpServletRequest 請求**：

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   請記住，在上述內容中，使用 `@Self Image image` 注入核心元件影像 Sling 模型是一個選項；`@Self` 註解嘗試注入可適配物件 (在本案例中為 SlingHttpServletRequest)，並適配至註解欄位類型。由於核心元件影像 Sling 模型可從 SlingHttpServletRequest 物件進行適配，因此這種方法應該可行，而且程式碼數量比偏向探索性的 `modelFactory` 方法更少。

   現在已注入透過 ModelFactory API 實例化影像模型所需的變數。我們會使用 Sling 模型的 **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 註解，以便在 Sling 模型實例化之後取得此物件。

   `@PostConstruct` 非常實用，而且其作用與建構函數類似，但是此註解是在類別已實例化而且所有註解的 Java™ 欄位皆已注入後，才能叫用。其他 Sling 模型註解會註解 Java™ 類別欄位 (變數)，而 `@PostConstruct` 則是註解空白、零參數的方法，通常名為 `init()` (但可以是任何名稱)。

1. 新增 **@PostConstruct** 方法：

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   請記住，Sling 模型&#x200B;**不是** OSGi 服務，因此維護類別狀態是安全的。`@PostConstruct` 通常會衍生並設定 Sling 模型類別狀態以供日後使用，類似於普通建構函數的作用。

   如果 `@PostConstruct` 方法發生異常，Sling 模型不會實例化且為 null。

1. 現在可以更新 **getImage()**，以便直接回傳影像物件。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 讓我們回到 `isEmpty()` 並完成實施：

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   請注意，多次呼叫 `getImage()` 並不會造成問題，因為會傳回已經實例化的 `image` 類別變數，而且不會叫用 `modelFactory.getModelFromWrappedRequest(...)`，雖然這個方法成本不是過度高昂，但是沒有必要時應避免呼叫。

1. 最後的 `BylineImpl.java` 看起來應是這樣：


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## 署名 HTL {#byline-htl}

在 `ui.apps` 模組中，開啟在稍早設定 AEM 元件時建立的 `/apps/wknd/components/byline/byline.html`。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

我們回顧一下這個 HTL 指令碼到目前為止所做的事情：

* `placeholderTemplate` 指向核心元件的預留位置，當元件未設定完成時便會顯示這個預留位置。這樣會讓 AEM Sites 頁面編輯器轉譯為附有元件標題的方塊，如上文的 `cq:Component` 之 `jcr:title` 屬性所定義。

* `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 載入上述定義的 `placeholderTemplate`，並傳入一個布林值 (目前硬編碼為 `false`) 到預留位置範本中。當 `isEmpty` 為 true 時，預留位置範本將轉譯為灰色方框，否則不轉譯任何內容。

### 更新署名 HTL

1. 使用以下骨架式 HTML 結構更新 **byline.html**：

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   請注意，CSS 類別遵循 [BEM 命名慣例](https://getbem.com/naming/)。雖然不是強制使用但是建議使用 BEM 命名慣例，因為這是用於核心元件 CSS 類別，並且通常會產生乾淨、可讀的 CSS 規則。

### 在 HTL 中實例化 Sling 模型物件 {#instantiating-sling-model-objects-in-htl}

[使用區塊陳述式](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use)用於實例化 HTL 指令碼中的 Sling 模型物件，並將其指派給 HTL 變數。

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 使用由 BylineImpl 實施的署名介面 (com.adobe.aem.guides.wknd.models.Byline)，並將目前的 SlingHttpServletRequest 適配到該介面，然後將結果儲存在 HTL 變數名稱署名 (`data-sly-use.<variable-name>`) 中。

1. 更新外層的 `div`，以便依照其公開介面參照&#x200B;**署名** Sling 模型。

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### 存取 Sling 模型方法 {#accessing-sling-model-methods}

HTL 向 JSTL 借用相同的 Java™ getter 方法名稱簡化規則。

例如，叫用署名 Sling 模型的 `getName()` 方法可以簡化為 `byline.name`，同樣地，不要使用 `byline.isEmpty`，而可以簡化為 `byline.empty`。使用完整的方法名稱，`byline.getName` 或者 `byline.isEmpty`，也一樣有效。請注意，從未使用 `()` 來叫用 HTL 中的方法 (類似於 JSTL)。

在 HTL 中&#x200B;**不能**&#x200B;使用需要參數的 Java™ 方法。這是為了保持 HTL 的簡單邏輯所刻意設計的。

1. 在署名 Sling 模型上叫用 `getName()` 方法或在 HTL 中使用 `${byline.name}`，可以將署名名稱新增到元件中。

   更新 `h2` 標記：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用 HTL 運算式選項 {#using-htl-expression-options}

[HTL 運算式選項](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options)可用作 HTL 中內容的修飾符，而且從日期格式到國際化翻譯皆可。也可以使用運算式來連接值的清單或陣列，這正是以逗號分隔格式顯示職業所需要的功能。

使用 HTL 運算式中的 `@` 運算子可新增運算式。

1. 若要使用「,」來連接職業清單，請使用以下程式碼：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 依條件顯示預留位置 {#conditionally-displaying-the-placeholder}

用於 AEM 元件的大部分 HTL 指令碼使用&#x200B;**預留位置典範**，為作者提供視覺線索，**指出未正確製作的某個元件，而且不會在 AEM Publish 中顯示**。促成這項決定的慣例，就是對元件背後的 Sling 模型實施方法，在這個案例中是 `Byline.isEmpty()`。

在署名 Sling 模型上叫用 `isEmpty()` 方法，然後把結果 (或者如果是負數，透過 `!` 運算子) 儲存到名為 `hasContent` 的 HTL 變數中：

1. 更新外層 `div` 以儲存名為 `hasContent` 的 HTL 變數：

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   請注意，使用 `data-sly-test` 時，HTL `test` 區塊是關鍵，其設定 HTL 變數而且轉譯或不轉譯其所在的 HTML 元素。這是以 HTL 運算式評估的結果為依據。若為「true」，則 HTML 元素會轉譯，否則便不轉譯。

   現在可以重複使用這個 HTL 變數 `hasContent`，以便按照條件顯示或隱藏預留位置。

1. 使用以下內容更新檔案底部對 `placeholderTemplate` 的條件式呼叫：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心元件顯示影像 {#using-the-core-components-image}

`byline.html` 的 HTL 指令碼現在已將近完成，僅缺少影像。

由於 `sling:resourceSuperType` 指向核心元件的影像元件來製作影像，因此可以使用核心元件的影像元件來轉譯影像。

為此，我們會包含目前的署名資源，但會使用資源類型 `core/wcm/components/image/v2/image`，強制使用核心元件的影像元件資源類型。這個方式能夠發揮重複使用元件之強大效用。為此，我們使用 HTL 的 `data-sly-resource` 區塊。

1. 使用以下內容，將 `div` 取代為類別 `cmp-byline__image`：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   這個 `data-sly-resource`，透過相對路徑 `'.'` 包含目前資源，並強制包含資源類型為 `core/wcm/components/image/v2/image` 的目前資源 (或署名內容資源)。

   可直接使用核心元件資源類型，而不是透過 Proxy，因為這是在指令碼內使用，而且絕不會保留至內容中。

2. 以下是已完成的 `byline.html`：

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. 將程式碼基底部署到本機 AEM 實例。因為 `core` 和 `ui.apps` 皆有進行變更，所以必須部署兩個模組。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   若要部署到 AEM 6.5/6.4，請叫用 `classic` 設定檔：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > 您也可以使用 Maven 設定檔 `autoInstallSinglePackage` 從根目錄建置整個專案，但這可能會覆寫頁面上的內容變更。這是因為我們已經為了教學課程入門程式碼而修改 `ui.content/src/main/content/META-INF/vault/filter.xml`，以便乾淨地覆寫現有的 AEM 內容。在現實世界的情境中，這不會構成任何問題。

### 檢閱無樣式的署名元件 {#reviewing-the-unstyled-byline-component}

1. 部署更新後，導覽至[洛杉磯滑板場終極指南](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)頁面，或您在本章前面新增署名元件的任何位置。

1. 現在會顯示&#x200B;**影像**、**名稱**&#x200B;和&#x200B;**職業**，而且出現無樣式但可運作的署名元件。

   ![無樣式署名元件](assets/custom-component/unstyled.png)

### 檢閱 Sling 模型註冊 {#reviewing-the-sling-model-registration}

[AEM 網頁控制台的 Sling 模型狀態視圖](http://localhost:4502/system/console/status-slingmodels)顯示 AEM 中所有已註冊的 Sling 模型。檢閱這份清單便可以驗證署名 Sling 模型是否已經安裝且被識別。

如果此清單中未顯示 **BylineImpl**，則可能是 Sling 模型的註解發生問題，或是模型未新增到核心專案中的正確封裝 (`com.adobe.aem.guides.wknd.core.models`)。

![已註冊署名 Sling 模型](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 署名樣式 {#byline-styles}

為了讓署名元件與所提供的創意設計相符，我們要設定其樣式。使用 SCSS 檔案並更新 **ui.frontend** 模組中的檔案即可做到。

### 新增預設樣式

新增署名元件的預設樣式。

1. 返回 IDE 以及在 `/src/main/webpack/components` 之下的 **ui.frontend** 專案：
1. 建立一個名為 `_byline.scss` 的檔案。

   ![署名專案檔案總管](assets/custom-component/byline-style-project-explorer.png)

1. 將署名實施 CSS (寫成 SCSS) 新增到 `_byline.scss` 中：

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. 開啟終端機並導覽到 `ui.frontend` 模組。
1. 使用以下 npm 指令啟動 `watch` 程序：

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 返回瀏覽器並導覽至[洛杉磯滑板場文章](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。您應該會看到已更新樣式的元件。

   ![已完成的署名元件](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 您可能需要清除瀏覽器快取以確保不會提供過時的 CSS，並使用署名元件來重新整理頁面以便顯示完整的樣式。

## 恭喜！ {#congratulations}

恭喜，您已使用 Adobe Experience Manager 從頭開始建立自訂元件！

### 後續步驟 {#next-steps}

繼續了解 AEM 元件開發，探索如何為署名 Java™ 程式碼編寫 JUnit 測試，以確保正確開發所有內容，而且所實施的商業邏輯皆正確且完整。

* [編寫單元測試或 AEM 元件](unit-testing.md)

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上檢視已完成的程式碼，或在 Git 分支 `tutorial/custom-component-solution` 上本機檢閱與部署程式碼。

1. 原地複製 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 查看 `tutorial/custom-component-solution` 分支。
