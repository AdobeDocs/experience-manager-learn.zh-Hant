---
title: 使用AEM元件自訂Adobe用戶端資料層
description: 了解如何使用自訂AEM元件的內容來自訂Adobe用戶端資料層。 了解如何使用AEM核心元件提供的API來擴充和自訂資料層。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '2016'
ht-degree: 2%

---

# 使用AEM元件自訂Adobe用戶端資料層 {#customize-data-layer}

了解如何使用自訂AEM元件的內容來自訂Adobe用戶端資料層。 了解如何使用 [AEM要擴充的核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html) 和自訂資料層。

## 您將建置的

![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

在本教學課程中，您將探討如何更新WKND以擴充Adobe用戶端資料層 [署名元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html). 這是自訂元件，本教學課程中的經驗教訓可套用至其他自訂元件。

### 目標 {#objective}

1. 延伸Sling模型和元件HTL，將元件資料插入資料層
1. 使用核心元件資料層公用程式來減輕工作量
1. 使用核心元件資料屬性連結至現有的資料層事件

## 必備條件 {#prerequisites}

A **本地開發環境** 是完成本教學課程的必要項目。 螢幕擷取畫面和影片是使用在macOS上執行的AEMas a Cloud ServiceSDK擷取。 除非另有說明，否則命令和代碼獨立於本地作業系統。

**AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)。

**AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 下載並部署WKND參考站點 {#set-up-wknd-site}

本教學課程會擴充WKND參考網站中的署名元件。 將WKND程式碼基底原地複製並安裝至本機環境。

1. 啟動本地快速入門 **作者** AEM例項在執行 [http://localhost:4502](http://localhost:4502).
1. 開啟終端機視窗，並使用Git複製WKND程式碼基底：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 將WKND程式碼基底部署至AEM的本機執行個體：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5和最新的Service Pack，請新增 `classic` 配置檔案到Maven命令：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 開啟新的瀏覽器視窗並登入AEM。 開啟 **雜誌** 頁面如下： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![頁面上的署名元件](assets/adobe-client-data-layer/byline-component-onpage.png)

   您應該會看到已新增至頁面作為體驗片段一部分的署名元件範例。 您可以在檢視體驗片段 [http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)
1. 開啟開發人員工具，並在 **主控台**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect回應，可查看AEM網站上資料層的目前狀態。 您應該會看到頁面和個別元件的相關資訊。

   ![Adobe資料層回應](assets/data-layer-state-response.png)

   請注意，資料層中未列出署名元件。

## 更新署名Sling模型 {#sling-model}

若要在資料層中插入元件相關資料，必須先更新元件的Sling模型。 接下來，更新Byline的Java介面和Sling Model實作，以新增方法 `getData()`. 此方法將包含我們要插入資料層的屬性。

1. 在所選IDE中，開啟 `aem-guides-wknd` 專案。 導覽至 `core` 模組。
1. 開啟檔案 `Byline.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.

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

1. 開啟檔案 `BylineImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

   這是 `Byline` 介面，並以Sling模型實作。

1. 將下列匯入陳述式新增至檔案的開頭：

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   此 `fasterxml.jackson` API可用來序列化要公開為JSON的資料。 此 `ComponentUtils` 的AEM核心元件，以檢查資料層是否已啟用。

1. 新增未實作的方法 `getData()` to `BylineImple.java`:

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

   在上述方法中， `HashMap` 可用來擷取要公開為JSON的屬性。 請注意，現有方法如 `getName()` 和 `getOccupations()` 中所有規則的URL區段。 `@type` 代表元件的唯一資源類型，這可讓用戶端根據元件類型輕鬆識別事件和/或觸發器。

   此 `ObjectMapper` 可用來序列化屬性並傳回JSON字串。 然後，此JSON字串可插入資料層。

1. 開啟終端窗口。 只建置和部署 `core` 模組使用Maven技能：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 更新署名HTL {#htl}

接下來，更新 `Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl). HTL(HTML範本語言)是用來轉譯元件HTML的範本。

特殊資料屬性 `data-cmp-data-layer` 在每個AEM元件上，用來公開其資料層。  AEM核心元件提供的JavaScript會尋找此資料屬性，其值會填入由署名Sling模型傳回的JSON字串 `getData()` 方法，並將值插入Adobe用戶端資料層。

1. 在IDE中，開啟 `aem-guides-wknd` 專案。 導覽至 `ui.apps` 模組。
1. 開啟檔案 `byline.html` at `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

   ![署名HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. 更新 `byline.html` 包括 `data-cmp-data-layer` 屬性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   的值 `data-cmp-data-layer` 設為 `"${byline.data}"` where `byline` 是先前更新的Sling模型。 `.data` 是在HTL中呼叫Java Getter方法的標準標籤法 `getData()` 執行。

1. 開啟終端窗口。 只建置和部署 `ui.apps` 模組使用Maven技能：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並使用署名元件重新開啟頁面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 開啟開發人員工具，並檢查Byline元件的頁面HTML來源：

   ![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

   您應該會看到 `data-cmp-data-layer` 已填入Sling模型的JSON字串。

1. 開啟瀏覽器的開發人員工具，並在 **主控台**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在回應下方導覽 `component` 若要尋找 `byline` 元件已新增至資料層：

   ![資料層的署名部分](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   您應會看到下列項目：

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   請注意，公開的屬性與 `HashMap` 在Sling模型中。

## 新增點按事件 {#click-event}

Adobe用戶端資料層由事件驅動，觸發動作的最常見事件之一是 `cmp:click` 事件。 AEM核心元件可讓您在資料元素的協助下，輕鬆註冊元件： `data-cmp-clickable`.

可點按的元素通常是CTA按鈕或導覽連結。 很可惜，署名元件沒有這些，但我們會以任何方式註冊，因為這可能是其他自訂元件的常見情況。

1. 開啟 `ui.apps` 模組
1. 開啟檔案 `byline.html` at `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`.

1. 更新 `byline.html` 包括 `data-cmp-clickable` 屬性 **名稱** 元素：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 開啟新的終端。 只建置和部署 `ui.apps` 模組使用Maven技能：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並重新開啟已新增Byline元件的頁面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).

   若要測試事件，我們將使用開發人員主控台手動新增一些JavaScript。 請參閱 [搭配AEM核心元件使用Adobe用戶端資料層](data-layer-overview.md) 如何做的視頻。

1. 開啟瀏覽器的開發人員工具，並在 **主控台**:

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   此簡單方法應處理按一下署名元件的名稱。

1. 在 **主控台**:

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上述方法會將事件接聽程式推送至資料層，以接聽 `cmp:click` 事件和呼叫 `bylineClickHandler`.

   >[!CAUTION]
   >
   > 這很重要 **not** 在本練習中重新整理瀏覽器，否則主控台JavaScript會遺失。

1. 在瀏覽器中，使用 **主控台** 開啟，在署名元件中按一下作者的名稱：

   ![已點按署名元件](assets/adobe-client-data-layer/byline-component-clicked.png)

   您應該會看到主控台訊息 `Byline Clicked!` 署名。

   此 `cmp:click` 事件是最容易勾搭的。 若要了解更複雜的元件並追蹤其他行為，可以新增自訂javascript以新增和註冊新事件。 輪播元件就是絕佳的範例，它會觸發 `cmp:show` 切換投影片時的事件。 請參閱 [詳細資訊的原始碼](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219).

## 使用DataLayerBuilder公用程式 {#data-layer-builder}

Sling模型是 [更新](#sling-model) 在章節前面，我們選擇使用 `HashMap` 並手動設定每個屬性。 此方法適用於小型一次性元件，但若是擴充AEM核心元件的元件，則可能會產生大量額外程式碼。

一個實用類， `DataLayerBuilder`，以執行大部分的重力提升。 這可讓實施只擴充其所需的屬性。 更新Sling模型以使用 `DataLayerBuilder`.

1. 返回IDE並導航到 `core` 模組。
1. 開啟檔案 `Byline.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`.
1. 修改 `getData()` 返回類型的方法 `ComponentData`

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

   `ComponentData` 是AEM核心元件提供的物件。 這會產生JSON字串，如同上一個範例，但也會執行許多額外工作。

1. 開啟檔案 `BylineImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`.

1. 新增下列匯入陳述式：

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. 取代 `getData()` 方法，並搭配下列項目：

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

   署名元件會重複使用影像核心元件的部分，以顯示代表作者的影像。 在上述程式碼片段中， [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html) 用於擴充 `Image` 元件。 這會以所使用影像的所有相關資料預先填入JSON物件。 它也會執行一些常式功能，例如設定 `@type` 和元件的唯一識別碼。 請注意，方法非常小！

   唯一的屬性會將 `withTitle` 會以 `getName()`.

1. 開啟終端窗口。 只建置和部署 `core` 模組使用Maven技能：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. 返回到IDE並開啟 `byline.html` 檔案 `ui.apps`
1. 更新HTL以使用 `byline.data.json` 填入 `data-cmp-data-layer` 屬性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   請記住，我們現在傳回的是類型的物件 `ComponentData`. 此對象包括getter方法 `getJson()` 而這是用來填入 `data-cmp-data-layer` 屬性。

1. 開啟終端窗口。 只建置和部署 `ui.apps` 模組使用Maven技能：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並重新開啟已新增Byline元件的頁面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html).
1. 開啟瀏覽器的開發人員工具，並在 **主控台**:

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在回應下方導覽 `component` 若要尋找 `byline` 元件：

   ![已更新署名資料層](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   您應會看到下列項目：

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

   請注意，現在有 `image` 物件 `byline` 元件項目。 這裡有更多關於DAM中資產的資訊。 另請注意， `@type` 和唯一id(在此例中為 `byline-136073cfcb`)，以及 `repo:modifyDate` 表示元件修改的時間。

## 其他範例 {#additional-examples}

1. 延伸資料層的另一個範例可透過檢查 `ImageList` 元件（在WKND代碼庫中）:
   * `ImageList.java`  — 中的Java介面 `core` 模組。
   * `ImageListImpl.java`  — 中的Sling模型 `core` 模組。
   * `image-list.html`  — 中的HTL範本 `ui.apps` 模組。

   >[!NOTE]
   >
   > 要包含自訂屬性(例如 `occupation` 使用 [DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html). 不過，如果擴充包含影像或頁面的核心元件，公用程式可節省大量時間。

   >[!NOTE]
   >
   > 如果針對在整個實施中重複使用的物件建立進階資料層，建議將資料層元素擷取至其自己的資料層專用的Java物件。 例如，商務核心元件已新增 `ProductData` 和 `CategoryData` 因為這些可用於商務實施中的許多元件。 檢閱 [aem-cif-core-components repo中的程式碼](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer) 以取得更多詳細資訊。

## 恭喜！ {#congratulations}

您剛剛探索了幾種使用AEM元件擴充和自訂Adobe用戶端資料層的方式！

## 其他資源 {#additional-resources}

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [與核心元件整合資料層](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [使用Adobe用戶端資料層與核心元件檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
