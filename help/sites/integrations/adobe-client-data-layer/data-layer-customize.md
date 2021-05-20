---
title: 使用AEM元件自訂Adobe用戶端資料層
description: 了解如何使用自訂AEM元件的內容來自訂Adobe用戶端資料層。 了解如何使用AEM核心元件提供的API來擴充和自訂資料層。
feature: Adobe用戶端資料層，核心元件
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
topic: Integrations
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 3%

---


# 使用AEM元件{#customize-data-layer}自訂Adobe用戶端資料層

了解如何使用自訂AEM元件的內容來自訂Adobe用戶端資料層。 了解如何使用[AEM核心元件提供的API來擴充](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)和自訂資料層。

## 您將建置的

![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

在本教程中，您將通過更新WKND [Byline元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html)來探索擴展Adobe客戶端資料層的各種選項。 這是自訂元件，本教學課程中的經驗教訓可套用至其他自訂元件。

### 目標 {#objective}

1. 延伸Sling模型和元件HTL，將元件資料插入資料層
1. 使用核心元件資料層公用程式來減輕工作量
1. 使用核心元件資料屬性連結至現有的資料層事件

## 必備條件 {#prerequisites}

完成本教學課程需要&#x200B;**本機開發環境**。 螢幕擷取畫面和影片是使用AEM作為在macOS上執行的Cloud ServiceSDK來擷取。 除非另有說明，否則命令和代碼獨立於本地作業系統。

**AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://docs.adobe.com/content/help/zh-Hant/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://docs.adobe.com/content/help/zh-Hant/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 下載並部署WKND參考站點{#set-up-wknd-site}

本教學課程會擴充WKND參考網站中的署名元件。 將WKND程式碼基底原地複製並安裝至本機環境。

1. 啟動運行在[http://localhost:4502](http://localhost:4502)的AEM的本地Quickstart **author**&#x200B;實例。
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
   > 如果使用AEM 6.5和最新的Service Pack，請將`classic`設定檔新增至Maven命令：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 開啟新的瀏覽器視窗並登入AEM。 開啟&#x200B;**雜誌**&#x200B;頁面，如下所示：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![頁面上的署名元件](assets/adobe-client-data-layer/byline-component-onpage.png)

   您應該會看到已新增至頁面作為體驗片段一部分的署名元件範例。 您可以在[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)檢視體驗片段
1. 開啟開發人員工具，並在&#x200B;**主控台**&#x200B;中輸入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect回應，可查看AEM網站上資料層的目前狀態。 您應該會看到頁面和個別元件的相關資訊。

   ![Adobe資料層回應](assets/data-layer-state-response.png)

   請注意，資料層中未列出署名元件。

## 更新署名Sling模型{#sling-model}

若要在資料層中插入元件相關資料，必須先更新元件的Sling模型。 接下來，更新Byline的Java介面和Sling Model實作，以新增新方法`getData()`。 此方法將包含我們要插入資料層的屬性。

1. 在所選IDE中，開啟`aem-guides-wknd`項目。 導覽至`core`模組。
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

   這是`Byline`介面的實作，並以Sling模型實作。

1. 將下列匯入陳述式新增至檔案的開頭：

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API將用於序列化要公開為JSON的資料。 AEM核心元件的`ComponentUtils`將用於檢查資料層是否已啟用。

1. 將未實現的方法`getData()`添加到`BylineImple.java`:

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

   在上述方法中，新`HashMap`用於擷取要公開為JSON的屬性。 請注意，已使用現有方法，例如`getName()`和`getOccupations()`。 `@type` 代表元件的唯一資源類型，這可讓用戶端根據元件類型輕鬆識別事件和/或觸發器。

   `ObjectMapper`用於序列化屬性並傳回JSON字串。 然後，此JSON字串可插入資料層。

1. 開啟終端窗口。 使用您的Maven技能，只建立並部署`core`模組：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 更新署名HTL {#htl}

接下來，更新`Byline` [ HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl)。 HTL（HTML範本語言）是用來轉譯元件HTML的範本。

每個AEM元件上的特殊資料屬性`data-cmp-data-layer`用於公開其資料層。  AEM核心元件提供的JavaScript會尋找此資料屬性，其值會填入由署名Sling模型的`getData()`方法傳回的JSON字串，並將值插入Adobe用戶端資料層。

1. 在IDE中，開啟`aem-guides-wknd`項目。 導覽至`ui.apps`模組。
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

   `data-cmp-data-layer`的值已設為`"${byline.data}"`，其中`byline`為先前更新的Sling模型。 `.data` 是在先前練習中實作的HTL中呼叫Java Getter方 `getData()` 法的標準標籤法。

1. 開啟終端窗口。 使用您的Maven技能，只建立並部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並使用署名元件重新開啟頁面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 開啟開發人員工具，並檢查Byline元件頁面的HTML來源：

   ![署名資料層](assets/adobe-client-data-layer/byline-data-layer-html.png)

   您應會看到`data-cmp-data-layer`已填入Sling模型的JSON字串。

1. 開啟瀏覽器的開發人員工具，並在&#x200B;**Console**&#x200B;中輸入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的回應下方導覽，以尋找已新增至資料層的`byline`元件例項：

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

   請注意，公開的屬性與Sling模型中`HashMap`中新增的屬性相同。

## 新增點按事件{#click-event}

Adobe用戶端資料層是事件驅動的，觸發動作的最常見事件之一是`cmp:click`事件。 AEM核心元件可讓您在資料元素的協助下，輕鬆註冊元件：`data-cmp-clickable`。

可點按的元素通常是CTA按鈕或導覽連結。 很可惜，署名元件沒有這些，但我們會以任何方式註冊，因為這可能是其他自訂元件的常見情況。

1. 在IDE中開啟`ui.apps`模組
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`開啟檔案`byline.html`。

1. 更新`byline.html`以在署名的&#x200B;**name**&#x200B;元素上包含`data-cmp-clickable`屬性：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 開啟新的終端。 使用您的Maven技能，只建立並部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並重新開啟已新增Byline元件的頁面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   若要測試事件，我們將使用開發人員主控台手動新增一些JavaScript。 如需如何執行此動作的影片，請參閱[搭配使用Adobe用戶端資料層與AEM核心元件](data-layer-overview.md) 。

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

   此簡單方法應處理按一下署名元件的名稱。

1. 在&#x200B;**控制台**&#x200B;中輸入以下方法：

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上述方法將事件接聽程式推送至資料層，以接聽`cmp:click`事件，並呼叫`bylineClickHandler`。

   >[!CAUTION]
   >
   > 在本練習中重新整理瀏覽器很重要，否則主控台JavaScript將會遺失。****

1. 在瀏覽器中，開啟&#x200B;**主控台**&#x200B;後，在署名元件中按一下作者的名稱：

   ![已點按署名元件](assets/adobe-client-data-layer/byline-component-clicked.png)

   您應該會看到主控台訊息`Byline Clicked!`和署名名稱。

   `cmp:click`事件是最容易連結的事件。 若要了解更複雜的元件並追蹤其他行為，可以新增自訂javascript以新增和註冊新事件。 旋轉切換元件就是絕佳範例，每當切換滑片時，就會觸發`cmp:show`事件。 如需詳細資訊，請參閱[原始碼](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219)。

## 使用DataLayerBuilder實用程式{#data-layer-builder}

當章節中較早的Sling模型為[更新](#sling-model)時，我們選擇使用`HashMap`並手動設定每個屬性，以建立JSON字串。 此方法適用於小型一次性元件，但若是擴充AEM核心元件的元件，則可能會產生大量額外程式碼。

`DataLayerBuilder`實用程式類存在，用於執行大部分的重力提升。 這可讓實施只擴充其所需的屬性。 讓我們更新Sling模型，以使用`DataLayerBuilder`。

1. 返回IDE並導航到`core`模組。
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

   `ComponentData` 是AEM核心元件提供的物件。這會產生JSON字串，如同上一個範例，但也會執行許多額外工作。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`開啟檔案`BylineImpl.java`。

1. 新增下列匯入陳述式：

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

   署名元件會重複使用影像核心元件的部分，以顯示代表作者的影像。 在上述程式碼片段中，[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)用於擴充`Image`元件的資料層。 這會以所使用影像的所有相關資料預先填入JSON物件。 它也會執行一些常式功能，例如設定元件的`@type`和唯一識別碼。 請注意，方法非常小！

   唯一的屬性擴展了`withTitle`，該被`getName()`值替換。

1. 開啟終端窗口。 使用您的Maven技能，只建立並部署`core`模組：

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

   請記住，我們現在傳回`ComponentData`類型的物件。 此對象包括getter方法`getJson()`，用於填充`data-cmp-data-layer`屬性。

1. 開啟終端窗口。 使用您的Maven技能，只建立並部署`ui.apps`模組：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回瀏覽器，並重新開啟已新增Byline元件的頁面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。
1. 開啟瀏覽器的開發人員工具，並在&#x200B;**Console**&#x200B;中輸入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的回應下方導覽，以尋找`byline`元件的例項：

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

   請注意，`byline`元件項目中現在有`image`物件。 這裡有更多關於DAM中資產的資訊。 另請注意，已自動填入`@type`和唯一ID（在此例中為`byline-136073cfcb`），以及指示修改元件時的`repo:modifyDate`。

## 其他範例{#additional-examples}

1. 在WKND程式碼基底中檢查`ImageList`元件，即可檢視延伸資料層的另一個範例：
   * `ImageList.java`  — 模組中的Java介 `core` 面。
   * `ImageListImpl.java`  — 模組中的Sling `core` 模型。
   * `image-list.html`  — 模組中的HTL `ui.apps` 範本。

   >[!NOTE]
   >
   > 使用[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)時，要包含`occupation`之類的自訂屬性較為困難。 不過，如果擴充包含影像或頁面的核心元件，公用程式可節省大量時間。

   >[!NOTE]
   >
   > 如果針對在整個實施中重複使用的物件建立進階資料層，建議將資料層元素擷取至其自己的資料層專用的Java物件。 例如，商務核心元件已新增`ProductData`和`CategoryData`的介面，因為這些介面可用於商務實施中的許多元件。 如需詳細資訊，請檢閱aem-cif-core-components repo](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)中的程式碼[。

## 恭喜！ {#congratulations}

您剛剛探索了幾種使用AEM元件擴充和自訂Adobe用戶端資料層的方式！

## 其他資源 {#additional-resources}

* [Adobe用戶端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [與核心元件整合資料層](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [使用Adobe用戶端資料層與核心元件檔案](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/data-layer/overview.html)
