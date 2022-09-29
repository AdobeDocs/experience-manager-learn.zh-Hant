---
title: 擴充核心元件 |開始使用AEM SPA Editor and React
description: 了解如何擴充現有核心元件的JSON模型以與AEM SPA編輯器搭配使用。 了解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。 了解如何使用委派模式來擴充Sling模型和Sling Resource Merger的功能。
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 0%

---

# 擴充核心元件 {#extend-component}

了解如何擴充現有的核心元件以與AEM SPA編輯器搭配使用。 了解如何擴充現有元件是自訂和擴充AEM SPA Editor實作功能的強大技術。

## 目標

1. 使用其他屬性和內容擴充現有的核心元件。
2. 使用了解元件繼承的基本內容 `sling:resourceSuperType`.
3. 了解如何運用 [委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) ，以重新使用現有的邏輯和功能。

## 您將建置的

本章說明新增額外屬性至標準所需的其他程式碼 `Image` 元件，以滿足新 `Banner` 元件。 此 `Banner` 元件包含與標準相同的所有屬性 `Image` 元件，但包含其他屬性，供使用者填入 **橫幅文字**.

![最終製作的橫幅元件](assets/extend-component/final-author-banner-component.png)

## 必備條件

檢閱設定 [本地開發環境](overview.md#local-dev-environment). 在此階段，我們假設教學課程中的使用者已對AEM SPA Editor功能有完整的了解。

## 具有Sling資源超類型的繼承 {#sling-resource-super-type}

要擴展現有元件集的屬性，名為 `sling:resourceSuperType` 元件的定義。  `sling:resourceSuperType`是 [屬性](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) 可在指向其他元件的AEM元件定義上設定。 這會明確設定元件，以繼承識別為 `sling:resourceSuperType`.

如果我們想將 `Image` 元件於 `wknd-spa-react/components/image` 我們需要更新 `ui.apps` 模組。

1. 在 `ui.apps` 模組 `banner` at `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. 下方 `banner` 建立元件定義(`.content.xml`)如下所示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   此集 `wknd-spa-react/components/banner` 要繼承 `wknd-spa-react/components/image`.

## cq:editConfig {#cq-edit-config}

此 `_cq_editConfig.xml` 檔案會指定AEM製作UI中的拖放行為。 擴充影像元件時，資源類型必須符合元件本身。

1. 在 `ui.apps` 模組在下方建立另一個檔案 `banner` 已命名 `_cq_editConfig.xml`.
1. 填入 `_cq_editConfig.xml` ，並搭配下列XML:

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

1. 檔案的唯一方面是 `<parameters>` 將resourceType設定為 `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大部分元件不需要 `_cq_editConfig`. 影像元件和子體是例外。

## 擴展對話框 {#extend-dialog}

我們的 `Banner` 元件需要對話方塊中的額外文字欄位來擷取 `bannerText`. 由於我們使用Sling繼承，因此可使用 [Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) 覆蓋或擴展對話框的部分。 在此範例中，對話方塊已新增一個索引標籤，以從作者擷取其他資料以填入「卡片元件」。

1. 在 `ui.apps` 模組，在 `banner` 資料夾，建立名為 `_cq_dialog`.
1. 下方 `_cq_dialog` 建立對話框定義檔案 `.content.xml`. 填入下列項目：

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

   上述XML定義將建立一個名為 **文字** 訂購 *befor* 現有 **資產** 標籤。 其中會包含單一欄位 **橫幅文字**.

1. 對話方塊如下所示：

   ![橫幅最終對話方塊](assets/extend-component/banner-dialog.png)

   請注意，我們不必為 **資產** 或 **中繼資料**. 這些會透過 `sling:resourceSuperType` 屬性。

   在可以預覽對話方塊之前，我們需要實作SPA元件和 `MapTo` 函式。

## 實作SPA元件 {#implement-spa-component}

若要搭配SPA編輯器使用Banner元件，必須建立新的SPA元件，以對應至 `wknd-spa-react/components/banner`. 這是在 `ui.frontend` 模組。

1. 在 `ui.frontend` 模組為 `Banner` at `ui.frontend/src/components/Banner`.
1. 建立名為 `Banner.js` 在下面 `Banner` 檔案夾。 填入下列項目：

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
           if(BannerEditConfig.isEmpty(this.props)) {
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

   此SPA元件會對應至AEM元件 `wknd-spa-react/components/banner` 先前建立。

1. 更新 `import-components.js` at `ui.frontend/src/components/import-components.js` 納入新 `Banner` SPA元件：

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. 此時，專案可部署至AEM，且可測試對話方塊。 使用您的Maven技能部署專案：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 更新SPA範本的原則以新增 `Banner` 元件作為 **允許的元件**.

1. 導覽至SPA頁面並新增 `Banner` 元件至其中一個SPA頁面：

   ![新增橫幅元件](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > 對話方塊可讓您儲存 **橫幅文字** 但此值不會反映在SPA元件中。 若要啟用，我們需要擴充元件的Sling模型。

## 添加Java介面 {#java-interface}

若要最終將元件對話方塊中的值公開給React元件，我們需要更新填入JSON的Sling模型 `Banner` 元件。 這是在 `core` 包含SPA專案所有Java程式碼的模組。

首先，我們將為 `Banner` 延伸 `Image` Java介面。

1. 在 `core` 模組建立名為 `BannerModel.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. 填入 `BannerModel.java` 並搭配下列項目：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   這會繼承核心元件的所有方法 `Image` 介面和添加新方法 `getBannerText()`.

## 實作Sling模型 {#sling-model}

接下來，為 `BannerModel` 介面。

1. 在 `core` 模組建立名為 `BannerModelImpl.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. 填入 `BannerModelImpl.java` 並搭配下列項目：

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

   請注意， `@Model` 和 `@Exporter` 註解以確保Sling模型能透過Sling模型匯出工具序列化為JSON。

   `BannerModelImpl.java` 使用 [Sling模型的委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 以避免從影像核心元件重寫所有邏輯。

1. 請觀察下列行：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述註解將實例化名為的影像物件 `image` 根據 `sling:resourceSuperType` 繼承 `Banner` 元件。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   接著，您就可以直接使用 `image` 物件以實作由定義的方法 `Image` 介面，而無須自行編寫邏輯。 此技術用於 `getSrc()`, `getAlt()` 和 `getTitle()`.

1. 開啟終端機視窗，並只將更新部署至 `core` 使用Maven的模組 `autoInstallBundle` 從 `core` 目錄。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## 把它們放在一起 {#put-together}

1. 返回AEM並開啟具有的SPA頁面 `Banner` 元件。
1. 更新 `Banner` 包含元件 **橫幅文字**:

   ![橫幅文字](assets/extend-component/banner-text-dialog.png)

1. 將影像填入元件：

   ![將影像新增至橫幅對話方塊](assets/extend-component/banner-dialog-image.png)

   保存對話框更新。

1. 您現在應會看到 **橫幅文字**:

   ![顯示的橫幅文字](assets/extend-component/banner-text-displayed.png)

1. 請在下列網址檢視JSON模型回應： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 並搜尋 `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   請注意，在中實作Sling模型後，JSON模型會以其他索引鍵/值組更新 `BannerModelImpl.java`.

## 恭喜！ {#congratulations}

恭喜您，您已學會如何使用擴充AEM元件，以及Sling模型和對話方塊如何與JSON模型搭配運作。
