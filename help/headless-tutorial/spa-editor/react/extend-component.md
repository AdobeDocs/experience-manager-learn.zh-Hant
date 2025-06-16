---
title: 擴充核心元件 | AEM SPA Editor and React快速入門
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
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1058'
ht-degree: 0%

---

# 擴充核心元件 {#extend-component}

{{spa-editor-deprecation}}

瞭解如何擴充要與AEM SPA Editor搭配使用的現有核心元件。 瞭解如何擴充現有元件是一項強大的技術，可自訂和擴充AEM SPA Editor實作的功能。

## 目標

1. 使用其他屬性和內容擴充現有的核心元件。
2. 瞭解使用`sling:resourceSuperType`的元件繼承基本知識。
3. 瞭解如何為Sling模型善用[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)，以重複使用現有邏輯和功能。

## 您將建置的內容

本章說明將額外屬性新增至標準`Image`元件以滿足新`Banner`元件需求所需的額外程式碼。 `Banner`元件包含所有與標準`Image`元件相同的屬性，但包含使用者填入&#x200B;**橫幅文字**&#x200B;的額外屬性。

![最終編寫的橫幅元件](assets/extend-component/final-author-banner-component.png)

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。 在教學課程的這個階段，我們假設使用者已對AEM SPA Editor功能有深入的瞭解。

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

## cq：editConfig {#cq-edit-config}

`_cq_editConfig.xml`檔案指定AEM編寫UI中的拖放行為。 擴充影像元件時，資源型別必須符合元件本身。

1. 在`ui.apps`模組中，在`banner`下建立另一個名為`_cq_editConfig.xml`的檔案。
1. 使用下列XML填入`_cq_editConfig.xml`：

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

1. 檔案的唯一方面是將resourceType設定為`wknd-spa-react/components/banner`的`<parameters>`節點。

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大部分的元件不需要`_cq_editConfig`。 影像元件和後代則為例外。

## 延伸對話方塊 {#extend-dialog}

我們的`Banner`元件需要對話方塊中的額外文字欄位才能擷取`bannerText`。 由於我們使用Sling繼承，因此可以使用[Sling資源合併器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html?lang=zh-Hant)的功能來覆寫或擴充對話方塊的部分。 在此範例中，對話方塊中已新增索引標籤，以從作者擷取其他資料以填入卡片元件。

1. 在`ui.apps`模組的`banner`資料夾下方，建立名為`_cq_dialog`的資料夾。
1. 在`_cq_dialog`下建立對話方塊定義檔`.content.xml`。 請填入下列內容：

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

   上述XML定義將建立名為&#x200B;**Text**&#x200B;的新索引標籤，並在&#x200B;*現有&#x200B;**資產**&#x200B;索引標籤之前*&#x200B;對其進行排序。 它將包含單一欄位&#x200B;**橫幅文字**。

1. 對話方塊看起來像這樣：

   ![橫幅最終對話方塊](assets/extend-component/banner-dialog.png)

   請注意，我們不需要為&#x200B;**資產**&#x200B;或&#x200B;**中繼資料**&#x200B;定義索引標籤。 這些是透過`sling:resourceSuperType`屬性繼承的。

   在可以預覽對話方塊之前，我們需要實作SPA元件和`MapTo`函式。

## 實作SPA元件 {#implement-spa-component}

若要搭配SPA Editor使用Banner元件，必須建立將對應至`wknd-spa-react/components/banner`的新SPA元件。 這已在`ui.frontend`模組中完成。

1. 在`ui.frontend`模組中，於`ui.frontend/src/components/Banner`為`Banner`建立新資料夾。
1. 在`Banner`資料夾下建立名為`Banner.js`的新檔案。 請填入下列內容：

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

   此SPA元件對應至先前建立的AEM元件`wknd-spa-react/components/banner`。

1. 在`ui.frontend/src/components/import-components.js`更新`import-components.js`以包含新的`Banner` SPA元件：

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. 此時，可以將專案部署到AEM，並且可以測試對話方塊。 使用您的Maven技能部署專案：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 更新SPA範本的原則以將`Banner`元件新增為&#x200B;**允許的元件**。

1. 導覽至SPA頁面，並將`Banner`元件新增至其中一個SPA頁面：

   ![新增橫幅元件](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > 此對話方塊可讓您儲存&#x200B;**橫幅文字**&#x200B;的值，但此值不會反映在SPA元件中。 若要啟用，我們需要為元件延伸Sling模型。

## 新增Java介面 {#java-interface}

若要最終將元件對話方塊中的值公開給React元件，我們需要更新為`Banner`元件填入JSON的Sling模型。 這是在包含我們SPA專案所有Java程式碼的`core`模組中完成的。

首先，我們將為`Banner`建立擴充`Image` Java介面的新Java介面。

1. 在`core`模組中，於`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`建立名為`BannerModel.java`的新檔案。
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

   這會從核心元件`Image`介面繼承所有方法，並新增一個方法`getBannerText()`。

## 實施Sling模型 {#sling-model}

接下來，實作`BannerModel`介面的Sling模型。

1. 在`core`模組中，於`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`建立名為`BannerModelImpl.java`的新檔案。

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

   請注意，您使用了`@Model`和`@Exporter`註解，以確保Sling模型能夠透過Sling模型匯出工具序列化為JSON。

   `BannerModelImpl.java`對Sling模型使用[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)以避免重寫影像核心元件中的所有邏輯。

1. 複查下列各行：

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

## 整合所有內容 {#put-together}

1. 返回AEM並開啟具有`Banner`元件的SPA頁面。
1. 更新`Banner`元件以包含&#x200B;**橫幅文字**：

   ![橫幅文字](assets/extend-component/banner-text-dialog.png)

1. 將影像填入元件中：

   ![新增影像至橫幅對話方塊](assets/extend-component/banner-dialog-image.png)

   儲存對話方塊更新。

1. 您現在應該會看到&#x200B;**橫幅文字**&#x200B;的轉譯值：

![顯示的橫幅文字](assets/extend-component/banner-text-displayed.png)

1. 檢視JSON模型回應： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)並搜尋`wknd-spa-react/components/card`：

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
