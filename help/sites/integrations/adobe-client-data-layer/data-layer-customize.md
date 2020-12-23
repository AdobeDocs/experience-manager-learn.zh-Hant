---
title: 使用AEM元件自訂Adobe用戶端資料層
description: 瞭解如何使用自訂AEM元件的內容自訂Adobe用戶端資料層。 瞭解如何使用AEM核心元件提供的API來擴充和自訂資料層。
feature: core-component
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
translation-type: tm+mt
source-git-commit: 46936876de355de9923f7a755aa6915a13cca354
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 1%

---


# 使用AEM元件{#customize-data-layer}自訂Adobe用戶端資料層

瞭解如何使用自訂AEM元件的內容自訂Adobe用戶端資料層。 瞭解如何使用[AEM核心元件提供的API來擴充](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)並自訂資料層。

## 您將建立的

![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

在本教學課程中，您將探討如何更新WKND [Byline元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html)來擴充Adobe用戶端資料層的各種選項。 這是自訂元件，本教學課程中的學習經驗可套用至其他自訂元件。

### 目標{#objective}

1. 透過擴充Sling Model和元件HTL，將元件資料注入資料層
1. 使用核心元件資料層公用程式來減少工作量
1. 使用核心元件資料屬性，將其套用至現有的資料層事件

## 必備條件 {#prerequisites}

要完成本教程，需要&#x200B;**本地開發環境**。 螢幕擷取畫面和視訊會使用AEM擷取，當成在macOS上執行的Cloud Service SDK。 除非另有說明，否則命令和程式碼獨立於本機作業系統。

**您是AEM的新手嗎？** 請參閱下 [列指南，以使用AEM做為雲端服務SDK來設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**您是AEM 6.5的新手嗎？** 請參閱以 [下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 下載並部署WKND參考站點{#set-up-wknd-site}

本教學課程將WKND參考網站中的Byline元件延伸。 將WKND程式碼庫複製並安裝至您的本機環境。

1. 啟動在[http://localhost:4502](http://localhost:4502)執行的本機AEM快速入門&#x200B;**author**&#x200B;執行個體。
1. 開啟終端視窗，並使用Git複製WKND程式碼庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 將WKND程式碼庫部署至AEM的本機例項：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5和最新的Service Pack，請將`classic`描述檔新增至Maven命令：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 開啟新的瀏覽器視窗並登入AEM。 開啟&#x200B;**雜誌**&#x200B;頁面，如下所示：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![頁面上的Byline元件](assets/adobe-client-data-layer/byline-component-onpage.png)

   您應該會看到已新增至頁面的Byline元件範例，做為體驗片段的一部分。 您可以在[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)檢視體驗片段
1. 開啟您的開發人員工具，並在&#x200B;**Console**&#x200B;中輸入下列命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   檢查回應，以查看AEM網站上資料層的目前狀態。 您應該會看到頁面和個別元件的相關資訊。

   ![Adobe資料層回應](assets/data-layer-state-response.png)

   請注意，「資料層」中未列出Byline元件。

## 更新Byline Sling Model {#sling-model}

若要在資料層中插入元件的相關資料，我們必須先更新元件的Sling Model。 接著，更新Byline的Java介面和Sling Model實作，以新增新方法`getData()`。 此方法將包含我們要插入資料層的屬性。

1. 在您選擇的IDE中，開啟`aem-guides-wknd`項目。 導航到`core`模組。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`開啟檔案`Byline.java`。

   ![署名Java介面](assets/adobe-client-data-layer/byline-java-interface.png)

1. 將新方法新增至介面：

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`開啟檔案`BylineImpl.java`。

   這是`Byline`介面的實作，並實作為Sling Model。

1. 將以下導入語句添加到檔案的開頭：

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API將用來序列化我們要公開為JSON的資料。 AEM Core Components的`ComponentUtils`將用來檢查資料層是否已啟用。

1. 將未實作的方法`getData()`新增至`BylineImple.java`:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   在上述方法中，新`HashMap`可用來擷取我們要公開為JSON的屬性。 請注意，已使用現有方法，例如`getName()`和`getOccupations()`。 `@type` 代表元件的唯一資源類型，這可讓用戶端根據元件類型輕鬆識別事件和／或觸發器。

   `ObjectMapper`可用來序列化屬性並傳回JSON字串。 然後，此JSON字串可插入資料層。

1. 開啟終端窗口。 使用您的Maven技巧，只建立和部署`core`模組：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 更新署名HTL {#htl}

接著，更新`Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl)。 HTL（HTML範本語言）是用來轉換元件HTML的範本。

每個AEM元件上的特殊資料屬性`data-cmp-data-layer`可用來公開其資料層。  AEM Core Components提供的JavaScript會尋找此資料屬性，其值將會填入Byline Sling Model的`getData()`方法傳回的JSON字串，並將值插入Adobe Client資料層。

1. 在IDE中，開啟`aem-guides-wknd`項目。 導航到`ui.apps`模組。
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`開啟檔案`byline.html`。

   ![署名HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. 更新`byline.html`以包含`data-cmp-data-layer`屬性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   `data-cmp-data-layer`的值已設為`"${byline.data}"`，其中`byline`是Sling Model的更新日期。 `.data` 是在先前練習中實作的HTL中呼叫Java Getter方 `getData()` 法的標準符號。

1. 開啟終端窗口。 使用您的Maven技巧，只建立和部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器並使用Byline元件重新開啟頁面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 開啟開發人員工具並檢查頁面的HTML來源，以取得Byline元件：

   ![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

   您應該會看到`data-cmp-data-layer`已填入來自Sling Model的JSON字串。

1. 開啟瀏覽器的開發人員工具，並在&#x200B;**Console**&#x200B;中輸入下列命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的響應下方導航，以查找`byline`元件已添加到資料層的實例：

   ![資料層的署名部分](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   您應看到如下項目：

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   請注意，公開的屬性與Sling Model中`HashMap`中新增的屬性相同。

## 新增點按事件{#click-event}

Adobe用戶端資料層是事件導向，觸發動作的最常見事件之一是`cmp:click`事件。 AEM Core元件可讓您透過資料元素輕鬆註冊元件：`data-cmp-clickable`。

可點按的元素通常是CTA按鈕或導覽連結。 很遺憾，Byline元件沒有這些元件，但我們將以任何方式註冊它，因為這可能對其他自定義元件很常見。

1. 在IDE中開啟`ui.apps`模組
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`開啟檔案`byline.html`。

1. 更新`byline.html`，將`data-cmp-clickable`屬性包含在Byline的&#x200B;**name**&#x200B;元素中：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 開啟新的終端機。 使用您的Maven技巧，只建立和部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並重新開啟已新增Byline元件的頁面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   若要測試我們的事件，我們會使用開發人員主控台手動新增一些JavaScript。 如需如何進行此動作的影片，請參閱[搭配使用Adobe用戶端資料層與AEM核心元件](data-layer-overview.md)。

1. 開啟瀏覽器的開發人員工具，並在&#x200B;**Console**&#x200B;中輸入下列方法：

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   此簡單方法應處理Byline元件名稱的按一下。

1. 在&#x200B;**控制台**&#x200B;中輸入以下方法：

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上述方法將事件偵聽器推送至資料層以監聽`cmp:click`事件並呼叫`bylineClickHandler`。

   >[!CAUTION]
   >
   > 在本練習中重新整理瀏覽器很重要，否則主控台JavaScript將會遺失。****

1. 在瀏覽器中，當&#x200B;**Console**&#x200B;開啟時，按一下Byline元件中作者的名稱：

   ![已點按Byline元件](assets/adobe-client-data-layer/byline-component-clicked.png)

   您應看到控制台消息`Byline Clicked!`和位行名稱。

   `cmp:click`事件最容易連接。 若要建立更複雜的元件並追蹤其他行為，可新增自訂javascript以新增和註冊新事件。 最棒的例子是轉盤元件，每當切換投影片時，就會觸發`cmp:show`事件。 如需詳細資訊，請參閱[原始碼](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219)。

## 使用DataLayerBuilder實用程式{#data-layer-builder}

當Sling Model在章節中較早時為[updated](#sling-model)時，我們選擇使用`HashMap`並手動設定每個屬性來建立JSON字串。 此方法適用於小型一次性元件，但是對於擴充AEM核心元件的元件，這可能會產生大量額外的程式碼。

`DataLayerBuilder`實用程式類用於執行大部分的繁重工作。 這可讓實作只擴充其所需的屬性。 讓我們更新Sling Model以使用`DataLayerBuilder`。

1. 返回IDE並導航至`core`模組。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`開啟檔案`Byline.java`。
1. 修改`getData()`方法以返回`ComponentData`類型

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` 是AEM核心元件提供的物件。它會產生JSON字串，就像上個範例中的一樣，但也會執行許多額外的工作。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`開啟檔案`BylineImpl.java`。

1. 添加以下導入語句：

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. 將`getData()`方法替換為：

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   Byline元件重新使用影像核心元件的部分來顯示代表作者的影像。 在上述程式碼片段中，[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)用於延伸`Image`元件的資料層。 這會預先填入JSON物件，其中包含所用影像的所有資料。 它還執行一些例行功能，如設定元件的`@type`和唯一標識符。 請注意，這個方法太小了！

   唯一的屬性擴展了`withTitle`，該`getName()`被值替換。

1. 開啟終端窗口。 使用您的Maven技巧，只建立和部署`core`模組：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. 返回IDE並開啟`ui.apps`下的`byline.html`檔案
1. 更新HTL以使用`byline.data.json`填入`data-cmp-data-layer`屬性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   請記住，我們現在傳回的物件類型為`ComponentData`。 此對象包括getter方法`getJson()`，它用於填充`data-cmp-data-layer`屬性。

1. 開啟終端窗口。 使用您的Maven技巧，只建立和部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並重新開啟已新增Byline元件的頁面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。
1. 開啟瀏覽器的開發人員工具，並在&#x200B;**Console**&#x200B;中輸入下列命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的響應下導航以查找`byline`元件的實例：

   ![已更新署名資料層](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   您應看到如下項目：

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   請注意，`byline`元件條目中現在有`image`對象。 這會提供更多有關DAM中資產的資訊。 另請注意，`@type`和唯一ID（在本例中為`byline-136073cfcb`）已自動填入，以及`repo:modifyDate`在修改元件時指示的值。

## 其他範例{#additional-examples}

1. 延伸資料層的另一個範例是檢查WKND程式碼庫中的`ImageList`元件：
   * `ImageList.java` -模組中的Java `core` 介面。
   * `ImageListImpl.java` -模組中的Sling `core`  Model。
   * `image-list.html` -模組中的HTL `ui.apps` 模板。

   >[!NOTE]
   >
   > 使用[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)時，要包含例如`occupation`等自訂屬性會比較困難。 但是，如果擴充包含影像或頁面的核心元件，此公用程式可節省大量時間。

   >[!NOTE]
   >
   > 如果為整個實作中重複使用的物件建立進階資料層，建議將資料層元素擷取至其專屬的資料層Java物件。 例如，Commerce Core Components已新增`ProductData`和`CategoryData`的介面，因為這些介面可用於Commerce實作中的許多元件。 請參閱aem-cif-core-components repo](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)中的[程式碼，以取得詳細資訊。

## 恭喜！{#congratulations}

您剛才探索了幾種使用AEM元件擴充和自訂Adobe用戶端資料層的方式！

## 其他資源 {#additional-resources}

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [資料層與核心元件整合](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [使用Adobe用戶端資料層與核心元件檔案](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html)
