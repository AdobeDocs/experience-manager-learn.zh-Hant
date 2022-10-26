---
title: 自訂元件
description: 涵蓋顯示製作內容的自訂署文元件的端對端建立。 包括開發Sling模型以封裝商業邏輯，以填入署名元件和對應的HTL來轉譯元件。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '4131'
ht-degree: 0%

---

# 自訂元件 {#custom-component}

本教學課程涵蓋以端對端方式建立自訂AEM Byline元件，以顯示在對話方塊中撰寫的內容，並探討開發Sling模型，以封裝填入元件HTL的商業邏輯。

## 必備條件 {#prerequisites}

檢閱設定 [本地開發環境](overview.md#local-dev-environment).

### 入門專案

>[!NOTE]
>
> 如果成功完成上一章，則可以重新使用項目，並跳過簽出入門項目的步驟。

查看本教學課程所建置的底線程式碼：

1. 查看 `tutorial/custom-component-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. 使用您的Maven技能，將程式碼基底部署至本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請附加 `classic` 配置檔案。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) 或切換到分支，本地檢出代碼 `tutorial/custom-component-solution`.

## 目標

1. 了解如何建立自訂AEM元件
1. 了解如何使用Sling模型封裝商業邏輯
1. 了解如何從HTL指令碼使用Sling模型

## 您將建置的 {#byline-component}

在WKND教學課程的這一部分中，會建立署名元件，以顯示有關文章貢獻者的撰寫資訊。

![署名元件範例](assets/custom-component/byline-design.png)

*署名元件*

Byline元件的實作包含收集署名內容的對話方塊，以及擷取署名的自訂Sling模型：

* 名稱
* 影像
* 職業

## 建立署名元件 {#create-byline-component}

首先，建立「署名元件」節點結構並定義對話框。 這表示AEM中的元件，並依元件在JCR中的位置以隱含方式定義元件的資源類型。

對話方塊會公開內容作者可提供的介面。 針對此實作，AEM WCM核心元件 **影像** 元件可用來製作和轉譯署的影像，因此必須將其設為元件 `sling:resourceSuperType`.

### 建立元件定義 {#create-component-definition}

1. 在 **ui.apps** 模組，導覽至 `/apps/wknd/components` 並建立名為 `byline`.
1. 在 `byline` 資料夾添加名為 `.content.xml`

   ![對話框，建立節點](assets/custom-component/byline-node-creation.png)

1. 填入 `.content.xml` 檔案中填入下列項目：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上述XML檔案提供元件的定義，包括標題、說明和群組。 此 `sling:resourceSuperType` 指向 `core/wcm/components/image/v2/image`，即 [核心影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html).

### 建立HTL指令碼 {#create-the-htl-script}

1. 在 `byline` 資料夾，添加新檔案 `byline.html`，負責元件的HTML呈現。 將檔案命名為與資料夾相同的檔案很重要，因為它會成為Sling用來呈現此資源類型的預設指令碼。

1. 將下列程式碼新增至 `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` is [回訪稍後](#byline-htl)，建立Sling模型後即可。 HTL檔案的目前狀態可讓元件拖放至頁面時，顯示在AEM Sites的「頁面編輯器」中，處於空白狀態。

### 建立對話框定義 {#create-the-dialog-definition}

接下來，使用以下欄位為Byline元件定義一個對話框：

* **名稱**:貢獻者名稱的文字欄位。
* **影像**:參考貢獻者的簡歷。
* **職業**:貢獻者的職業清單。 職業應按字母順序遞增（a至z）。

1. 在 `byline` 資料夾，建立名為 `_cq_dialog`.
1. 下方 `byline/_cq_dialog` 添加名為 `.content.xml`. 這是對話框的XML定義。 添加以下XML:

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

   這些對話方塊節點定義使用 [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) 控制從中繼承的對話框頁簽 `sling:resourceSuperType` 元件，在此例中為 **核心元件的影像元件**.

   ![署名的已完成對話框](assets/custom-component/byline-dialog-created.png)

### 建立策略對話框 {#create-the-policy-dialog}

按照與建立對話框相同的方法，建立「策略」對話框（以前稱為「設計」對話框），以隱藏從核心元件的映像元件繼承的策略配置中不需要的欄位。

1. 在 `byline` 資料夾，建立名為 `_cq_design_dialog`.
1. 下方 `byline/_cq_design_dialog` 建立名為的新檔案 `.content.xml`. 使用下列項目更新檔案：的XML。 最簡單的方式是開啟 `.content.xml` 並將XML複製/貼到下方。

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

   前一個的依據 **策略對話** XML是從 [核心元件影像元件](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   就像在對話方塊設定中， [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) 用於隱藏不相關欄位，而這些欄位原本繼承自 `sling:resourceSuperType`，如節點定義所示，搭配 `sling:hideResource="{Boolean}true"` 屬性。

### 部署程式碼 {#deploy-the-code}

1. 同步中的變更 `ui.apps` 或使用Maven技能。

   ![匯出至AEM伺服器署名元件](assets/custom-component/export-byline-component-aem.png)

## 將元件新增至頁面 {#add-the-component-to-a-page}

為了讓程式簡單明瞭，並著重於AEM元件開發，我們會將目前狀態的署名元件新增至文章頁面，以驗證 `cq:Component` 節點定義已部署且正確，AEM可辨識新元件定義，而元件的對話方塊可用於編寫。

### 新增影像至AEM Assets

首先，將頭像範例上傳至AEM Assets，以用來填入署名元件中的影像。

1. 導覽至AEM Assets的LA Skateparks資料夾： [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. 上傳的頭部鏡頭  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** 到資料夾。

   ![頭照上傳至AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### 撰寫元件 {#author-the-component}

接下來，將署名元件新增至AEM中的頁面。 因為我們將署名元件新增至 **WKND Sites專案 — 內容** 元件群組，透過 `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 定義，它會自動可供任何 **容器** who **原則** 允許 **WKND Sites專案 — 內容** 元件群組，文章頁面的「配置容器」為此元件群組。

1. 導覽至LA Skatepark文章，網址為： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 從左側邊欄，拖放 **署名元件** 在 **底部** 的「版面容器」(Layout Container)。

   ![將署名元件新增至頁面](assets/custom-component/add-to-page.png)

1. 確保 **左側欄開啟** 和可見， **資產尋找器** 中所有規則的URL。

1. 選取 **署名元件預留位置**，動作隨即顯示動作列並點選 **扳手** 圖示以開啟對話方塊。

1. 在對話方塊開啟，且第一個索引標籤（資產）處於作用中狀態時，開啟左側邊欄，然後從資產尋找器將影像拖曳至「影像」放置區。 搜索「stacey」以查找WKND ui.content包中提供的Stacey Roswells簡歷圖片。

   ![將影像添加到對話框](assets/custom-component/add-image.png)

1. 新增影像後，按一下 **屬性** 頁簽 **名稱** 和 **職業**.

   進入職業時，請在 **反向字母** 順序如此一來，我們將在Sling模型中實作的商業邏輯依字母順序排列就顯而易見。

   點選 **完成** 按鈕以儲存變更。

   ![署名元件填充屬性](assets/custom-component/add-properties.png)

   AEM作者透過對話方塊來設定和製作元件。 此時，在開發署名元件時，會包含用於收集資料的對話方塊，但轉譯製作內容的邏輯尚未新增。 因此，只會顯示預留位置。

1. 儲存對話方塊後，導覽至 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) 並檢閱元件內容如何儲存在AEM頁面下的署名元件內容節點上。

   在「LA Skate Parks」(LA Skate Parks)頁面下查找Byline元件內容節點，即 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   請注意屬性名稱 `name`, `occupations`，和 `fileReference` 儲存於 **署名節點**.

   此外，請留意 `sling:resourceType` 的 `wknd/components/content/byline` 這是將此內容節點綁定到Byline元件實施的。

   ![CRXDE中的行屬性](assets/custom-component/byline-properties-crxde.png)

## 建立署名Sling模型 {#create-sling-model}

接下來，我們將建立Sling模型作為資料模型，並存放Byline元件的業務邏輯。

Sling模型是註解導向的Java &quot;POJO&#39;s&quot;（純舊Java物件），可方便將資料從JCR對應至Java變數，並在以AEM進行開發時提供許多其他細節。

### 檢閱Maven相依性 {#maven-dependency}

Byline Sling模型將仰賴AEM提供的數個Java API。 這些API可透過 `dependencies` 列於 `core` 模組的POM檔案。 本教學課程使用的專案已針對AEM as a Cloud Service建置。 但它的獨特之處在於它向後相容於AEM 6.5/6.4。因此，它同時包含了Cloud Service和AEM 6.x的依賴項。

1. 開啟 `pom.xml` 檔案下方 `<src>/aem-guides-wknd/core/pom.xml`.
1. 查找 `aem-sdk-api` - **僅AEMas a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   此 [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#building-for-the-sdk) 包含AEM公開的所有公用Java API。 此 `aem-sdk-api` 建置此專案時，預設會使用。 版本會維護在位於專案根目錄的Parent reactor pom中 `aem-guides-wknd/pom.xml`.

1. 尋找 `uber-jar` - **僅AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   此 `uber-jar` 只有在 `classic` 叫用設定檔，即 `mvn clean install -PautoInstallSinglePackage -Pclassic`. 同樣地，這是此專案專屬的。 在真實專案中，從AEM專案原型產生 `uber-jar` 如果指定的AEM版本為6.5或6.4，則為預設值。

   此 [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) 包含AEM 6.x公開的所有公用Java API。版本會維護在位於專案根目錄的Parent reactor pom中 `aem-guides-wknd/pom.xml`.

1. 查找 `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   這是AEM核心元件公開的所有公用Java API。 AEM核心元件是在AEM外部維護的專案，因此有個別的發行週期。 因此，它是需要個別包含的相依性，而是 **not** 隨附 `uber-jar` 或 `aem-sdk-api`.

   與uber-jar一樣，此相依性的版本會保留在位於的Parent reactor pom檔案中 `aem-guides-wknd/pom.xml`.

   在本教學課程的稍後部分，我們將使用核心元件影像類別，在署名元件中顯示影像。 您必須具有核心元件相依性，才能建置和編譯我們的Sling模型。

### 署名介面 {#byline-interface}

為署名建立公用Java介面。 `Byline.java` 定義驅動 `byline.html` HTL指令碼。

1. 在 `aem-guides-wknd.core` 模組下方 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 建立名為的新檔案 `Byline.java`

   ![建立署名介面](assets/custom-component/create-byline-interface.png)

1. 更新 `Byline.java` 搭配下列方法：

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

   前兩種方法會公開 **名稱** 和 **職業** （位元元件）。

   此 `isEmpty()` 方法可用來判斷元件是否有任何要呈現的內容，或元件是否等待設定。

   請注意，沒有影像的方法； [我們稍後會查看為什麼](#tackling-the-image-problem).

1. 包含公用Java類的Java套件（在此例中是Sling模型）必須使用套件的版本控制  `package-info.java` 檔案。

   自WKND源的Java包以來 `com.adobe.aem.guides.wknd.core.models` 聲明為版本 `1.0.0`，而且我們新增了不中斷的公用介面和方法，版本必須增加至 `1.1.0`. 在開啟檔案 `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` 和更新 `@Version("1.0.0")` to `@Version("1.1.0")`.

       &quot;
       @Version(&quot;2.1.0&quot;)
       package com.adobe.aem.guides.wknd.core.models;
       
       import org.osgi.annotation.versioning.Version;
       &quot;
   
每當對此包中的檔案進行更改時， [包版本必須在語義上調整](https://semver.org/). 如果沒有，馬文項目 [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) 將檢測無效的包版本，並中斷生成。 幸運的是，失敗時，Maven外掛程式會報告無效的Java套件版本，以及應該的版本。 只需更新 `@Version("...")` 違反Java包的 `package-info.java` 外掛程式建議的版本修正。

### 署名實作 {#byline-implementation}

`BylineImpl.java` 是實作的Sling模型實作 `Byline.java` 介面。 的完整程式碼 `BylineImpl.java` 可在此區段底部找到。

1. 建立名為 `impl` 在 `core/src/main/java/com/adobe/aem/guides/core/models`.
1. 在 `impl` 資料夾建立新檔案 `BylineImpl.java`.

   ![署名實施檔案](assets/custom-component/byline-impl-file.png)

1. 開啟 `BylineImpl.java`. 指定其實作 `Byline` 介面。 使用IDE的自動完成功能，或手動更新檔案以包括實施所需的方法 `Byline` 介面：

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

1. 更新以新增Sling模型註解 `BylineImpl.java` 並搭配下列類別層級註解。 此 `@Model(..)`annotation是將類轉換為Sling模型。

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

   讓我們檢閱此註解及其參數：

   * 此 `@Model` 註解將BylineImpl部署至AEM時註冊為Sling模型。
   * 此 `adaptables` 參數會指定此模型可由要求調整。
   * 此 `adapters` 參數可讓實施類別在署名介面下註冊。 這可讓HTL指令碼透過介面呼叫Sling模型（而非直接匯入）。 [有關適配器的更多詳細資訊，請參閱此處](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * 此 `resourceType` 指向署名元件資源類型（先前建立），並有助於在有多個實施時解析正確的模型。 [有關將模型類與資源類型關聯的更多詳細資訊，請參見此處](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### 實作Sling模型方法 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

我們要解決的第一個方法是 `getName()` 它只會傳回儲存在屬性下署名的JCR內容節點的值 `name`.

為此， `@ValueMapValue` Sling模型註解可用來使用請求的資源的ValueMap，將值插入Java欄位。


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

因為JCR屬性與Java欄位共用相同的名稱（兩者都是&quot;name&quot;）, `@ValueMapValue` 自動解析此關聯，並將屬性的值插入Java欄位中。

#### getSchropies() {#implementing-get-occupations}

下一個實作方法是 `getOccupations()`. 此方法會收集JCR屬性中儲存的所有職業 `occupations` 並傳回已排序（字母順序）的集合。

使用 `getName()` 屬性值可插入至Sling模型的欄位。

一旦JCR屬性值可在Sling模型中透過插入的Java欄位取得， `occupations`，則可在 `getOccupations()` 方法。


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

最後一個公用方法是 `isEmpty()` 這會決定元件何時應將自身視為「已編寫完畢」而可呈現。

對於此元件，我們有業務要求，指出必須填寫所有三個欄位、名稱、影像和職業 *befor* 可呈現元件。


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


#### 解決&quot;形象問題&quot; {#tackling-the-image-problem}

檢查名稱和職業條件很瑣碎(Apache Commons Lang3提供了始終方便的 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) )，不過，目前尚不清楚 **影像的存在** 可驗證，因為核心元件影像元件用於呈現影像。

有兩種方法可以解決這個問題：

檢查 `fileReference` JCR屬性解析至資產。 *或* 將此資源轉換為核心元件影像Sling模型，並確定 `getSrc()` 方法不為空。

我們選擇 **秒** 方法。 第一種方法可能已足夠，但在本教學課程中，後一種方法可讓我們探索Sling模型的其他功能。

1. 建立取得影像的私人方法。 此方法保留為私密狀態，因為我們不需要在HTL本身公開影像物件，且該物件僅用於驅動 `isEmpty().`

   為新增下列專用方法 `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，還有兩種方法可取得 **影像Sling模型**:

   第一個會使用 `@Self` 附註，以自動調整目前要求與核心元件的 `Image.class`

   第二個會使用 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服務是一項非常實用的服務，可協助我們在Java程式碼中建立其他類型的Sling模型。

   我們將選擇第二種方法。

   >[!NOTE]
   >
   >在實際實作中，採用「一」方法，使用 `@Self` 更簡單、更優雅，因此更受青睞。 在本教學課程中，我們將使用第二種方法，因為這需要我們探索Sling模型的更多方面，這些方面對於更複雜的元件非常有用！

   由於Sling模型是Java POJO的，而非OSGi Services，因此通常的OSGi插入註解 `@Reference` **不能** ，而Sling模型會提供特殊 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 提供類似功能的注釋。

1. 更新 `BylineImpl.java` 包括 `OSGiService` 要插入的注釋 `ModelFactory`:

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

   使用 `ModelFactory` 可用，則可使用下列方式建立核心元件影像Sling模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   不過，此方法同時需要要求和資源，但Sling模型中均不提供。 若要取得這些資訊，需使用更多Sling模型註解！

   若要取得目前的要求，請 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 注釋可用於插入 `adaptable` (定義於 `@Model(..)` as `SlingHttpServletRequest.class`，放入Java類欄位中。

1. 新增 **@Self** 附註以取得 **SlingHttpServletRequest要求**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   記住，使用 `@Self Image image` 若要插入核心元件影像Sling模型，上方是選項 —  `@Self` 註解會嘗試插入可調整的物件（在我們的例子中是SlingHttpServletRequest），並適應註解欄位類型。 由於核心元件影像Sling模型可從SlingHttpServletRequest物件調整，因此此方法原本可行，且程式碼比較少，比較探索性的方法。

   現在，我們已插入必要的變數，透過ModelFactory API將影像模型實例化。 我們會使用Sling模型 **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 註解，在Sling模型實例化後取得此物件。

   `@PostConstruct` 非常有用，而且作用能力與建構子類似，不過，在類實例化並插入所有附加註釋的Java欄位後，會叫用它。 其他Sling模型註解會在Java類別欄位（變數）上加上註解， `@PostConstruct` 標注void，零參數方法，通常名為 `init()` （但可以命名任何名稱）。

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

   請記住，Sling模型為 **NOT** OSGi服務，因此維護類狀態是安全的。 經常 `@PostConstruct` 衍生並設定Sling Model類狀狀態以供日後使用，類似於純建構函式。

   請注意，若 `@PostConstruct` 方法會擲回例外狀況，Sling模型不會實例化（為null）。

1. **getImage()** 現在可更新，只要傳回影像物件即可。

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

   記下多個呼叫 `getImage()` 沒有問題，因為返回已初始化 `image` 類別變數和未叫用 `modelFactory.getModelFromWrappedRequest(...)` 這不是太貴，但值得避免不必要地打電話。

1. 最後 `BylineImpl.java` 看起來應該像這樣：


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


## 署名HTL {#byline-htl}

在 `ui.apps` 模組，開啟 `/apps/wknd/components/byline/byline.html` 我們在先前的AEM元件設定中建立。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

讓我們來檢視此HTL指令碼目前的功能：

* 此 `placeholderTemplate` 指向核心元件的預留位置，該預留位置會在元件未完全設定時顯示。 如上所定義，這會在AEM Sites頁面編輯器中呈現元件標題的方塊，如 `cq:Component`&#39;s  `jcr:title` 屬性。

* 此 `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 載入 `placeholderTemplate` 定義，並傳遞至布林值(目前硬式編碼為 `false`)放入預留位置範本中。 當 `isEmpty` 為true，預留位置範本會呈現灰色方塊，否則不會呈現任何內容。

### 更新署名HTL

1. 更新 **byline.html** 骨HTML結構：

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

   請注意，CSS類別會遵循 [BEM命名慣例](https://getbem.com/naming/). 雖然使用BEM慣例並非強制性，但建議使用BEM，因為BEM用於核心元件CSS類別，且通常會產生乾淨、可讀的CSS規則。

### 在HTL中實例化Sling模型物件 {#instantiating-sling-model-objects-in-htl}

此 [使用塊語句](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) 可用來實例化HTL指令碼中的Sling模型物件，並將其指派給HTL變數。

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 使用由BylineImpl實作的Byline介面(com.adobe.aem.guides.wknd.models.Byline)並調整目前的SlingHttpServletRequest，而結果會以位元組()儲存在HTL變數名稱中 `data-sly-use.<variable-name>`)。

1. 更新外部 `div` 以參考 **署名** Sling模型及其公用介面：

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### 存取Sling模型方法 {#accessing-sling-model-methods}

HTL從JSTL借用，並使用與縮短Java getter方法名稱相同的名稱。

例如，叫用署名Sling模型的 `getName()` 方法可縮短為 `byline.name`，類似而非 `byline.isEmpty`，這可以短接到 `byline.empty`. 使用完整方法名稱， `byline.getName` 或 `byline.isEmpty`，也行。 請注意 `()` 在HTL中絕不用於叫用方法（類似JSTL）。

需要參數的Java方法 **不能** 用於HTL。 這是為了讓HTL的邏輯保持簡單而設計。

1. 可借由叫用 `getName()` 方法（在署名Sling模型中或在HTL中）: `${byline.name}`.

   更新 `h2` 標籤：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用HTL運算式選項 {#using-htl-expression-options}

[HTL運算式選項](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) 在HTL中擔任內容的修飾元，範圍從日期格式到i18n翻譯。 運算式也可用來加入清單或值陣列，這是以逗號分隔格式顯示職業所需的項目。

運算式是透過 `@` 運算子。

1. 若要加入具有「 」、「 」的職業清單，請使用下列程式碼：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 有條件地顯示佔位符 {#conditionally-displaying-the-placeholder}

AEM元件的大部分HTL指令碼都採用 **預留位置範例** 為作者提供視覺提示 **指出元件撰寫不正確，且不會顯示在AEM發佈上**. 推動此決策的慣例是，在元件的支援Sling模型上實作方法，在本例中為： `Byline.isEmpty()`.

`isEmpty()` 會在Byline Sling模型上叫用，並透過 `!` 運算子)儲存至名為的HTL變數 `hasContent`:

1. 更新外部 `div` 儲存HTL變數，名稱為 `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   請注意 `data-sly-test`, HTL `test` 區塊的有趣之處在於，它會根據HTL運算式的結果是否為真，將HTL變數設為AND轉譯/不轉譯其所在的HTML元素。 如果為「truthy」，HTML元素會呈現，否則不會呈現。

   此HTL變數 `hasContent` 現在可重新用來有條件地顯示/隱藏預留位置。

1. 將條件呼叫更新至 `placeholderTemplate` 填入以下項目：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心元件顯示影像 {#using-the-core-components-image}

適用於 `byline.html` 現在幾乎已完成，且僅缺少影像。

由於我們使用 `sling:resourceSuperType` 核心元件影像元件提供影像的製作功能，我們也可以使用核心元件影像元件來轉譯影像！

為此，我們需要包含目前的署名資源，但需使用資源類型，強制使用核心元件影像元件的資源類型 `core/wcm/components/image/v2/image`. 這是元件重複使用的強大模式。 為此，HTL `data-sly-resource` 已使用區塊。

1. 取代 `div` 有 `cmp-byline__image` 並搭配下列項目：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   此 `data-sly-resource`，透過相對路徑包含目前資源 `'.'`，並強制將目前資源（或署名內容資源）包含在資源類型為 `core/wcm/components/image/v2/image`.

   核心元件資源類型是直接使用，而非透過Proxy，因為這是指令碼內的使用，且永遠不會持續保存在我們的內容中。

2. 已完成 `byline.html` 如下：

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

3. 將程式碼基底部署至本機AEM執行個體。 由於 `core` 和 `ui.apps` 兩個模組都需要部署。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   若部署至AEM 6.5/6.4，請叫用 `classic` 設定檔：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > 您也可以使用Maven設定檔，從根目錄建立整個專案 `autoInstallSinglePackage` 但這可能會覆寫頁面上的內容變更。 這是因為 `ui.content/src/main/content/META-INF/vault/filter.xml` 已針對教學課程入門程式碼修改，以徹底覆寫現有的AEM內容。 在現實情況中，這不是問題。

### 檢閱未設定樣式的署名元件 {#reviewing-the-unstyled-byline-component}

1. 部署更新後，請導覽至 [LA Skateparks終極指南 ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 頁面，或您在章節前面新增「署名」元件的位置。

1. 此 **影像**, **名稱**，和 **職業** 現在已顯示，而且我們有未設定樣式，但可使用Byline元件。

   ![未設定署名元件樣式](assets/custom-component/unstyled.png)

### 檢閱Sling模型註冊 {#reviewing-the-sling-model-registration}

此 [AEM Web Console的Sling模型狀態檢視](http://localhost:4502/system/console/status-slingmodels) 顯示AEM中所有已註冊的Sling模型。 您可以檢閱此清單，驗證是否已安裝並辨識Byline Sling模型。

若 **署名實施** 清單中未顯示，則Sling模型註解可能會發生問題，或Sling模型未新增至已註冊的Sling模型套件(`com.adobe.aem.guides.wknd.core.models`)。

![已註冊署名Sling模型](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## 署名樣式 {#byline-styles}

署名元件需要設定樣式，以與署名元件的創意設計一致。 這是透過使用SCSS來達成，AEM透過 **ui.frontend** Maven子項目。

### 添加預設樣式

為署名元件新增預設樣式。

1. 返回到IDE和 **ui.frontend** 項目 `/src/main/webpack/components`:
1. 建立名為 `_byline.scss`.

   ![署名專案總管](assets/custom-component/byline-style-project-explorer.png)

1. 將署名實作CSS（寫入為SCSS）新增至 `_byline.scss`:

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
1. 開啟終端機並導覽至 `ui.frontend` 模組。
1. 啟動 `watch` 使用下列npm命令處理：

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 返回瀏覽器並導覽至 [洛杉磯冰鞋公園文章](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 您應該會看到元件的更新樣式。

   ![已完成的署名元件](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 您可能需要清除瀏覽器快取，以確保不會提供過時的CSS，並使用Byline元件重新整理頁面，以取得完整樣式。

## 恭喜！ {#congratulations}

恭喜，您剛使用Adobe Experience Manager從頭建立自訂元件！

### 後續步驟 {#next-steps}

繼續了解AEM元件開發，探索如何針對Byline Java程式碼編寫JUnit測試，以確保所有項目都能正確開發，且實作的業務邏輯正確且完整。

* [編寫單元測試或AEM元件](unit-testing.md)

在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或是在本機的Git程式庫上檢閱並部署程式碼 `tutorial/custom-component-solution`.

1. 複製 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 查看 `tutorial/custom-component-solution` 分支
