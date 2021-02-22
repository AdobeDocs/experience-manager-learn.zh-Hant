---
title: 建立自訂元件 | AEM SPA編輯器與Angular快速入門
description: 瞭解如何建立要與AEM SPA編輯器搭配使用的自訂元件。 瞭解如何開發作者對話方塊和Sling Models以擴充JSON模型以填入自訂元件。
sub-product: sites
feature: SPA編輯器
doc-type: tutorial
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
translation-type: tm+mt
source-git-commit: 1fd4d31770a4eac37a88a7c6960fd51845601bee
workflow-type: tm+mt
source-wordcount: '1507'
ht-degree: 1%

---


# 建立自訂元件{#custom-component}

瞭解如何建立要與AEM SPA編輯器搭配使用的自訂元件。 瞭解如何開發作者對話方塊和Sling Models以擴充JSON模型以填入自訂元件。

## 目標

1. 瞭解Sling Models在控制AEM提供的JSON模型API中的角色。
2. 瞭解如何建立新的AEM元件對話方塊。
3. 瞭解如何建立與SPA編輯器架構相容的&#x200B;**自訂** AEM元件。

## 您將建立的

前幾章的重點是開發SPA元件，並將它們對應至&#x200B;*現有* AEM核心元件。 本章將著重說明如何建立和擴充&#x200B;*new* AEM元件，以及如何控制AEM提供的JSON模型。

簡單的`Custom Component`說明建立新AEM元件所需的步驟。

![顯示在「全部大寫」中的消息](assets/custom-component/message-displayed.png)

## 必備條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. 使用Maven將程式碼庫部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)新增`classic`描述檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 為傳統[WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)安裝完成的軟體包。 由[WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的影像將重新用於WKND SPA。 可使用[AEM的Package Manager](http://localhost:4502/crx/packmgr/index.jsp)安裝套件。

   ![Package Manager install wknd.all](./assets/map-components/package-manager-wknd-all.png)

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution)上檢視完成的程式碼，或切換至分支`Angular/custom-component-solution`，在本機檢出程式碼。

## 定義AEM元件

AEM元件定義為節點和屬性。 在項目中，這些節點和屬性在`ui.apps`模組中表示為XML檔案。 接著，在`ui.apps`模組中建立AEM元件。

>[!NOTE]
>
> 快速進一步瞭解AEM元件的[基本概念可能很有幫助。](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html)

1. 在您選擇的IDE中，開啟`ui.apps`資料夾。
2. 導覽至`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components`並建立名為`custom-component`的新資料夾。
3. 在`custom-component`資料夾下建立一個名為`.content.xml`的新檔案。 在`custom-component/.content.xml`中填入下列內容：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![建立自訂元件定義](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` -標識此節點將是AEM元件。

   `jcr:title` 是顯示給「內容作者」的值，並決 `componentGroup` 定製作UI中的元件群組。

4. 在`custom-component`資料夾下，建立名為`_cq_dialog`的另一個資料夾。
5. 在`_cq_dialog`資料夾下方，建立名為`.content.xml`的新檔案，並填入下列內容：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Custom Component"
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
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldDescription="The text to display on the component."
                                                   fieldLabel="Message"
                                                   name="./message"/>
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

   上述XML檔案會為`Custom Component`產生一個非常簡單的對話方塊。 檔案的關鍵部分是內部`<message>`節點。 此對話方塊將包含名為`Message`的簡單`textfield`，並將textifeld的值保留至名為`message`的屬性。

   Sling Model將會建立在旁邊，以透過JSON模型公開`message`屬性的值。

   >[!NOTE]
   >
   > 查看核心元件定義](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)可以查看更多[對話框示例。 您也可以檢視其他表格欄位，例如`select`、`textarea`、`pathfield`,`/libs/granite/ui/components/coral/foundation/form`下方的[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)。

   對於傳統AEM元件，通常需要[HTL](https://docs.adobe.com/content/help/zh-Hant/experience-manager-htl/using/overview.html)指令碼。 由於SPA會轉譯元件，因此不需要HTL指令碼。

## 建立Sling Model

Sling Models是註解導向的Java &quot;POJO&#39;s&quot;(Plain Old Java Objects)，可協助將資料從JCR對應至Java變數。 [Sling ](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/component-basics.html#sling-models) Models通常可用來封裝AEM元件的複雜伺服器端商業邏輯。

在SPA編輯器中，Sling Models會透過使用[Sling Model Exporter](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)的功能，透過JSON模型公開元件的內容。

1. 在您選擇的IDE中，開啟`core`模組。 `CustomComponent.java` 並 `CustomComponentImpl.java` 且已建立和研究過，作為章節起始代碼的一部分。

   >[!NOTE]
   >
   > 如果使用Visual Studio代碼IDE，則安裝Java](https://code.visualstudio.com/docs/java/extensions)的[副檔名可能會很有幫助。

2. 在`core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`處開啟Java介面`CustomComponent.java` :

   ![CustomComponent.java介面](assets/custom-component/custom-component-interface.png)

   這是Java介面，將由Sling Model實作。

3. 更新`CustomComponent.java`以擴展`ComponentExporter`介面：

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   實作`ComponentExporter`介面是JSON模型API自動擷取Sling Model的需求。

   `CustomComponent`介面包括單個getter方法`getMessage()`。 這是透過JSON模型公開作者對話方塊值的方法。 JSON模型中只會匯出具有空參數`()`的getter方法。

4. 在`core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`開啟`CustomComponentImpl.java`。

   這是`CustomComponent`介面的實現。 `@Model`附註會將Java類別識別為Sling Model。 `@Exporter`註解可讓Java類別透過Sling Model Exporter序列化和匯出。

5. 更新靜態變數`RESOURCE_TYPE`，以指向在上一個練習中建立的AEM元件`wknd-spa-angular/components/custom-component`。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   元件的資源類型是將Sling Model系結至AEM元件，最終將會映射至Angular元件的內容。

6. 將`getExportedType()`方法添加到`CustomComponentImpl`類以返回元件資源類型：

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   實施`ComponentExporter`介面時需要此方法，並將顯示允許映射到Angular元件的資源類型。

7. 更新`getMessage()`方法，以傳回作者對話方塊中持續存在的`message`屬性值。 使用`@ValueMap`注釋將JCR值`message`映射到Java變數：

   ```java
   import org.apache.commons.lang3.StringUtils;
   ...
   
   @ValueMapValue
   private String message;
   
   @Override
   public String getMessage() {
       return StringUtils.isNotBlank(message) ? message.toUpperCase() : null;
   }
   ```

   新增一些額外的「商業邏輯」，以傳回訊息的值為大寫。 這可讓我們查看作者對話方塊儲存的原始值與Sling Model公開的值之間的差異。

   >[!NOTE]
   >
   > 您可以在此處查看[完成的CustomComponentImpl.java。](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java)

## 更新角度元件

「自訂元件」的「角度」程式碼已建立。 接下來，進行一些更新，將Angular元件對應至AEM元件。

1. 在`ui.frontend`模組中開啟檔案`ui.frontend/src/app/components/custom/custom.component.ts`
2. 觀察`@Input() message: string;`行。 轉換後的大寫值應該會對應至此變數。
3. 從AEM SPA Editor JS SDK匯入`MapTo`物件，並使用它對應至AEM元件：

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 開啟`cutom.component.html`，並觀察`{{message}}`的值會顯示在`<h2>`標籤的旁邊。
5. 開啟`custom.component.css`並新增下列規則：

   ```css
   :host-context {
       display: block;
   }
   ```

   為了讓AEM Editor預留位置在元件為空時正確顯示，`:host-context`或其他`<div>`必須設為`display: block;`。

6. 使用您的Maven技能，從專案目錄的根目錄將所有更新部署至本機AEM環境：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新範本原則

接著，導覽至AEM以驗證更新並允許將`Custom Component`新增至SPA。

1. 導覽至[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)以驗證新Sling Model的註冊。

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   您應該會看到上述兩行，指出`CustomComponentImpl`與`wknd-spa-angular/components/custom-component`元件相關聯，且它已透過Sling Model Exporter註冊。

2. 導覽至位於[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)的SPA頁面範本。
3. 更新「配置容器」的原則，將新`Custom Component`新增為允許的元件：

   ![更新配置容器原則](assets/custom-component/custom-component-allowed.png)

   保存對策略的更改，並將`Custom Component`作為允許的元件進行觀察：

   ![自訂元件做為允許的元件](assets/custom-component/custom-component-allowed-layout-container.png)

## 編寫自訂元件

接著，使用AEM SPA編輯器編寫`Custom Component`。

1. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。
2. 在`Edit`模式中，將`Custom Component`新增至`Layout Container`:

   ![插入新元件](assets/custom-component/insert-custom-component.png)

3. 開啟元件的對話框，並輸入包含某些小寫字母的消息。

   ![設定自訂元件](assets/custom-component/enter-dialog-message.png)

   這是基於本章前面的XML檔案建立的對話框。

4. 儲存變更。請注意，顯示的訊息已全部加上大寫。

   ![顯示在「全部大寫」中的消息](assets/custom-component/message-displayed.png)

5. 導覽至[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)以檢視JSON模型。 搜尋 `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   請注意，JSON值會根據新增至Sling Model的邏輯，設為所有大寫字母。

## 恭喜！{#congratulations}

恭喜您，您學習了如何建立自訂AEM元件，以及Sling Models和對話方塊如何與JSON模型搭配運作。

您隨時都可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution)上檢視完成的程式碼，或切換至分支`Angular/custom-component-solution`，在本機檢出程式碼。

### 後續步驟{#next-steps}

[擴充核心元件](extend-component.md) -瞭解如何擴充現有的核心元件，以便與AEM SPA編輯器搭配使用。瞭解如何將屬性和內容新增至現有元件是擴充AEM SPA編輯器實作功能的強大技巧。
