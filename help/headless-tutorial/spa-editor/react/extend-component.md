---
title: 擴充核心元件| AEM SPA Editor and React快速入門
description: 瞭解如何為要與AEM SPA編輯器搭配使用的現有核心元件擴充JSON模型。 瞭解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。 瞭解如何使用委派模式來延伸Sling模型和Sling Resource Merger的功能。
feature: SPA Editor, Core Components
version: Experience Manager as a Cloud Service
jira: KT-5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
duration: 316
hide: true
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 5%

---

# 擴充核心元件 {#extend-component}

{{spa-editor-deprecation}}

瞭解如何擴充要與AEM SPA Editor搭配使用的現有核心元件。 瞭解如何擴充現有元件是一項強大的技術，可自訂和擴充AEM SPA Editor實作的功能。

## 目標

1. 使用其他屬性和內容擴充現有的核心元件。
2. 瞭解使用`sling:resourceSuperType`的元件繼承基本知識。
3. 瞭解如何為Sling模型善用[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)，以重複使用現有邏輯和功能。

## 您將要建置的內容

本章說明將額外屬性新增至標準`Image`元件以滿足新`Banner`元件需求所需的額外程式碼。 `Banner`元件包含所有與標準`Image`元件相同的屬性，但包含使用者填入&#x200B;**橫幅文字**&#x200B;的額外屬性。

![最終編寫的橫幅元件](assets/extend-component/final-author-banner-component.png)

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具與指示。 在教學課程的這個階段，我們假設使用者已對AEM SPA Editor功能有深入的瞭解。

## Sling資源超級型別的繼承 {#sling-resource-super-type}

若要擴充現有元件，請在元件的定義上設定名為`sling:resourceSuperType`的屬性。  `sling:resourceSuperType`是可設定在指向其他元件之AEM元件定義上的[屬性](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties)。 這會明確設定元件以繼承識別為`sling:resourceSuperType`之元件的所有功能。

如果我們想要在`wknd-spa-react/components/image`擴充`Image`元件，則需要更新`ui.apps`模組中的程式碼。

1. 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`的`banner`的`ui.apps`模組下建立新資料夾。
1. 在`banner`下建立元件定義(`.content.xml`)，如下所示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   此設定`wknd-spa-react/components/banner`以繼承`wknd-spa-react/components/image`的所有功能。

## cq:editConfig {#cq-edit-config}

The `_cq_editConfig.xml` file dictates the drag and drop behavior in the AEM authoring UI. When extending the Image component it is important that the resource type matches the component itself.

1. In the `ui.apps` module create another file beneath `banner` named `_cq_editConfig.xml`.
1. Populate `_cq_editConfig.xml` with the following XML:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. The unique aspect of the file is the `<parameters>` node that sets the resourceType to `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   Most component&#39;s do not require a `_cq_editConfig`. Image components and descendants are the exception.

## Extend the Dialog {#extend-dialog}

Our `Banner` component requires an extra text field in the dialog to capture the `bannerText`. Since we are using Sling inheritance, we can use features of the [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) to override or extend portions of the dialog. In this sample a new tab has been added to the dialog to capture additional data from an author to populate the Card Component.

1. In the `ui.apps` module, beneath the `banner` folder, create a folder named `_cq_dialog`.
1. Beneath `_cq_dialog` create a Dialog definition file `.content.xml`. Populate it with the following:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   The above XML definition will create a new tab named **Text** and order it *before* the existing **Asset** tab. It will contain a single field **Banner Text**.

1. The dialog will look like the following:

   ![Banner final dialog](assets/extend-component/banner-dialog.png)

   Observe that we did not have to define the tabs for **Asset** or **Metadata**. These are inherited via the `sling:resourceSuperType` property.

   Before we can preview the dialog, we need to implement the SPA Component and the `MapTo` function.

## Implement SPA Component {#implement-spa-component}

In order to use the Banner component with the SPA Editor, a new SPA component must be created that will map to `wknd-spa-react/components/banner`. This is done in the `ui.frontend` module.

1. In the `ui.frontend` module create a new folder for `Banner` at `ui.frontend/src/components/Banner`.
1. 在`Banner`資料夾下建立名為`Banner.js`的新檔案。 Populate it with the following:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if (BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   This SPA component maps to the AEM component `wknd-spa-react/components/banner` created earlier.

1. Update `import-components.js` at `ui.frontend/src/components/import-components.js` to include the new `Banner` SPA component:

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. At this point the project can be deployed to AEM and the dialog can be tested. Deploy the project using your Maven skills:

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. Update the SPA Template&#39;s policy to add the `Banner` component as an **allowed component**.

1. Navigate to a SPA page and add the `Banner` component to one of the SPA pages:

   ![Add Banner component](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > The dialog will allow you to save a value for **Banner Text** but this value is not reflected in the SPA component. To enable, we need to extend the Sling Model for the component.

## Add Java Interface {#java-interface}

To ultimately expose the values from the component dialog to the React component we need to update the Sling Model that populates the JSON for the `Banner` component. This is done in the `core` module that contains all of the Java code for our SPA project.

First we will create a new Java interface for `Banner` that extends the `Image` Java interface.

1. In the `core` module create a new file named `BannerModel.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. 以下列專案填入`BannerModel.java`：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   This will inherit all of the methods from the Core Component `Image` interface and add one new method `getBannerText()`.

## Implement Sling Model {#sling-model}

Next, implement the Sling Model for the `BannerModel` interface.

1. In the `core` module create a new file named `BannerModelImpl.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. 以下列專案填入`BannerModelImpl.java`：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   }
   ```

   Notice the use of the `@Model` and `@Exporter` annotations to ensure the Sling Model is able to be serialized as JSON via the Sling Model Exporter.

   `BannerModelImpl.java` uses the [Delegation pattern for Sling Models](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) to avoid rewriting all of the logic from the Image core component.

1. Review the following lines:

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述註解會根據`Banner`元件的`sling:resourceSuperType`繼承將名為`image`的影像物件具現化。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然後就可以直接使用`image`物件來實作`Image`介面定義的方法，而不需要自行撰寫邏輯。 此技巧用於`getSrc()`、`getAlt()`和`getTitle()`。

1. 開啟終端機視窗，並使用`core`目錄中的Maven `autoInstallBundle`設定檔僅部署`core`模組的更新。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## 整合 {#put-together}

1. 返回AEM並開啟具有`Banner`元件的SPA頁面。
1. 更新`Banner`元件以包含&#x200B;**橫幅文字**：

   ![橫幅文字](assets/extend-component/banner-text-dialog.png)

1. 將影像填入元件中：

   ![新增影像至橫幅對話方塊](assets/extend-component/banner-dialog-image.png)

   儲存對話方塊更新。

1. 您現在應該會看到&#x200B;**橫幅文字**&#x200B;的轉譯值：

![顯示的橫幅文字](assets/extend-component/banner-text-displayed.png)

1. 檢視JSON模型回應： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)，並搜尋`wknd-spa-react/components/card`：

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   請注意，在`BannerModelImpl.java`中實作Sling模型後，JSON模型已更新為其他索引鍵/值組。

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何使用擴充AEM元件，以及Sling模型和對話方塊如何搭配JSON模型使用。
