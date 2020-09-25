---
title: 自訂元件
description: 涵蓋自訂逐行元件的端對端建立，以顯示撰寫的內容。 包括開發Sling Model以封裝商業邏輯以填入署名元件和對應的HTL來轉換元件。
sub-product: sites
feature: sling-models
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '4011'
ht-degree: 0%

---


# 自訂元件 {#custom-component}

本教學課程涵蓋自訂AEM Byline元件的端對端建立，以顯示在Dialog中編寫的內容，並探討開發Sling模型以封裝商業邏輯，以填入元件的HTL。

## 必備條件 {#prerequisites}

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。

### Starter Project

>[!NOTE]
>
> 如果您在本教學課程的前幾部分中也有相關內容，您會注意到本章的「入門專案」可加速實作。 它還包含一些範本和更多內容。 值得一提的是，除了自訂元件開發外，您還可以自由探索新內容和實施的其他方面。

查看教學課程所建立的基線程式碼：

1. 克隆github.com/adobe/aem-guides-wknd [儲存庫](https://github.com/adobe/aem-guides-wknd) 。
1. 查看分 `custom-component/start` 支

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout custom-component/start
   ```

1. 使用您的Maven技巧，將程式碼庫部署至本機AEM實例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

您隨時都可以在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd/tree/custom-component/solution) ，或切換至分支，在本機檢出程式碼 `custom-component/solution`。

## 目標

1. 瞭解如何建立自訂AEM元件
1. 瞭解如何使用Sling Models封裝商業邏輯
1. 瞭解如何從HTL指令碼內使用Sling Model

## 您將建立的 {#byline-component}

>[!VIDEO](https://video.tv.adobe.com/v/30181/?quality=12&learn=on)

在WKND教學課程的這部分，會建立「署名元件」，用來顯示文章投稿者的撰寫資訊。

![byline元件示例](./assets/custom-component/byline-design.png)

*由WKND設計團隊提供的署名元件視覺設計*

Byline元件的實作包含收集署名內容的對話方塊，以及擷取署名的自訂Sling Model:

* 名稱
* 影像
* 職業

顯示，則會轉譯瀏覽器最終顯示的HTML。

![位行去構成](./assets/custom-component/byline-decomposition.png)

*逐行分解*

## 建立署名元件 {#create-byline-component}

首先，建立「同行元件」節點結構並定義對話框。 這代表AEM中的元件，並依其在JCR中的位置隱式定義元件的資源類型。

對話方塊會顯示內容作者可提供的介面。 在此實作中，AEM WCM Core Component的 **Image** （影像）元件將用來製作和轉換Byline的影像，因此會設為我們的元件 `sling:resourceSuperType`。

### 建立元件節點 {#create-component-node}

1. 在 **ui.apps模組中** ，導覽至並 `/apps/wknd/components/content` 建立名為類型位 **元的新節點**`cq:Component`。

   ![對話框，建立節點](./assets/custom-component/byline-node-creation.png)

1. 將下列屬性添加到Byline元件的節 `cq:Component` 點。

   ```plain
   jcr:title = Byline
   jcr:description = Displays a contributor's byline.
   componentGroup = WKND.Content
   sling:resourceSuperType =  core/wcm/components/image/v2/image
   ```

   ![Byline元件屬性](./assets/custom-component/byline-component-properties.png)

   產生此 `.content.xml` XML的原因：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="https://sling.apache.org/jcr/sling/1.0" xmlns:jcr="https://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND.Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

### 建立HTL指令碼 {#create-the-htl-script}

1. 在節 `byline` 點下方，新增負 `byline.html`責元件HTML呈現的新檔案。 將檔案命名為與節點相同 `cq:Component` 的檔案很重要，因為它會變成Sling將用來呈現此資源類型的預設指令碼。

1. 將下列程式碼新增至 `byline.html`。

   ```xml
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` 在 [Sling Model建立後](#byline-htl)，就會重新檢視。 HTL檔案的目前狀態可讓元件在拖放至頁面時，以空白狀態顯示在AEM Sites的「頁面編輯器」中。

### 建立對話框定義 {#create-the-dialog-definition}

接著，為Byline元件定義一個對話框，其中包含以下欄位：

* **名稱**:文字欄位，作為投稿者的名稱。
* **影像**:這是對投稿人簡歷的參考。
* **職業**:一份由貢獻者貢獻的職業清單。 職業應依字母順序遞增（a至z）排序。

1. 在元件節 `byline` 點下面建立一個名為類型 `cq:dialog` 的新節點 `nt:unstructured`。
1. 使用下 `cq:dialog` 列XML更新。 您最容易開啟並復 `.content.xml` 制／貼上下列XML。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
           xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
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

   這些節點定義使用 [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) ，以控制哪些對話方塊標籤繼承自元件，在本例中為 `sling:resourceSuperType` Core Components的Image元件 ****。

   ![已完成的逐行對話方塊](./assets/custom-component/byline-dialog-created.png)

### 建立策略對話框 {#create-the-policy-dialog}

按照與建立對話框相同的方法，建立策略對話框（以前稱為設計對話框）以隱藏從核心元件映像元件繼承的策略配置中不需要的欄位。

1. 在節點 `byline` 下方，創 `cq:Component` 建名為類型的 `cq:design_dialog` 新節點 `nt:unstructured`。
1. 使用下 `cq:design_dialog` 列XML更新。 您最容易開啟並復 `.content.xml` 制／貼上下方的XML。

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

   前面「策略」對 **話框** XML的基礎是從「核心元件 [映像」元件獲取的](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)。

   就像在Dialog設定中， [Sling Resource Merger](https://sling.apache.org/documentation/bundles/resource-merger.html) ( `sling:resourceSuperType`Sling Resource Merger `sling:hideResource="{Boolean}true"` )可用來隱藏從中繼承的不相關欄位，如節點定義與屬性所示。

### 部署程式碼 {#deploy-the-code}

1. 使用您的Maven技巧，將更新的程式碼庫部署至本機AEM實例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallPackage
   ```

### 將元件新增至頁面 {#add-the-component-to-a-page}

為了簡化工作並專注於AEM元件開發，我們會將目前狀態的Byline元件新增至「文章」頁面，以確認節點定義已部署且正確無誤，AEM會辨識新元件定義，而元件的對話方塊也適用於編寫。 `cq:Component`

由於我 [們通過屬性將Byline元件添加到 **WKND.Content** Component Group](#create-component-node)，所以它可自動用於任何Container `/apps/wknd/components/content/byline@componentGroup=WKND.Content` ，其LayoutContainer允許Article Article的Wknd. ************ Content Group的Article的LayoutContainerGroup的元件。

#### 將元件拖放至頁面 {#drag-and-drop-the-component-onto-the-page}

1. **在** 「 **AEM >網站> WKND網站>語言管理員>英文>雜誌> LA Skateparks旗艦指南」編輯文章頁面**。
1. 從左側列，將 **Byline元件拖放至已開** 啟文章頁面 **** 的「版面容器」底部。

   ![將byline元件新增至頁面](assets/custom-component/add-to-page.png)

#### 編寫元件 {#author-the-component}

AEM作者會透過對話方塊設定和編寫元件。 此時，在開發Byline元件時，會包含對話方塊以收集資料，但是尚未新增轉換製作內容的邏輯。

1. 確保左側 **邊欄已開啟** ，且已選取 **資產搜尋器** 。

   ![開啟資產搜尋器](assets/custom-component/open-asset-finder.png)

1. 選取「 **Byline」元件預留位置**，接著會顯示動作列並點選扳 **手圖示** ，以開啟對話方塊。

   ![元件操作欄](assets/custom-component/action-bar.png)

1. 當對話方塊開啟，且第一個標籤（資產）啟用時，開啟左側邊欄，並從資產搜尋器將影像拖曳至「影像」下拉區。 搜尋「stacey」，尋找WKND ui.content套件中提供的Stacey Roswells簡歷圖片。

   **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**

   ![將影像加入對話方塊](assets/custom-component/add-image.png)

1. 新增影像後，按一下「屬 **性** 」標籤以輸入「名 **稱** 」和「職 **業」**。

   在進入職業時，請以反 **向字母順序** ，如此我們將在Sling Model中實作的按字母順序排列的商業邏輯就顯而易見了。

   點選右 **下方** 的「完成」按鈕以儲存變更。

   ![行元件填充屬性](assets/custom-component/add-properties.png)

1. 儲存對話方塊後，請導覽至 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline) ，並檢視元件的內容如何儲存在AEM頁面下的位元件內容節點上。

   在節點（如「Byline」）下查找Byline `jcr:content/root/responsivegrid/responsivegrid` 元件內容節點 `/content/wknd/language-masters/en/magazine/guide-la-skateparks/jcr:content/root/responsivegrid/responsivegrid/byline`。

   請注意，屬性 `name`名 `occupations`稱、 `fileReference` 和儲存在位 **元節點上**。

   此外，請注意， `sling:resourceType` 節點的設定是將此內 `wknd/components/content/byline` 容節點綁定到Byline元件實施的原因。

   ![CRXDE中的byline屬性](assets/custom-component/byline-properties-crxde.png)

   */content/wknd/language-masters/tw/magazine/guide-la-skateparks/jcr:content/root/roost/responsivegrid/responsivegrid/byline*

## Create Byline Sling Model {#create-sling-model}

接下來，我們將建立Sling Model，做為資料模型，並代管Byline元件的商業邏輯。

Sling Models是註解導向的Java &quot;POJO&#39;s&quot;(Plain Old Java Objects)，可協助將資料從JCR對應至Java變數，並在AEM中進行開發時提供許多其他細節。

### 查看Maven依賴項 {#maven-dependency}

Byline Sling Model將仰賴AEM提供的數個Java API。 這些API可通過模 `dependencies` 塊的POM文 `core` 件中列出。

1. 在下方開 `pom.xml` 啟檔案 `<src>/aem-guides-wknd/core/pom.xml`。
1. 在pom檔案的「相 `uber-jar` 關性」部分中查找對的相關性：

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   The [uber-jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) contains all public Java API by AEM ovessed by AEM. 請注意，檔案中未指定版 `core/pom.xml` 本。 版本會改為保留在位於項目根部的Parent反應器pom中 `aem-guides-wknd/pom.xml`。

1. 查找以下項的相依 `core.wcm.components.core`性：

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   這是AEM核心元件公開的所有公開Java API。 AEM Core Components是AEM外部維護的專案，因此有個別的發行週期。 因此，它是需要單獨包含的依賴項， **而不** 是Uber-jar。

   與uber-jar一樣，此相依性的版本會保存在位於的Parent reactor pom檔案中 `aem-guides-wknd/pom.xml`。

   在本教學課程的後面，我們將使用「核心元件影像」類別，在Byline元件中顯示影像。 必須具備核心元件相依性才能建立並編譯我們的Sling Model。

### 署名介面 {#byline-interface}

為署名建立公用Java介面。 `Byline.java` 定義驅動HTL指令碼所需的公 `byline.html` 用方法。

1. 在模 `aem-guides-wknd.core` 塊中， `src/main/java,` 按一下右鍵「包」>「新建」>「介面」 `Byline.java` ，建立一個名為的新Java `com.adobe.aem.guides.wknd.core.models` 介面 ****。 輸入 **Byline** 作為介面名稱，然後按一下完成。

   ![建立行文介面](assets/custom-component/create-byline-interface.png)

1. 使用 `Byline.java` 下列方法更新：

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

   前兩種方法會公開Byline元 **件****的名** 稱和職業值。

   該方 `isEmpty()` 法用於確定元件是否具有要渲染的內容或元件是否等待配置。

   請注意，影像沒有任何方法； [我們稍後會看看為什麼](#tackling-the-image-problem)。

### 署名實作 {#byline-implementation}

`BylineImpl.java` 是實作Sling Model的實作，實作先前定義 `Byline.java` 的介面。 您可在本節 `BylineImpl.java` 底部找到完整的程式碼。

1. 在下方 `core` 的模 `src/main/java`組中，以滑鼠右鍵按一下套件並選取「新增」>「類別」，建立名為 **BylineImpl.java** 的新類別檔案 `com.adobe.aem.guides.wknd.core.models.impl`****。

   為名稱輸入 **BylineImpl**。 將 **Byline介面新增為實作介面** 。

   ![建立行動實作](assets/custom-component/create-byline-impl.png)

1. 開啟 `BylineImpl.java`. 它會自動填入介面中定義的所有方法 `Byline.java`。 使用下列類別層級的註解更 `BylineImpl.java` 新，以新增Sling Model註解。 這 `@Model(..)`個註解會將類別轉換為Sling Model。

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
       ...
   }
   ```

   讓我們回顧一下此注釋及其參數：

   * 注 `@Model` 釋將BylineImpl部署至AEM時註冊為Sling Model。
   * 參 `adaptables` 數指定此模型可由請求調整。
   * 該參 `adapters` 數允許在Byline介面下註冊實現類。 這可讓HTL指令碼透過介面呼叫Sling Model（而非直接執行）。 [有關適配器的詳細資訊，請參閱此處](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)。
   * 該 `resourceType` 指向Byline元件資源類型（先前建立的），如果存在多個實現，則有助於解析正確的模型。 [有關將模型類與資源類型關聯的詳細資訊，請參閱此處](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)。

### 實作Sling Model方法 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

我們要解決的第一個方法 `getName()` 是，它只會傳回儲存至位元的JCR內容節點，位於屬性下方 `name`。

對於此， `@ValueMapValue` Sling Model註解會用來使用Request資源的ValueMap，將值插入Java欄位。

```java
...
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
...
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

由於JCR屬性與Java欄位（兩者皆為&quot;name&quot;）具有相同的名稱， `@ValueMapValue` 因此會自動解析此關聯，並將屬性的值插入Java欄位。

#### getSchorips() {#implementing-get-occupations}

下一個要實現的方法是 `getOccupations()`。 此方法會收集JCR屬性中儲存的所有職業， `occupations` 並傳回已排序（依字母順序）的職業集合。

使用屬性值中探 `getName()` 索的相同技術，可將其注入Sling Model欄位。

一旦JCR屬性值可透過插入的Java欄位在Sling Model中提供 `occupations`，排序商業邏輯就可套用在方 `getOccupations()` 法中。

```java
import java.util.ArrayList;
import java.util.Collections;
...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
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

最後一個公用方法會 `isEmpty()` 決定元件何時應將自身視為「已編寫足夠」以呈現。

對於此元件，我們有業務要求，指出必須填寫所有三個欄位、名稱、影像和職 *業* ，才能生成元件。

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

檢查名稱和佔用條件很瑣碎(而Apache Commons Lang3提供隨時都方便使用的 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html)**** 類)，但是，由於使用核心元件映像元件來呈現映像，因此不清楚如何驗證映像的存在。

解決這個問題的方法有兩種：

1. 檢查JCR屬 `fileReference` 性是否解析為資產。
1. 將此資源轉換為Core Component Image Sling Model，並確 `getSrc()` 保方法不為空。

   我們將選擇第二 **種方** 法。 第一種方法可能已足夠，但在本教學課程中，後一種方法將被用來讓我們探索Sling Models的其他功能。

1. 建立取得影像的私用方法。 此方法保留為私密，因為我們不需要在HTL本身中公開Image物件，而且它只用於驅動 `isEmpty().`

   下列專用方法 `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，還有兩種方法可取得 **Image Sling Model**:

   第一個使用注 `@Self` 釋，以自動將當前請求調整為「核心元件」的 `Image.class`

   ```java
   @Self
   private Image image;
   ```

   第二種是使用 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服務，這是一項非常方便的服務，並協助我們以Java程式碼建立其他類型的Sling Models。

   我們將選擇第二種方法。

   >[!NOTE]
   >
   >在實際實作中，偏好使用「一」的方 `@Self` 法，因為它是更簡單、更精美的解決方案。 在本教學課程中，我們將使用第二種方法，因為它要求我們探索更多Sling Models的小面，這些小面對更複雜的元件非常有用！

   由於Sling Models是Java POJO的，而非OSGi Services，所以通常的OSGi注入註解 `@Reference` 不 **能使用** ，而Sling Models會提供特殊 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** annotation，提供類似的功能。

1. 更新 `BylineImpl.java` 以包含注 `OSGiService` 釋以插入 `ModelFactory`:

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

   有了可 `ModelFactory` 用項目，您可使用下列方式建立Core Component Image Sling Model:

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   不過，此方法需要請求和資源，但Sling Model中目前都不提供。 若要取得這些，請使用更多Sling Model註解！

   要獲取當前請求， **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** annotation可用於將 `adaptable` （在「為」中定義）注入Java類 `@Model(..)` 字 `SlingHttpServletRequest.class`段中。

1. 新增 **@Self** annotation以取得 **SlingHttpServletRequest請求**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   請記住， `@Self Image image``@Self` 使用注入核心元件影像Sling Model是上述選項——註解會嘗試注入可調整的物件（在我們的例子中為SlingHttpServletRequest），並調整為注釋欄位類型。 由於Core Component Image Sling Model是可從SlingHttpServletRequest物件調整的，所以這會奏效，而且比我們更探索性的方式少程式碼。

   現在，我們已注入必要的變數，以透過ModelFactory API實例化我們的影像模型。 我們將在Sling Model實例化後，使 **[用Sling Model的@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** annotation來取得這個物件。

   `@PostConstruct` 的功能非常有用，而且其作用能力與建構函式類似，但是，在類別執行個體化並插入所有註解的Java欄位後，就會呼叫它。 其他Sling Model註解會註解Java類別欄位（變數）, `@PostConstruct` 但註解void, zero parameter方法，通常會命名 `init()` （但可以命名任何項目）。

1. 添加 **@PostConstruct方法** :

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

   請記住，Sling Models是 **NOT** OSGi Services，因此維護類別狀態是安全的。 通常 `@PostConstruct` 會衍生並設定Sling Model類別狀態以供日後使用，類似於一般的建構函式。

   請注意，如果 `@PostConstruct` 方法引發例外，Sling Model將不會執行個體化（它將為null）。

1. **getImage()現在可** 以更新，只要傳回影像物件即可。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 讓我們回到實 `isEmpty()` 作並完成：

   ```java
   @Override
   public boolean isEmpty() {
       ...
       } else if (getImage() == null || StringUtils.isBlank(getImage().getSrc())) {
           // A valid image is required
           return true;
       } else {
       ...
   }
   ```

   請注意，多次呼 `getImage()` 叫不會有問題，因為傳回已初始化的類 `image` 別變數，而且不會叫用 `modelFactory.getModelFromWrappedRequest(...)` 代價不高但值得避免不必要的呼叫。

1. 最終 `BylineImpl.java` 結果如下：

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       @PostConstruct
       private void init() {
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
           final Image image = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (image == null || StringUtils.isBlank(image.getSrc())) {
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

在模 `ui.apps` 組中，開 `/apps/wknd/components/content/byline/byline.html` 啟我們先前設定的AEM元件中建立的。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

讓我們來回顧一下此HTL指令碼目前的運作方式：

* 指 `placeholderTemplate` 向「核心元件」的預留位置，此預留位置會在元件未完全設定時顯示。 如上所述，在AEM Sites Page Editor中，這會呈現為具有元件標題的方 `cq:Component`塊( `jcr:title` Property)。

* 載 `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 入上述定 `placeholderTemplate` 義並以布林值(目前硬式編碼為 `false`)傳遞至預留位置範本。 當 `isEmpty` 為true時，預留位置範本會呈現灰色方塊，否則不會呈現任何內容。

### 更新署名HTL

1. 使用 **下列骨架HTML結構** ，更新byline.html:

   ```xml
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!-- Include the Core Components Image Component -->
           </div>
           <h2 class="cmp-byline__name"><!-- Include the name --></h2>
           <p class="cmp-byline__occupations"><!-- Include the occupations --></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   請注意，CSS類遵循 [BEM命名慣例](https://getbem.com/naming/)。 雖然使用BEM慣例並非強制性，但建議使用BEM，因為它用於核心元件CSS類，通常會產生簡潔、可讀的CSS規則。

#### 在HTL中執行個體化Sling Model物件 {#instantiating-sling-model-objects-in-htl}

Use [block陳述式](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) ，可用來在HTL指令碼中執行個體化Sling Model物件，並將它指派給HTL變數。

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 使用BylineImpl實作的Byline介面(com.adobe.aem.guides.wknd.models.Byline)，並將目前的SlingHttpServletRequest套用至它，而結果會儲存在HTL變數名稱的位元( `data-sly-use.<variable-name>`)中。

1. 更新外部 `div` 以參照 **Byline** Sling Model（依其公用介面）:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

#### 存取Sling Model方法 {#accessing-sling-model-methods}

HTL從JSTL借用，並使用相同的縮短Java getter方法名稱。

例如，叫用Byline Sling Model的方 `getName()` 法可縮短為 `byline.name`，而 `byline.isEmpty`非縮短為 `byline.empty`。 使用完整方法名 `byline.getName` 稱 `byline.isEmpty`或，同樣適用。 請注意， `()` 在HTL中，不會使用來叫用方法（類似於JSTL）。

需要參數的Java方 **法** ，無法用於HTL。 這是為了讓HTL的邏輯保持簡單。

1. Byline name can be added to the component by accounted the method on `getName()` the Byline Sling Model, or in HTL: `${byline.name}`.

   更新標 `h2` 記：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

#### 使用HTL運算式選項 {#using-htl-expression-options}

[「HTL運算式選項](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) 」在HTL中做為內容的修飾元，範圍從日期格式到i18n轉換。 運算式也可用來連結清單或值陣列，這是以逗號分隔格式顯示職業的必要項。

運算式會透過HTL運 `@` 算式中的運算子新增。

1. 要使用&quot;, &quot;加入職業清單，請使用以下代碼：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

#### 有條件地顯示預留位置 {#conditionally-displaying-the-placeholder}

AEM Components的大部分HTL指令碼都採用預留 **位置範例** ，為作者提供視覺提示， **指出元件的製作不正確，不會顯示在AEM Publish**。 推動此決定的慣例是在元件支援Sling Model上實作方法，在我們的案例中： `Byline.isEmpty()`.

`isEmpty()` 在Byline Sling Model上呼叫，結果（或其負值，透過運算子）會儲 `!` 存至名為 `hasContent`:

1. 更新外部 `div` 以儲存名為 `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   請注意， `data-sly-test``test` HTL區塊的使用很有趣，因為它會根據HTL運算式的結果是否真實，設定HTL變數AND轉譯／不轉譯其所在的HTML元素。 如果是&quot;truthy&quot;,HTML元素會轉譯，否則不會轉譯。

   現在，此HTL `hasContent` 變數可重新用於有條件地顯示／隱藏預留位置。

1. 使用下列項目，將條件 `placeholderTemplate` 性呼叫更新至檔案底部：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

#### 使用核心元件顯示影像 {#using-the-core-components-image}

HTL指令碼現 `byline.html` 在幾乎已完成，只會遺失影像。

```html
<!--/* current progress of byline.html */-->
<div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!byline.empty}"
     class="cmp-byline">
    <div class="cmp-byline__image">
        <!-- Include the Core Components Image component -->
    </div>
    <h2 class="cmp-byline__name">${byline.name}</h2>
    <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
```

由於我們使 `sling:resourceSuperType` 用核心元件影像元件來製作影像，因此我們也可以使用核心元件影像元件來轉換影像！

為此，我們需要包括當前的行資源，但使用資源類型強制使用核心元件映像元件的資源類型 `core/wcm/components/image/v2/image`。 這是強大的模式，可供元件重複使用。 為此，使用HTL `data-sly-resource` 區塊。

1. 以 `div` 類別取代 `cmp-byline__image` 為：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   這 `data-sly-resource`會透過相對路徑包含目前資源 `'.'`，並強制將目前資源（或署名內容資源）與資源類型一起包含 `core/wcm/components/image/v2/image`。

   核心元件資源類型是直接使用，而非透過Proxy使用，因為這是指令碼內使用，而且不會持續存在於我們的內容中。

2. 完成 `byline.html` 如下：

   ```html
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
            data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
           <h2 class="cmp-byline__name">${byline.name}</h2>
           <p class="cmp-byline__occupations">${byline.occupations @ join=','}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. 將程式碼庫部署至本機AEM例項。 由於對POM檔案進行了重大更改，因此從項目的根目錄執行完整的Maven生成。

   >[!WARNING]
   >
   > 請注意，WKND專案的設定會覆寫 `ui.content``ui.apps` JCR中的任何變更，因此，我們希望確保我們只部署專案，以避免抹去先前新增至文章頁面的Byline元件。

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.apps
   $ mvn -PautoInstallPackage clean install
   ...
   Package imported.
   Package installed in 338ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

#### 檢視未設定樣式的Byline元件 {#reviewing-the-unstyled-byline-component}

1. 部署更新後，請導覽至「LA Skateparks的 [Ultimate Guide」（終極指南）頁 ](http://localhost:4502/editor.html/content/wknd/language-masters/en/magazine/guide-la-skateparks.html) 面，或您在章節中稍早新增Byline元件的位置。

1. 現 **在出現**&#x200B;影像 **、姓名和職**&#x200B;業 **** ，而且我們有一個未設定樣式的Byline元件。

   ![非樣式的署名元件](assets/custom-component/unstyled.png)

#### 檢視Sling Model註冊 {#reviewing-the-sling-model-registration}

[AEM Web Console的Sling Models Status檢視會顯示](http://localhost:4502/system/console/status-slingmodels) AEM中所有已註冊的Sling Models。 Byline Sling Model可以透過檢閱此清單來驗證是否已安裝及識別。

如果 **BylineImpl** 未顯示在此清單中，則Sling Model的註解可能會發生問題，或Sling Model未新增至核心專案中已註冊的Sling Models套件(com.adobe.aem.guides.wknd.core.models)。

![已註冊Byline Sling Model](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## 署名樣式 {#byline-styles}

Byline元件需要設定樣式，以符合Byline元件的創意設計。 這將透過使用SCSS來達成，AEM透過 **ui.frontend** Maven子專案提供支援。

在設定樣式後，Byline元件應採用依循美學。

![署名模擬樣式](./assets/custom-component/byline-design.png)

*由WKND創意團隊定義的署名元件設計*

### 新增預設樣式

為Byline元件新增預設樣式。 在 **ui.frontend專案中** ，位於 `/src/main/webpack/components/content`:

1. 建立名為的新資料夾 `byline`。
1. 在名為的資料夾下建立新 `byline` 資料夾 `scss`。
1. 在名為的資料夾下 `byline/scss` 建立新檔案 `byline.scss`。
1. 在名為的資料夾下建立新 `byline/scss` 資料夾 `styles`。
1. 在名為的資料夾下 `byline/scss/styles` 建立新檔案 `default.scss`。

   ![byline project explorer](assets/custom-component/byline-style-project-explorer.png)

1. 首先，填 **入byline.scss** ，加入預設樣式：

   ```scss
    /* WKND Byline styles */
   @import 'styles/default';
   ```

1. 將Byline實作CSS（寫入為SCSS）新增至 `default.scss`:

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
           font-size: $font-size-large;
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

1. 在UI. `main.scss` frontend專案的 **下方開啟檔案** ，並 `/src/main/webpack/site` 在區段中新增下列 `/* Components */` 行：

   ```scss
   @import '../components/content/byline/scss/byline.scss';
   ```

1. 使用NPM構建和編 `ui.frontend` 譯模組：

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend
    $ npm run dev
   ```

1. 使用Maven，建立 `ui.apps` 並部署專案(將會轉譯包 `ui.frontend` 含專案)至本機AEM例項：

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.apps
    $ mvn clean install -PautoInstallPackage
   ```

   >[!TIP]
   >
   >您可能需要清除瀏覽器快取以確保不提供過時的CSS，並使用Byline元件重新整理頁面，以取得完整樣式。

## 整合 {#putting-it-together}

以下是完整編寫和樣式化的Byline元件在AEM頁面上的外觀。

![完成的位元件](assets/custom-component/final-byline-component.png)

觀看以下影片，以快速瞭解本教學課程中的內建內容。

>[!VIDEO](https://video.tv.adobe.com/v/30174/?quality=12&learn=on)

## 恭喜！ {#congratulations}

恭喜您，您剛使用Adobe Experience Manager從頭開始建立自訂元件！

### 後續步驟 {#next-steps}

繼續瞭解AEM元件開發，探索如何編寫Byline Java程式碼的JUnit測試，以確保一切正確開發，並實作的商業邏輯正確且完整。

* [編寫單元測試或AEM元件](unit-testing.md)

在 [GitHub上檢視完成的程式碼](https://github.com/adobe/aem-guides-wknd) ，或在Git筆刷上在本機檢視並部署程式碼 `custom-component/solution`。

1. 克隆github.com/adobe/aem-guides-wknd [儲存庫](https://github.com/adobe/aem-guides-wknd) 。
1. 查看分 `custom-component/solution` 支

## 疑難排解 {#troubleshooting}

### 缺少源資料夾

如果您在Eclipse中未看 `src/main/java` 到源資料夾，可以通過按一下右鍵src並添加主資料夾和java資料夾來添加資料夾。 添加資料夾後，您應看到包 `src/main/java` 出現。

### 未解析的包

![疑難排解的軟體包](assets/custom-component/troubleshoot-unresolved-packages.png)

>[!NOTE]
>
> 如果您有未解析的套件匯入，以匯入核心專案中新增的某些新相依項，請嘗試更新aem-guides-wknd maven專案，如此會更新所有子專案。 您可以按一下滑鼠右鍵 **>「Maven >更新專案」來執行此動作**。
