---
title: 自訂元件
description: 涵蓋顯示創作內容的自定義行元件的端到端建立。 包括開發Sling模型以封裝業務邏輯以填充該並行元件和相應的HTL以呈現該元件。
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
source-git-commit: 434f56e143bc0f969723de48abd26d49a308af9b
workflow-type: tm+mt
source-wordcount: '4061'
ht-degree: 0%

---

# 自訂元件 {#custom-component}

本教程介紹定製的端到端建立 `Byline` 顯AEM示在對話框中創作的內容的元件，並探索開發Sling模型以封裝填充元件HTL的業務邏輯。

## 必備條件 {#prerequisites}

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。

### 入門項目

>[!NOTE]
>
> 如果成功完成了上一章，則可以重新使用項目，並跳過簽出起始項目的步驟。

檢查本教程基於的基線代碼：

1. 查看 `tutorial/custom-component-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
   ```

1. 使用Maven技能將代碼AEM庫部署到本地實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使AEM用6.5或6.4，則追加 `classic` 配置檔案。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始終可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) 或通過切換到分支本地檢出代碼 `tutorial/custom-component-solution`。

## 目標

1. 瞭解如何構建自定義組AEM件
1. 學習用Sling模型封裝業務邏輯
1. 瞭解如何從HTL指令碼內使用Sling模型

## 您要構建的 {#what-build}

在WKND教程的這一部分中，將建立一個Byline元件，用於顯示有關文章參與者的創作資訊。

![byline元件示例](assets/custom-component/byline-design.png)

*Byline元件*

Byline元件的實現包括收集該Byline內容的對話框和檢索詳細資訊的自定義Sling模型，如：

* 名稱
* 影像
* 職業

## 建立Byline元件 {#create-byline-component}

首先，建立「行元件」節點結構並定義對話框。 這表示中的組AEM件，並通過元件在JCR中的位置隱式定義元件的資源類型。

該對話框顯示內容作者可以提供的介面。 為此實施，AEM WCM核心元件 **影像** 元件用於處理Byline影像的創作和渲染，因此必須將其設定為此元件的 `sling:resourceSuperType`。

### 建立元件定義 {#create-component-definition}

1. 在 **ui.apps** 模組，導航 `/apps/wknd/components` 建立名為 `byline`。
1. 在 `byline` 資料夾，添加名為 `.content.xml`

   ![對話框建立節點](assets/custom-component/byline-node-creation.png)

1. 填充 `.content.xml` 檔案，如下所示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上述XML檔案提供了元件的定義，包括標題、說明和組。 的 `sling:resourceSuperType` 指向 `core/wcm/components/image/v2/image`，即 [核心影像元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html)。

### 建立HTL指令碼 {#create-the-htl-script}

1. 在 `byline` 資料夾，添加檔案 `byline.html`，負責元件的HTML演示。 將檔案命名為與資料夾相同的檔案非常重要，因為該檔案將成為Sling用於呈現此資源類型的預設指令碼。

1. 將以下代碼添加到 `byline.html`。

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

的 `byline.html` 是 [後](#byline-htl)，建立「吊具模型」後。 HTL檔案的當前狀態允許元件在拖放到頁面上時以空狀態顯示在AEM站點的頁面編輯器中。

### 建立對話框定義 {#create-the-dialog-definition}

接下來，為Byline元件定義一個對話框，其中包含以下欄位：

* **名稱**:一個文本欄位，該欄位是參與者的名稱。
* **影像**:參考作者的生物照片。
* **職業**:一份由貢獻者貢獻的職業清單。 職業應按升序（a至z）按字母順序排序。

1. 在 `byline` 資料夾，建立名為 `_cq_dialog`。
1. 在 `byline/_cq_dialog`，添加名為 `.content.xml`。 這是對話框的XML定義。 添加以下XML:

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

   這些對話框節點定義使用 [Sling資源合併](https://sling.apache.org/documentation/bundles/resource-merger.html) 控制從中繼承的對話框頁籤 `sling:resourceSuperType` 元件，在本例中 **核心元件的映像元件**。

   ![已完成的按行對話框](assets/custom-component/byline-dialog-created.png)

### 建立策略對話框 {#create-the-policy-dialog}

按照與建立對話框相同的方法，建立一個策略對話框（以前稱為「設計對話框」），以隱藏從核心元件的映像元件繼承的策略配置中不需要的欄位。

1. 在 `byline` 資料夾，建立名為 `_cq_design_dialog`。
1. 在 `byline/_cq_design_dialog`，建立名為 `.content.xml`。 使用以下命令更新檔案：以下XML。 最容易開啟 `.content.xml` 將下面的XML複製/貼上到其中。

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

   前一條的依據 **策略對話** 從 [核心元件映像元件](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)。

   就像對話框配置中， [Sling資源合併](https://sling.apache.org/documentation/bundles/resource-merger.html) 用於隱藏從 `sling:resourceSuperType`，如節點定義所示 `sling:hideResource="{Boolean}true"` 屬性。

### 部署代碼 {#deploy-the-code}

1. 同步中的更改 `ui.apps` 或使用Maven技能。

   ![導出到服AEM務器行元件](assets/custom-component/export-byline-component-aem.png)

## 將元件添加到頁面 {#add-the-component-to-a-page}

為了使事情變得簡單並AEM專注於元件開發，讓我們將處於當前狀態的Byline元件添加到「文章」頁面以驗證 `cq:Component` 節點定義正確。 另外，驗證AEM是否識別新元件定義，並且元件的對話框可用於創作。

### 將影像添加到AEM Assets

首先，將樣本頭照片上載到AEM Assets，以用於填充Byline元件中的影像。

1. 導航到AEM Assets的LA Staleparks資料夾： [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks)。

1. 上傳頭部槍  **[斯泰西·羅斯威爾斯.jpg](assets/custom-component/stacey-roswells.jpg)** 到D3

   ![頭照上傳到AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### 編寫元件 {#author-the-component}

接下來，將Byline元件添加到中的頁面AEM。 因為Byline元件已添加到 **WKND站點項目 — 內容** 元件組，通過 `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 定義，它可自動用於任何 **容器** 誰 **策略** 允許 **WKND站點項目 — 內容** 元件組。 因此，它可在文章頁的佈局容器中找到。

1. 請瀏覽以下LA Stalepark文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 從左側欄中拖放 **Byline元件** 上 **底** 的子菜單。

   ![將行元件添加到頁面](assets/custom-component/add-to-page.png)

1. 確保左邊欄處於開啟狀態&#x200B;**可見，**&#x200B;已選擇資產查找器**。

1. 選擇 **行元件佔位符**，這樣，將顯示操作欄並點擊 **扳** 表徵圖開啟對話框。

1. 開啟對話框並激活第一個頁籤（資產）後，開啟左邊欄，然後從資產查找器將影像拖動到「影像」下拉區域。 搜索「stacey」以查找WKND ui.content包中提供的Stacey Roswells生物圖片。

   ![將影像添加到對話框](assets/custom-component/add-image.png)

1. 添加影像後，按一下 **屬性** 的 **名稱** 和 **職業**。

   進入職業時，輸入 **逆字母** 對Sling模型中實現的業務邏輯進行字母化驗證。

   點擊 **完成** 按鈕。

   ![填充行元件的屬性](assets/custom-component/add-properties.png)

   作AEM者通過對話框配置和編寫元件。 此時，在Byline元件的開發中包括用於收集資料的對話，但呈現創作內容的邏輯尚未添加。 因此，只顯示佔位符。

1. 保存對話框後，導航到 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) 並查看元件內容如何儲存在頁面下的byline元件內容節AEM點上。

   在「LA Skate Parks」(LA Skate Parks)頁面下查找Byline元件內容節點，即 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`。

   注意屬性名稱 `name`。 `occupations`, `fileReference` 儲存在 **位元組節點**。

   另外，請注意 `sling:resourceType` 的 `wknd/components/content/byline` 將此內容節點綁定到Byline元件實現。

   ![CRXDE中的行屬性](assets/custom-component/byline-properties-crxde.png)

## 建立Byline Sling模型 {#create-sling-model}

接下來，我們建立一個Sling模型來充當資料模型並容納Byline元件的業務邏輯。

Sling模型是注釋驅動的Java™ POJOs(Plain Old Java™ Objects)，可方便將資料從JCR映射到Java™變數，並在上下文中開發時提供AEM效率。

### 查看Maven依賴項 {#maven-dependency}

Byline Sling模型依賴於提供的幾個Java™ APIAEM。 通過 `dependencies` 在 `core` 模組的POM檔案。 本教程使用的項目是為AEMas a Cloud Service構建的。 但它是獨特的，因為它向後相容AEM6.5/6.4。因此，包括了Cloud ServiceAEM和6.x的依賴項。

1. 開啟 `pom.xml` 檔案 `<src>/aem-guides-wknd/core/pom.xml`。
1. 查找的依賴項 `aem-sdk-api` - **僅AEMas a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   的 [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) 包含由公開的所有公共Java™ APIAEM。 的 `aem-sdk-api` 在生成此項目時預設使用。 版本從項目的根部在Parent反應器pom中維護 `aem-guides-wknd/pom.xml`。

1. 查找的依賴關係 `uber-jar` - **僅AEM6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   的 `uber-jar` 僅在 `classic` 調用配置檔案，即 `mvn clean install -PautoInstallSinglePackage -Pclassic`。 同樣，這是此項目所獨有的。 在真實世界項目中，根據項目原AEM型生成 `uber-jar` 如果指定的版本為6.5或6.4，則AEM為預設值。

   的 [優步罐](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) 包含6.x公開的所AEM有公共Java™ API。版本從項目的根部保留在Parent反應器pom中 `aem-guides-wknd/pom.xml`。

1. 查找的依賴項 `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   這是核心元件公開的完整公AEM用Java™ API。 核AEM心元件是在外部維護的項AEM目，因此有單獨的發佈週期。 因此，它是一個需要單獨包含的依賴項，並且 **不** 包含 `uber-jar` 或 `aem-sdk-api`。

   與uber-jar一樣，此依賴項的版本在父反應器pom檔案中維護 `aem-guides-wknd/pom.xml`。

   在本教程的後面部分，核心元件映像類用於在Byline元件中顯示映像。 為了建立和編譯Sling模型，必須具有核心元件相關性。

### Byline介面 {#byline-interface}

為Byline建立公共Java™介面。 的 `Byline.java` 定義驅動 `byline.html` HTL指令碼。

1. 裡面， `core` 模組 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 資料夾建立名為 `Byline.java`

   ![建立行介面](assets/custom-component/create-byline-interface.png)

1. 更新 `Byline.java` 採用以下方法：

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

   前兩種方法將顯示 **名稱** 和 **職業** 的下界。

   的 `isEmpty()` 方法用於確定元件是否具有要呈現的內容或是否正在等待配置。

   請注意，沒有對影像執行任何方法； [以後將審閱](#tackling-the-image-problem)。

1. 包含公共Java™類的Java™包（本例中為Sling模型）必須使用包的版本更新  `package-info.java` 的子菜單。

   自WKND源的Java™軟體包以來 `com.adobe.aem.guides.wknd.core.models` 聲明版本 `1.0.0`，並且添加了不中斷的公共介面和方法，必須將版本增加到 `1.1.0`。 在以下位置開啟檔案 `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` 更新 `@Version("1.0.0")` 至 `@Version("2.1.0")`。

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

只要對此包中的檔案進行更改， [包版本必須在語義上進行調整](https://semver.org/)。 否則，馬文項目 [bnd-baseline-maven插件](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) 檢測無效的包版本並中斷生成。 幸運的是，在失敗時，Maven插件報告無效的Java™軟體包版本及其應該的版本。 更新 `@Version("...")` 違反Java™包的聲明 `package-info.java` 到插件建議的版本進行修復。

### Byline實現 {#byline-implementation}

的 `BylineImpl.java` 是Sling模型的實現 `Byline.java` 介面。 的完整代碼 `BylineImpl.java` 的下方。

1. 建立名為 `impl` 下 `core/src/main/java/com/adobe/aem/guides/core/models`。
1. 在 `impl` 資料夾，建立檔案 `BylineImpl.java`。

   ![行導入檔案](assets/custom-component/byline-impl-file.png)

1. 開啟 `BylineImpl.java`. 指定它實現 `Byline` 。 使用IDE的自動完成功能或手動更新檔案，以包括實現 `Byline` 介面：

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

1. 通過更新添加Sling模型注釋 `BylineImpl.java` 以下類級注釋。 此 `@Model(..)`注釋是將類變為Sling模型的內容。

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

   讓我們查看此注釋及其參數：

   * 的 `@Model` 注釋將BylineImpl部署到時註冊為Sling模AEM型。
   * 的 `adaptables` 參數指定此模型可以根據請求進行調整。
   * 的 `adapters` 參數允許在Byline介面下註冊實現類。 這允許HTL指令碼通過介面（而不是直接實現）調用Sling Model。 [有關適配器的詳細資訊，請參閱](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)。
   * 的 `resourceType` 指向Byline元件資源類型（先前建立），並幫助在存在多個實現時解析正確的模型。 [有關將模型類與資源類型關聯的詳細資訊，請參閱此處](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)。

### 吊具模型方法的實現 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

實現的第一個方法是 `getName()`，它只是將儲存到該屬性下行的JCR內容節點的值 `name`。

為了這個， `@ValueMapValue` Sling Model注釋用於使用請求資源的ValueMap將值注入Java™欄位。


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

由於JCR屬性將名稱共用為Java™欄位（兩者均為&quot;name&quot;）, `@ValueMapValue` 自動解析此關聯，並將屬性值彈出到Java™欄位。

#### getSchrops() {#implementing-get-occupations}

接下來要實現的方法是 `getOccupations()`。 此方法載入JCR屬性中儲存的職位 `occupations` 並返回已排序（按字母順序）的集合。

使用在 `getName()` 屬性值可以注入到Sling模型的欄位中。

一旦JCR屬性值通過注入的Java™欄位在Sling模型中可用 `occupations`，分類業務邏輯可應用於 `getOccupations()` 的雙曲餘切值。


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

最後一個公共方法是 `isEmpty()` 它確定元件何時應考慮「已創作足夠」來呈現。

對於這個元件，業務需求是三個領域， `name, image and occupations` 必須填空 *先* 可以呈現元件。


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

檢查名稱和職業條件很瑣碎，Apache Commons Lang3提供了方便 [字串實用程式](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) 類。 不過，目前還不清楚 **影像的存在** 由於核心元件影像元件用於表面影像，因此可以驗證。

解決這一問題有兩種方法：

檢查 `fileReference` JCR屬性解析為資產。 *或* 將此資源轉換為核心元件影像吊帶模型，並確保 `getSrc()` 方法不為空。

讓我們使用 **第二** 方法。 第一種方法可能已足夠，但在本教程中，後一種方法用於讓我們探索Sling Models的其他功能。

1. 建立獲取映像的專用方法。 此方法保持私有狀態，因為無需在HTL本身中公開Image對象，並且它僅用於驅動 `isEmpty().`

   為添加以下私有方法 `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，還有兩種方法 **影像吊具模型**:

   第一個使用 `@Self` 注釋，以自動將當前請求調整為「核心元件」 `Image.class`

   第二個使用 [Apache Sling模型工廠](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服務是一項方便的服務，它幫助我們在Java™代碼中建立其他類型的Sling模型。

   我們用第二種方法。

   >[!NOTE]
   >
   >在實際實施中，採用&quot;一&quot;，使用 `@Self` 更簡單、更優雅的解決方案。 在本教程中，使用了第二種方法，因為它需要探索Sling Models的更多有用的方面是更複雜的元件！

   由於Sling Models是Java™ POJO，而不是OSGi Services，因此通常的OSGi注入注釋 `@Reference` **不能** 而Sling Models提供了 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 提供類似功能的注釋。

1. 更新 `BylineImpl.java` 包含 `OSGiService` 要注入的注釋 `ModelFactory`:

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

   使用 `ModelFactory` 可用，可使用以下方式建立核心元件影像吊帶模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   但是，此方法需要請求和資源，但Sling模型中尚未提供。 要獲取這些注釋，請使用更多Sling模型注釋！

   要獲取當前請求，請 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 注釋可用於注入 `adaptable` (定義見 `@Model(..)` 如 `SlingHttpServletRequest.class`，進入Java™類欄位。

1. 添加 **@Self** 要獲取的注釋 **SlingHttpServletRequest請求**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   記住，使用 `@Self Image image` 以注入核心元件影像吊掛模型是上面的選項 —  `@Self` 批注嘗試插入可修改的對象（在本例中為SlingHttpServletRequest），並適應批注欄位類型。 由於核心元件影像Sling模型可以從SlingHttpServletRequest對象中修改，因此這樣做會起作用，並且比較探索性的代碼少 `modelFactory` 方法。

   現在，通過ModelFactory API實例化映像模型所需的變數將被注入。 讓我們用Sling模型 **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 注釋，以在Sling模型實例化後獲取此對象。

   `@PostConstruct` 它非常有用，並且以與建構子類似的容量運行，但是，在實例化類並注入所有注釋的Java™欄位後，會調用它。 其他Sling模型注釋可注釋Java™類欄位（變數）, `@PostConstruct` 注釋void，零參數方法，通常名為 `init()` （但任何東西都可以被命名）。

1. 添加 **@PostConstruct** 方法：

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

   記住，吊帶模型 **不** OSGi服務，因此維護階級狀態是安全的。 通常 `@PostConstruct` 派生並設定Sling Model類狀態供以後使用，類似於普通建構子的操作。

   如果 `@PostConstruct` 方法引發異常，Sling模型未實例化且為空。

1. **getImage()** 現在可以更新以僅返回影像對象。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 我們回到 `isEmpty()` 並完成實施：

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

   將多個呼叫記錄到 `getImage()` 沒有問題，因為返回已初始化 `image` 類變數和不調用 `modelFactory.getModelFromWrappedRequest(...)` 這並不過於昂貴，但值得避免不必要地打電話。

1. 決賽 `BylineImpl.java` 應該是這樣的：


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


## Byline HTL {#byline-htl}

在 `ui.apps` 模組，開啟 `/apps/wknd/components/byline/byline.html` 在「元件」的早期設定中創AEM建的。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

讓我們回顧一下此HTL指令碼到目前為止的功能：

* 的 `placeholderTemplate` 指向「核心元件」佔位符，該佔位符在元件未完全配置時顯示。 這在AEM Sites頁面編輯器中呈現為帶有元件標題的框，如上所定義 `cq:Component``s  `jcr:title` 屬性。

* 的 `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 載入 `placeholderTemplate` 定義並以布爾值傳遞(當前硬編碼為 `false`)。 當 `isEmpty` 如果為true，則佔位符模板將呈現灰色框，否則將不呈現任何內容。

### 更新行HTL

1. 更新 **byline.html** 骨骼HTML結構。

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

   請注意，CSS類 [BEM命名約定](https://getbem.com/naming/)。 雖然BEM約定不是強制性的，但建議使用BEM，因為它在核心元件CSS類中使用，並且通常會生成乾淨、可讀的CSS規則。

### HTL中Sling模型對象實例化 {#instantiating-sling-model-objects-in-htl}

的 [使用block語句](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) 用於在HTL指令碼中實例化Sling Model對象，並將其分配給HTL變數。

的 `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 使用由BylineImpl實現的Byline介面(com.adobe.aem.guides.wknd.models.Byline)，並將當前SlingHttpServletRequest適用於它，結果儲存在HTL變數名稱的行中( `data-sly-use.<variable-name>`)。

1. 更新外部 `div` 引用 **副線** Sling Model通過其公共介面：

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### Sling模型的訪問方法 {#accessing-sling-model-methods}

HTL借用JSTL，並使用Java™ getter方法名稱的相同縮短。

例如，調用Byline Sling模型 `getName()` 方法可縮短為 `byline.name`，類似 `byline.isEmpty`，這可以短到 `byline.empty`。 使用完整方法名稱， `byline.getName` 或 `byline.isEmpty`，同樣有效。 注意 `()` 在HTL中，從不使用調用方法（與JSTL類似）。

需要參數的Java™方法 **不能** 用於HTL。 這是為了使HTL中的邏輯保持簡單而設計的。

1. 通過調用 `getName()` 方法（在Byline Sling Model中）或HTL中： `${byline.name}`。

   更新 `h2` 標籤：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用HTL表達式選項 {#using-htl-expression-options}

[HTL表達式選項](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) 在HTL中充當內容的修飾符，範圍從日期格式到i18n翻譯。 表達式還可用於連接清單或值陣列，這些值是以逗號分隔格式顯示職業所需要的。

通過 `@` HTL表達式中的運算子。

1. 要加入具有&quot;, &quot;的職業清單，請使用以下代碼：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 有條件地顯示佔位符 {#conditionally-displaying-the-placeholder}

大多數用於元件AEM的HTL指令碼使用 **佔位符** 為作者提供視覺提示 **表示元件創作不正確，且未在AEM發佈上顯示**。 驅動此決定的慣例是在元件的後援Sling Model上實施一種方法，在本例中： `Byline.isEmpty()`。

的 `isEmpty()` 方法在Byline Sling模型上調用，結果(或者，它是負的，通過 `!` 運算子)保存到名為 `hasContent`:

1. 更新外部 `div` 保存名為的HTL變數 `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   注意 `data-sly-test`, HTL `test` block是key，它同時設定HTL變數並呈現/不呈現它所在的HTML元素。 它基於HTL表達式評價結果。 如果「true」，則HTML元素呈現，否則不呈現。

   此HTL變數 `hasContent` 現在可重新使用，以有條件地顯示/隱藏佔位符。

1. 將條件調用更新到 `placeholderTemplate` 檔案底部，具有以下內容：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心元件顯示影像 {#using-the-core-components-image}

HTL指令碼 `byline.html` 現在已接近完成，並且只是缺少影像。

作為 `sling:resourceSuperType` 指向核心元件的影像元件以建立影像，核心元件的影像元件可用於渲染影像。

為此，讓我們包括當前的行資源，但使用資源類型強制核心元件的映像元件的資源類型 `core/wcm/components/image/v2/image`。 這是一種強大的元件重用模式。 為此，HTL `data-sly-resource` 框。

1. 替換 `div` 和 `cmp-byline__image` 下面列出：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   此 `data-sly-resource`，通過相對路徑包括當前資源 `'.'`，並強制將當前資源（或行內內容資源）與資源類型 `core/wcm/components/image/v2/image`。

   核心元件資源類型直接使用，而不是通過代理使用，因為這是指令碼內使用，並且從未持久保留到內容。

2. 已完成 `byline.html` 以下：

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

3. 將代碼庫部署到本地實AEM例。 因為對 `core` 和 `ui.apps` 兩個模組都需要部署。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   部署到AEM6.5/6.4調用 `classic` 配置檔案：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > 您也可以使用Maven配置檔案從根構建整個項目 `autoInstallSinglePackage` 但這可能會覆蓋頁面上的內容更改。 這是因為 `ui.content/src/main/content/META-INF/vault/filter.xml` 已修改教程啟動程式碼以完全覆蓋現有AEM內容。 在現實世界中，這不是問題。

### 查看未設樣式的Byline元件 {#reviewing-the-unstyled-byline-component}

1. 部署更新後，導航至 [洛杉磯滑板終極指南 ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 或您在本章前面添加的Byline元件的位置。

1. 的 **影像**。 **名稱**, **職業** 現在顯示，且未設定樣式，但存在工作的Byline元件。

   ![非樣式的位元組元件](assets/custom-component/unstyled.png)

### 查看吊具模型註冊 {#reviewing-the-sling-model-registration}

的 [AEMWeb控制台的Sling模型狀態視圖](http://localhost:4502/system/console/status-slingmodels) 顯示中註冊的所有Sling模AEM型。 可以通過查看此清單來驗證Byline Sling Model是否已安裝並識別。

如果 **BylineImpl** 清單中未顯示，Sling Model的注釋或模型未添加到正確的包中可能出現問題(`com.adobe.aem.guides.wknd.core.models`)。

![已註冊Byline Sling模型](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 行樣式 {#byline-styles}

要將Byline元件與提供的創意設計對齊，讓我們對其進行樣式設定。 這是通過使用SCSS檔案和在 **ui.frontend** 中。

### 添加預設樣式

為Byline元件添加預設樣式。

1. 返回到IDE和 **ui.frontend** 項目 `/src/main/webpack/components`:
1. 建立名為 `_byline.scss`。

   ![行項目瀏覽器](assets/custom-component/byline-style-project-explorer.png)

1. 將Byline實現CSS（寫為SCSS）添加到 `_byline.scss`:

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

1. 開啟終端並導航到 `ui.frontend` 中。
1. 啟動 `watch` 使用以下npm命令進行進程：

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 返回到瀏覽器並導航到 [洛杉磯冰鞋公園條目](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 您應看到元件的更新樣式。

   ![完成的行元件](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 您可能需要清除瀏覽器快取以確保不提供陳舊的CSS，並使用Byline元件刷新頁面以獲取完整樣式。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager從頭建立了自定義元件！

### 後續步驟 {#next-steps}

繼續瞭解元件開AEM發，方法是為Byline Java™代碼編寫JUnittest，以確保所有內容都得到正確開發，並且實現的業務邏輯正確且完整。

* [編寫設備TestAEM或元件](unit-testing.md)

查看完成的代碼 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上本地查看和部署代碼 `tutorial/custom-component-solution`。

1. 克隆 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 儲存庫。
1. 查看 `tutorial/custom-component-solution` 分支
