---
title: 建立自訂天氣元件 | AEM SPA Editor and React快速入門
description: 瞭解如何建立要與AEM SPA Editor搭配使用的自訂天氣元件。 瞭解如何開發作者對話方塊和Sling模型，以擴充JSON模型來填入自訂元件。 系統會使用Open Weather API和React Open Weather元件。
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5878
thumbnail: 5878-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 82466e0e-b573-440d-b806-920f3585b638
duration: 323
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1105'
ht-degree: 0%

---

# 建立自訂天氣元件 {#custom-component}

{{spa-editor-deprecation}}

瞭解如何建立要與AEM SPA Editor搭配使用的自訂天氣元件。 瞭解如何開發作者對話方塊和Sling模型，以擴充JSON模型來填入自訂元件。 已使用[Open Weather API](https://openweathermap.org)和[React Open Weather元件](https://www.npmjs.com/package/react-open-weather)。

## 目標

1. 瞭解Sling模型在操控AEM提供的JSON模型API方面的作用。
2. 瞭解如何建立新的AEM元件對話方塊。
3. 瞭解如何建立與SPA編輯器架構相容的&#x200B;**自訂** AEM元件。

## 您將建置的內容

建置簡單的天氣元件。 此元件可供內容作者新增至SPA。 使用AEM對話方塊，作者可以設定天氣的顯示位置。  此元件的實作說明建立與AEM SPA Editor架構相容的全新AEM元件所需的步驟。

![設定開放天氣元件](assets/custom-component/enter-dialog.png)

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。 本章是[導覽及路由](navigation-routing.md)章節的延續，但您只需要將已啟用SPA的AEM專案部署至本機AEM執行個體即可。

### 開放氣象API金鑰

需要來自[Open Weather](https://openweathermap.org/)的API金鑰以及教學課程。 [免費註冊](https://home.openweathermap.org/users/sign_up)有限數量的API呼叫。

## 定義AEM元件

AEM元件定義為節點和屬性。 在專案中，這些節點和屬性在`ui.apps`模組中表示為XML檔案。 接下來，在`ui.apps`模組中建立AEM元件。

>[!NOTE]
>
> 快速重新整理AEM元件的[基本知識可能會有幫助](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html)。

1. 在您選擇的IDE中，開啟`ui.apps`資料夾。
2. 瀏覽至`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components`並建立名為`open-weather`的新資料夾。
3. 在`open-weather`資料夾下建立名為`.content.xml`的新檔案。 以下列專案填入`open-weather/.content.xml`：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Open Weather"
       componentGroup="WKND SPA React - Content"/>
   ```

   ![建立自訂元件定義](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` — 識別此節點是AEM元件。

   `jcr:title`是顯示給內容作者的值，且`componentGroup`會決定編寫UI中的元件分組。

4. 在`custom-component`資料夾下，建立另一個名為`_cq_dialog`的資料夾。
5. 在`_cq_dialog`資料夾下建立名為`.content.xml`的新檔案，並填入下列內容：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Open Weather"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
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
                                               <label
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The label to display for the component"
                                                   fieldLabel="Label"
                                                   name="./label"/>
                                               <lat
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The latitude of the location."
                                                   fieldLabel="Latitude"
                                                   step="any"
                                                   name="./lat" />
                                               <lon
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/numberfield"
                                                   fieldDescription="The longitude of the location."
                                                   fieldLabel="Longitude"
                                                   step="any"
                                                   name="./lon"/>
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

   ![自訂元件定義](assets/custom-component/dialog-custom-component-defintion.png)

   上述XML檔案為`Weather Component`產生非常簡單的對話方塊。 檔案的重要部分是內部`<label>`、`<lat>`和`<lon>`節點。 此對話方塊包含兩個`numberfield`和一個`textfield`，可讓使用者設定要顯示的天氣。

   已建立Sling模型，以透過JSON模型公開`label`、`lat`和`long`屬性的值。

   >[!NOTE]
   >
   > 您可以檢視核心元件定義](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)，以檢視更多[對話方塊範例。 您也可以檢視[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)中`/libs/granite/ui/components/coral/foundation/form`下方可用的其他表單欄位，例如`select`、`textarea`、`pathfield`。

   使用傳統AEM元件時，通常需要[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html)指令碼。 由於SPA會轉譯元件，因此不需要HTL指令碼。

## 建立Sling模型

Sling模型是註釋驅動的Java「POJO」（純舊的Java物件），可方便將資料從JCR對應至Java變數。 [Sling模型](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html?lang=en#sling-models)通常可封裝AEM元件的複雜伺服器端商業邏輯。

在SPA編輯器的內容中，Sling模型使用[Sling模型匯出工具](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)，透過JSON模型透過功能公開元件的內容。

1. 在您選擇的IDE中，開啟位於`aem-guides-wknd-spa.react/core`的`core`模組。
1. 在`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`的`OpenWeatherModel.java`處建立名為的檔案。
1. 以下列專案填入`OpenWeatherModel.java`：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.export.json.ComponentExporter;
   
   // Sling Models intended to be used with SPA Editor must extend ComponentExporter interface
   public interface OpenWeatherModel extends ComponentExporter {
       public String getLabel();
       public double getLat();
       public double getLon();
   }
   ```

   這是元件的Java介面。 為了讓Sling模型與SPA Editor架構相容，它必須擴充`ComponentExporter`類別。

1. 在`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`下建立名為`impl`的資料夾。
1. 在`impl`下建立名為`OpenWeatherModelImpl.java`的檔案，並填入下列專案：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import com.adobe.aem.guides.wkndspa.react.core.models.OpenWeatherModel;
   
   // Sling Model annotation
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { OpenWeatherModel.class, ComponentExporter.class }, 
       resourceType = OpenWeatherModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter( //Exporter annotation that serializes the modoel as JSON
       name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
       extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class OpenWeatherModelImpl implements OpenWeatherModel {
   
       @ValueMapValue
       private String label; //maps variable to jcr property named "label" persisted by Dialog
   
       @ValueMapValue
       private double lat; //maps variable to jcr property named "lat"
   
       @ValueMapValue
       private double lon; //maps variable to jcr property named "lon"
   
       // points to AEM component definition in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/open-weather";
   
       // public getter method to expose value of private variable `label`
       // adds additional logic to default the label to "(Default)" if not set.
       @Override
       public String getLabel() {
           return StringUtils.isNotBlank(label) ? label : "(Default)";
       }
   
       // public getter method to expose value of private variable `lat`
       @Override
       public double getLat() {
           return lat;
       }
   
       // public getter method to expose value of private variable `lon`
       @Override
       public double getLon() {
           return lon;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/open-weather`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return OpenWeatherModelImpl.RESOURCE_TYPE;
       }
   } 
   ```

   靜態變數`RESOURCE_TYPE`必須指向元件`ui.apps`中的路徑。 `getExportedType()`是用來透過`MapTo`將JSON屬性對應至SPA元件。 `@ValueMapValue`是讀取對話方塊儲存之jcr屬性的註解。

## 更新SPA

接下來，更新React程式碼以包含[React Open Weather元件](https://www.npmjs.com/package/react-open-weather)，並使其對應至在先前步驟中建立的AEM元件。

1. 將React Open Weather元件安裝為&#x200B;**npm**&#x200B;相依性：

   ```shell
   $ cd aem-guides-wknd-spa.react/ui.frontend
   $ npm i react-open-weather
   ```

1. 在`ui.frontend/src/components/OpenWeather`建立名為`OpenWeather`的新資料夾。
1. 新增名為`OpenWeather.js`的檔案，並填入下列內容：

   ```js
   import React from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   import ReactWeather, { useOpenWeather } from 'react-open-weather';
   
   // Open weather API Key
   // For simplicity it is hard coded in the file, ideally this is extracted in to an environment variable
   const API_KEY = 'YOUR_API_KEY';
   
   // Logic to render placeholder or component
   const OpenWeatherEditConfig = {
   
       emptyLabel: 'Weather',
       isEmpty: function(props) {
           return !props || !props.lat || !props.lon || !props.label;
       }
   };
   
   // Wrapper function that includes react-open-weather component
   function ReactWeatherWrapper(props) {
       const { data, isLoading, errorMessage } = useOpenWeather({
           key: API_KEY,
           lat: props.lat, // passed in from AEM JSON
           lon: props.lon, // passed in from AEM JSON
           lang: 'en',
           unit: 'imperial', // values are (metric, standard, imperial)
       });
   
       return (
           <div className="cmp-open-weather">
               <ReactWeather
                   isLoading={isLoading}
                   errorMessage={errorMessage}
                   data={data}
                   lang="en"
                   locationLabel={props.label} // passed in from AEM JSON
                   unitsLabels={{ temperature: 'F', windSpeed: 'mph' }}
                   showForecast={false}
                 />
           </div>
       );
   }
   
   export default function OpenWeather(props) {
   
           // render nothing if component not configured
           if (OpenWeatherEditConfig.isEmpty(props)) {
               return null;
           }
   
           // render ReactWeather component if component configured
           // pass props to ReactWeatherWrapper. These props include the mapped properties from AEM JSON
           return ReactWeatherWrapper(props);
   
   }
   
   // Map OpenWeather to AEM component
   MapTo('wknd-spa-react/components/open-weather')(OpenWeather, OpenWeatherEditConfig);
   ```

1. 在`ui.frontend/src/components/import-components.js`更新`import-components.js`以包含`OpenWeather`元件：

   ```diff
     // import-component.js
     import './Container/Container';
     import './ExperienceFragment/ExperienceFragment';
   + import './OpenWeather/OpenWeather';
   ```

1. 使用您的Maven技能，從專案目錄的根目錄將所有更新部署至本機AEM環境：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新範本原則

接下來，導覽至AEM以驗證更新，並允許將`OpenWeather`元件新增至SPA。

1. 請瀏覽至[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)，以驗證新Sling模型的註冊。

   ```plain
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl - wknd-spa-react/components/open-weather
   
   com.adobe.aem.guides.wkndspa.react.core.models.impl.OpenWeatherModelImpl exports 'wknd-spa-react/components/open-weather' with selector 'model' and extension '[Ljava.lang.String;@2fd80fc5' with exporter 'jackson'
   ```

   您應該會看到以上兩行，指出`OpenWeatherModelImpl`與`wknd-spa-react/components/open-weather`元件相關聯，並已透過Sling模型匯出工具註冊。

1. 導覽至[http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html)的SPA頁面範本。
1. 更新配置容器的原則以將新的`Open Weather`新增為允許的元件：

   ![更新配置容器原則](assets/custom-component/custom-component-allowed.png)

   儲存原則的變更，並將`Open Weather`視為允許的元件：

   ![自訂元件為允許的元件](assets/custom-component/custom-component-allowed-layout-container.png)

## 編寫開啟的天氣元件

接下來，使用AEM SPA編輯器編寫`Open Weather`元件。

1. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。
1. 在`Edit`模式中，將`Open Weather`新增至`Layout Container`：

   ![插入新元件](assets/custom-component/insert-custom-component.png)

1. 開啟元件的對話方塊並輸入&#x200B;**標籤**、**緯度**&#x200B;和&#x200B;**經度**。 例如&#x200B;**聖地亞哥**、**32.7157**&#x200B;和&#x200B;**-117.1611**。 西半球和南半球的數字會使用Open Weather API以負數表示

   ![設定開放天氣元件](assets/custom-component/enter-dialog.png)

   這是根據章節中先前的XML檔案建立的對話方塊。

1. 儲存變更。 請注意，**San Diego**&#x200B;的天氣現在已顯示：

   ![天氣元件已更新](assets/custom-component/weather-updated.png)

1. 導覽至[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)以檢視JSON模型。 搜尋`wknd-spa-react/components/open-weather`：

   ```json
   "open_weather": {
       "label": "San Diego",
       "lat": 32.7157,
       "lon": -117.1611,
       ":type": "wknd-spa-react/components/open-weather"
   }
   ```

   Sling模型會輸出JSON值。 這些JSON值會作為prop傳遞至React元件。

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何建立要與SPA編輯器搭配使用的自訂AEM元件。 您也學習了對話方塊、JCR屬性和Sling模型如何互動以輸出JSON模型。

### 後續步驟 {#next-steps}

[擴充核心元件](extend-component.md) — 瞭解如何擴充現有的AEM核心元件，以搭配AEM SPA Editor使用。 瞭解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。
