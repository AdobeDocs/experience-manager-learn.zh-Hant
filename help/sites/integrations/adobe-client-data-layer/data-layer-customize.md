---
title: 使用AEM元件自訂Adobe使用者端資料層
description: 瞭解如何使用自訂Adobe元件的內容來自訂AEM Client Data Layer。 瞭解如何使用AEM核心元件提供的API來擴充及自訂資料層。
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
jira: KT-6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
doc-type: Tutorial
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
duration: 452
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1813'
ht-degree: 2%

---

# 使用AEM元件自訂Adobe使用者端資料層 {#customize-data-layer}

瞭解如何使用自訂Adobe元件的內容來自訂AEM Client Data Layer。 瞭解如何使用[AEM核心元件提供的API來擴充](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html?lang=zh-Hant)和自訂資料層。

## 您即將建置的內容

![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

在本教學課程中，讓我們透過更新WKND [Byline元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=zh-Hant)，探索擴充Adobe Client Data Layer的各種選項。 _Byline_&#x200B;元件是&#x200B;**自訂元件**，在本教學課程中學到的課程可套用至其他自訂元件。

### 目標 {#objective}

1. 透過延伸Sling模型和元件HTL，將元件資料插入資料層
1. 使用核心元件資料層公用程式來減少工作量
1. 使用核心元件資料屬性以連結至現有的資料層事件

## 先決條件 {#prerequisites}

需要&#x200B;**本機開發環境**&#x200B;才能完成此教學課程。 熒幕截圖和影片都是使用在macOS上執行的AEM as a Cloud Service SDK擷取。 除非另有說明，否則指令和程式碼與本機作業系統無關。

**AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)。

**AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hant)。

## 下載及部署WKND參考網站 {#set-up-wknd-site}

本教學課程會擴充WKND參考網站中的Byline元件。 將WKND程式碼基底複製並安裝至您的本機環境。

1. 啟動在[http://localhost:4502](http://localhost:4502)執行的本機Quickstart **作者** AEM執行個體。
1. 開啟終端機視窗並使用Git複製WKND程式碼庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 將WKND程式碼基底部署到AEM的本機執行個體：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 若為AEM 6.5和最新的Service Pack，請將`classic`設定檔新增至Maven命令：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 開啟新的瀏覽器視窗並登入AEM。 開啟&#x200B;**雜誌**&#x200B;頁面，例如： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   第![&#128279;](assets/adobe-client-data-layer/byline-component-onpage.png)頁上的署名元件

   您應該會看到作為體驗片段的一部分新增到頁面的署名元件範例。 您可以在[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)檢視體驗片段
1. 開啟您的開發人員工具，並在&#x200B;**主控台**&#x200B;中輸入下列命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   若要檢視AEM網站上資料層的目前狀態，請檢查回應。 您應該會看到頁面和個別元件的相關資訊。

   ![Adobe資料層回應](assets/data-layer-state-response.png)

   請注意，署名元件並未列於資料層中。

## 更新署名Sling模型 {#sling-model}

若要在資料層中插入元件的相關資料，請先更新元件的Sling模型。 接下來，更新Byline的Java™介面和Sling模型實作，以使用新方法`getData()`。 此方法包含要插入資料層的屬性。

1. 在您選擇的IDE中開啟`aem-guides-wknd`專案。 導覽至`core`模組。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`開啟檔案`Byline.java`。

   ![署名Java介面](assets/adobe-client-data-layer/byline-java-interface.png)

1. 將下列方法新增至介面：

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

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`開啟檔案`BylineImpl.java`。 這是`Byline`介面的實作，並且實作為Sling模型。

1. 將下列匯入陳述式新增至檔案的開頭：

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API可用來序列化要公開為JSON的資料。 AEM核心元件的`ComponentUtils`用於檢查資料層是否已啟用。

1. 將未實作的方法`getData()`新增至`BylineImple.java`：

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

   在上述方法中，使用新的`HashMap`來擷取要公開為JSON的屬性。 請注意，已使用`getName()`和`getOccupations()`等現有方法。 `@type`代表元件的唯一資源型別，它可讓使用者端根據元件的型別輕鬆識別事件和/或觸發器。

   `ObjectMapper`可用來序列化屬性並傳回JSON字串。 然後可以將此JSON字串插入資料層中。

1. 開啟終端機視窗。 僅使用您的Maven技能建置和部署`core`模組：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 更新署名HTL {#htl}

接下來，更新`Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=zh-Hant)。 HTL (HTML範本語言)是用來呈現元件HTML的範本。

每個AEM元件上的特殊資料屬性`data-cmp-data-layer`可用來公開其資料層。 AEM核心元件所提供的JavaScript會尋找此資料屬性。 此資料屬性的值會以Byline Sling模型的`getData()`方法傳回的JSON字串填入，並插入至Adobe使用者端資料層。

1. 在IDE中開啟`aem-guides-wknd`專案。 導覽至`ui.apps`模組。
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

   `data-cmp-data-layer`的值已設定為`"${byline.data}"`，其中`byline`是先前更新的Sling模型。 `.data`是呼叫HTL中`getData()`之Java™ Getter方法的標準標籤法（在上一個練習中實作）。

1. 開啟終端機視窗。 僅使用您的Maven技能建置和部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，然後使用Byline元件重新開啟頁面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 開啟開發人員工具，並檢查Byline元件頁面的HTML來源：

   ![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

   您應該會看到`data-cmp-data-layer`已填入來自Sling模型的JSON字串。

1. 開啟瀏覽器的開發人員工具，並在&#x200B;**主控台**&#x200B;中輸入下列命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 導覽至`component`下回應下方，尋找已新增至資料層的`byline`元件執行個體：

   ![資料層的署名部分](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   您應該會看到類似以下的專案：

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   請注意，公開的屬性與Sling模型中`HashMap`新增的屬性相同。

## 新增點選事件 {#click-event}

Adobe Client Data Layer是事件導向，而觸發動作的最常見事件之一是`cmp:click`事件。 AEM核心元件可透過資料元素`data-cmp-clickable`輕鬆註冊您的元件。

可點按的元素通常是CTA按鈕或導覽連結。 很遺憾，署名元件沒有這些專案，但我們仍會註冊它，因為這可能適用於其他自訂元件。

1. 在IDE中開啟`ui.apps`模組
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`開啟檔案`byline.html`。

1. 更新`byline.html`以包含Byline的&#x200B;**name**&#x200B;專案上的`data-cmp-clickable`屬性：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 開啟新的終端機。 僅使用您的Maven技能建置和部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，然後使用新增的署名元件重新開啟頁面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   為了測試事件，我們將使用開發人員控制檯手動新增一些JavaScript。 如需如何執行此動作的影片，請參閱[搭配使用Adobe使用者端資料層與AEM核心元件](data-layer-overview.md)。

1. 開啟瀏覽器的開發人員工具，並在&#x200B;**主控台**&#x200B;中輸入下列方法：

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   這個簡單方法應該會處理署名元件名稱的點按。

1. 在&#x200B;**主控台**&#x200B;中輸入下列方法：

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上述方法會將事件接聽程式推送到資料層以接聽`cmp:click`事件並呼叫`bylineClickHandler`。

   >[!CAUTION]
   >
   > 在本練習中，很重要的一點是&#x200B;**不**&#x200B;要重新整理瀏覽器，否則主控台JavaScript會遺失。

1. 在瀏覽器中，開啟&#x200B;**主控台**&#x200B;後，按一下Byline元件中的作者名稱：

   已按下![署名元件](assets/adobe-client-data-layer/byline-component-clicked.png)

   您應該會看到主控台訊息`Byline Clicked!`和署名名稱。

   `cmp:click`事件是最容易連結到的。 如需更複雜的元件及追蹤其他行為，可以新增自訂JavaScript以新增及註冊新事件。 Carousel元件是很好的範例，每當切換幻燈片時會觸發`cmp:show`事件。 如需更多詳細資料，請參閱[原始程式碼](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js)。

## 使用DataLayerBuilder公用程式 {#data-layer-builder}

在本章的早期，當Sling模型已[更新](#sling-model)時，我們選擇使用`HashMap`並手動設定每個屬性來建立JSON字串。 此方法適用於小型一次性元件，但若是擴充AEM核心元件的元件，可能會導致大量額外程式碼。

公用程式類別`DataLayerBuilder`可用來執行大部分的繁重工作。 這允許實作僅擴充其所需的屬性。 讓我們更新Sling模型以使用`DataLayerBuilder`。

1. 返回IDE並導覽至`core`模組。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`開啟檔案`Byline.java`。
1. 修改`getData()`方法以傳回`ComponentData`的型別

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

   `ComponentData`是由AEM核心元件提供的物件。 這會產生JSON字串，就像上一個範例一樣，但也會執行許多其他工作。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`開啟檔案`BylineImpl.java`。

1. 新增下列匯入陳述式：

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. 將`getData()`方法取代為下列專案：

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

   「署名」元件會重複使用影像核心元件的部分，以顯示代表作者的影像。 在上述程式碼片段中，[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)是用來擴充`Image`元件的資料層。 如此會使用所用影像的所有相關資料預先填入JSON物件。 它也會執行某些常式功能，例如設定`@type`和元件的唯一識別碼。 請注意，方法很小！

   唯一一個延伸`withTitle`的屬性已被`getName()`的值取代。

1. 開啟終端機視窗。 僅使用您的Maven技能建置和部署`core`模組：

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

   請記住，我們現在正在傳回型別`ComponentData`的物件。 此物件包含getter方法`getJson()`，且用來填入`data-cmp-data-layer`屬性。

1. 開啟終端機視窗。 僅使用您的Maven技能建置和部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，然後使用新增的署名元件重新開啟頁面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。
1. 開啟瀏覽器的開發人員工具，並在&#x200B;**主控台**&#x200B;中輸入下列命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 瀏覽至`component`下回應下方，尋找`byline`元件的執行個體：

   ![署名資料層已更新](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   您應該會看到類似以下的專案：

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

   請注意，`byline`元件專案內現在有`image`物件。 這包含更多有關DAM中資產的資訊。 另請注意，`@type`和唯一ID （在此例中為`byline-136073cfcb`）已自動填入，且`repo:modifyDate`表示元件已修改的時間。

## 其他範例 {#additional-examples}

1. 您可以透過檢查WKND程式碼基底中的`ImageList`元件來檢視擴充資料層的另一個範例：
   * `ImageList.java` - `core`模組中的Java介面
   * `ImageListImpl.java` - `core`模組中的Sling模型。
   * `image-list.html` - `ui.apps`模組中的HTL範本。

   >[!NOTE]
   >
   > 使用[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)時，納入`occupation`等自訂屬性會比較困難。 不過，如果擴充包含影像或頁面的核心元件，公用程式會節省大量時間。

   >[!NOTE]
   >
   > 如果為實作中重複使用的物件建立進階資料層，建議將資料層元素擷取到它們自己的資料層特定Java™物件中。 例如，Commerce核心元件已新增`ProductData`和`CategoryData`的介面，因為這些介面可用於Commerce實作中的許多元件。 如需詳細資訊，請檢閱aem-cif-core-components repo[&#128279;](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)中的程式碼。

## 恭喜！ {#congratulations}

您剛剛探索了幾個方法，可使用AEM元件擴充和自訂Adobe Client Data Layer！

## 其他資源 {#additional-resources}

* [Adobe使用者端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [資料層與核心元件的整合](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [使用Adobe使用者端資料層和核心元件檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=zh-Hant)
