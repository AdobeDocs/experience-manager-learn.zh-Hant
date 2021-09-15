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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1091'
ht-degree: 0%

---

# 擴充核心元件 {#extend-component}

了解如何擴充現有的核心元件以與AEM SPA編輯器搭配使用。 了解如何擴充現有元件是自訂和擴充AEM SPA Editor實作功能的強大技術。

## 目標

1. 使用其他屬性和內容擴充現有的核心元件。
2. 使用`sling:resourceSuperType`了解元件繼承的基本內容。
3. 了解如何針對Sling模型運用[委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models)來重複使用現有的邏輯和功能。

## 您將建置的

本章說明在標準`Image`元件中添加額外屬性以滿足新`Banner`元件要求所需的附加代碼。 `Banner`元件包含與標準`Image`元件相同的所有屬性，但包含一個附加屬性，供用戶填入&#x200B;**橫幅文字**。

![最終製作的橫幅元件](assets/extend-component/final-author-banner-component.png)

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。 在此階段，我們假設教學課程中的使用者已對AEM SPA Editor功能有完整的了解。

## 具有Sling資源超類型的繼承 {#sling-resource-super-type}

要在元件的定義上擴展現有元件集名為`sling:resourceSuperType`的屬性。  `sling:resourceSuperType`是屬 [](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) 性，可在指向其他元件的AEM元件定義上設定。這會明確設定元件，以繼承識別為`sling:resourceSuperType`的元件的所有功能。

如果要在`wknd-spa-react/components/image`擴展`Image`元件，則需要更新`ui.apps`模組中的代碼。

1. 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`的`ui.apps`模組下建立`banner`的新資料夾。
1. 在`banner`下方建立元件定義(`.content.xml`)，如下所示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   這設定`wknd-spa-react/components/banner`以繼承`wknd-spa-react/components/image`的所有功能。

## cq:editConfig {#cq-edit-config}

`_cq_editConfig.xml`檔案會指定AEM製作UI中的拖放行為。 擴充影像元件時，資源類型必須符合元件本身。

1. 在`ui.apps`模組中，在`banner`下建立名為`_cq_editConfig.xml`的另一個檔案。
1. 使用以下XML填入`_cq_editConfig.xml`:

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

   大多數元件不需要`_cq_editConfig`。 影像元件和子體是例外。

## 擴展對話框 {#extend-dialog}

我們的`Banner`元件需要對話方塊中的額外文字欄位來擷取`bannerText`。 由於我們使用Sling繼承，因此可以使用[Sling Resource Merger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html)的功能來覆寫或擴充對話方塊的部分。 在此範例中，對話方塊已新增一個索引標籤，以從作者擷取其他資料以填入「卡片元件」。

1. 在`ui.apps`模組的`banner`資料夾下方，建立名為`_cq_dialog`的資料夾。
1. 在`_cq_dialog`下方建立Dialog定義檔案`.content.xml`。 填入下列項目：

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

   上述XML定義將建立一個名為&#x200B;**Text**&#x200B;的新頁簽，並在&#x200B;*之前將其排序為*&#x200B;現有&#x200B;**Asset**&#x200B;頁簽。 它將包含單個欄位&#x200B;**橫幅文字**。

1. 對話方塊如下所示：

   ![橫幅最終對話方塊](assets/extend-component/banner-dialog.png)

   請注意，我們不必為&#x200B;**Asset**&#x200B;或&#x200B;**Metadata**&#x200B;定義索引標籤。 這些會透過`sling:resourceSuperType`屬性繼承。

   在可以預覽對話方塊之前，我們需要實作SPA元件和`MapTo`函式。

## 實作SPA元件 {#implement-spa-component}

若要搭配SPA編輯器使用Banner元件，必須建立新的SPA元件，以對應至`wknd-spa-react/components/banner`。 這將在`ui.frontend`模組中完成。

1. 在`ui.frontend`模組中，為`Banner`建立位於`ui.frontend/src/components/Banner`的新資料夾。
1. 在`Banner`資料夾下建立名為`Banner.js`的新檔案。 填入下列項目：

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

   此SPA元件映射至先前建立的AEM元件`wknd-spa-react/components/banner`。

1. 在`ui.frontend/src/components/import-components.js`更新`import-components.js`以包含新的`Banner` SPA元件：

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

1. 更新SPA範本的原則，將`Banner`元件新增為&#x200B;**允許的元件**。

1. 導覽至SPA頁面，並將`Banner`元件新增至其中一個SPA頁面：

   ![新增橫幅元件](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > 對話方塊可讓您儲存&#x200B;**橫幅文字**&#x200B;的值，但此值不會反映在SPA元件中。 若要啟用，我們需要擴充元件的Sling模型。

## 添加Java介面 {#java-interface}

若要最終將元件對話方塊中的值公開給React元件，我們需要更新填入`Banner`元件JSON的Sling模型。 這將在`core`模組中完成，該模組包含SPA專案的所有Java程式碼。

首先，我們將為`Banner`建立擴展`Image` Java介面的新Java介面。

1. 在`core`模組中，在`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`建立名為`BannerModel.java`的新檔案。
1. 將以下內容填入`BannerModel.java`:

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   這將繼承核心元件`Image`介面中的所有方法，並添加一個新方法`getBannerText()`。

## 實作Sling模型 {#sling-model}

接下來，為`BannerModel`介面實作Sling模型。

1. 在`core`模組中，在`core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`建立名為`BannerModelImpl.java`的新檔案。

1. 將以下內容填入`BannerModelImpl.java`:

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

   請注意使用`@Model`和`@Exporter`註解，以確保Sling模型能透過Sling模型匯出工具序列化為JSON。

   `BannerModelImpl.java` 使用Sling [模型的委派模](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 式，以避免從影像核心元件重寫所有邏輯。

1. 請觀察下列行：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述批注將根據`Banner`元件的`sling:resourceSuperType`繼承來實例化名為`image`的影像對象。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然後，只需使用`image`對象來實現由`Image`介面定義的方法，而無需自行編寫邏輯。 此技術用於`getSrc()`、`getAlt()`和`getTitle()`。

1. 開啟終端機視窗，使用`core`目錄中的Maven `autoInstallBundle`設定檔，只部署`core`模組的更新。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## 把它們放在一起 {#put-together}

1. 返回AEM並開啟具有`Banner`元件的SPA頁面。
1. 更新`Banner`元件以包含&#x200B;**橫幅文字**:

   ![橫幅文字](assets/extend-component/banner-text-dialog.png)

1. 將影像填入元件：

   ![將影像新增至橫幅對話方塊](assets/extend-component/banner-dialog-image.png)

   保存對話框更新。

1. 您現在應該會看到&#x200B;**橫幅文字**&#x200B;的呈現值：

   ![顯示的橫幅文字](assets/extend-component/banner-text-displayed.png)

1. 請在下列網址檢視JSON模型回應：[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)並搜尋`wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   請注意，在`BannerModelImpl.java`中實作Sling模型後，JSON模型已更新，並附加索引鍵/值組。

## 恭喜！ {#congratulations}

恭喜您，您已學會如何使用擴充AEM元件，以及Sling模型和對話方塊如何與JSON模型搭配運作。
