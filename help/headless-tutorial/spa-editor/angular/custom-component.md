---
title: 建立自訂元件 | AEM SPA Editor and Angular快速入門
description: 瞭解如何建立要與AEM SPA Editor搭配使用的自訂元件。 瞭解如何開發作者對話方塊和Sling模型，以擴充JSON模型來填入自訂元件。
feature: SPA Editor
version: Experience Manager as a Cloud Service
jira: KT-5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
duration: 308
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# 建立自訂元件 {#custom-component}

瞭解如何建立要與AEM SPA Editor搭配使用的自訂元件。 瞭解如何開發作者對話方塊和Sling模型，以擴充JSON模型來填入自訂元件。

## 目標

1. 瞭解Sling模型在操控AEM提供的JSON模型API方面的作用。
2. 瞭解如何建立AEM元件對話方塊。
3. 瞭解如何建立與SPA編輯器架構相容的&#x200B;**自訂** AEM元件。

## 您將建置的內容

前幾章的重點是開發SPA元件，並將它們對應至&#x200B;*現有*&#x200B;個AEM核心元件。 本章著重於如何建立及擴充&#x200B;*新*&#x200B;個AEM元件，以及如何操作AEM所提供的JSON模型。

簡單的`Custom Component`說明建立全新之AEM元件所需的步驟。

![訊息以全部大寫字顯示](assets/custom-component/message-displayed.png)

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。

### 取得程式碼

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. 使用Maven將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用[AEM 6.x](overview.md#compatibility)，請新增`classic`設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 安裝傳統[WKND參考站台](https://github.com/adobe/aem-guides-wknd/releases/latest)的完成套件。 由[WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest)提供的影像會在WKND SPA上重複使用。 可以使用[AEM的封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)來安裝封裝。

   ![封裝管理員安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution)上檢視完成的程式碼，或切換至分支`Angular/custom-component-solution`在本機簽出程式碼。

## 定義AEM元件

AEM元件定義為節點和屬性。 在專案中，這些節點和屬性在`ui.apps`模組中表示為XML檔案。 接下來，在`ui.apps`模組中建立AEM元件。

>[!NOTE]
>
> 快速重新整理AEM元件的[基本知識可能會有幫助](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html)。

1. 在您選擇的IDE中開啟`ui.apps`資料夾。
2. 瀏覽至`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components`並建立名為`custom-component`的資料夾。
3. 在`custom-component`資料夾下建立名為`.content.xml`的檔案。 以下列專案填入`custom-component/.content.xml`：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![建立自訂元件定義](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"` — 識別此節點是AEM元件。

   `jcr:title`是顯示給內容作者的值，且`componentGroup`會決定編寫UI中的元件分組。

4. 在`custom-component`資料夾下，建立另一個名為`_cq_dialog`的資料夾。
5. 在`_cq_dialog`資料夾下方建立名為`.content.xml`的檔案，並填入下列內容：

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

   上述XML檔案會為`Custom Component`產生簡單的對話方塊。 檔案的關鍵部分是內部`<message>`節點。 此對話方塊包含一個名為`Message`的簡單`textfield`，並將文字欄位的值保留到名為`message`的屬性。

   接著會建立Sling模型，以透過JSON模型公開`message`屬性的值。

   >[!NOTE]
   >
   > 您可以檢視核心元件定義[&#128279;](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)，以檢視更多對話方塊範例。 您也可以檢視[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)中`/libs/granite/ui/components/coral/foundation/form`下方可用的其他表單欄位，例如`select`、`textarea`、`pathfield`。

   使用傳統AEM元件時，通常需要[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html)指令碼。 由於SPA會轉譯元件，因此不需要HTL指令碼。

## 建立Sling模型

Sling模型是註釋驅動的Java™ 「POJO」(Plain Old Java™物件)，可方便將資料從JCR對應至Java™變數。 [Sling模型](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models)通常可封裝AEM元件的複雜伺服器端商業邏輯。

在SPA編輯器的內容中，Sling模型使用[Sling模型匯出工具](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)，透過JSON模型透過功能公開元件的內容。

1. 在您選擇的IDE中，開啟`core`模組。 `CustomComponent.java`和`CustomComponentImpl.java`已建立並作為章節起始程式碼的一部分進行存根。

   >[!NOTE]
   >
   > 如果使用Visual Studio Code IDE，則安裝Java™[&#128279;](https://code.visualstudio.com/docs/java/extensions)的擴充功能可能會有幫助。

2. 在`core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`開啟Java™介面`CustomComponent.java`：

   ![CustomComponent.java介面](assets/custom-component/custom-component-interface.png)

   這是由Sling模型實作的Java™介面。

3. 更新`CustomComponent.java`以擴充`ComponentExporter`介面：

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   實作`ComponentExporter`介面需要Sling模型才能由JSON模型API自動擷取。

   `CustomComponent`介麵包含單一getter方法`getMessage()`。 此方法會透過JSON模型公開作者對話方塊的值。 JSON模型中只會匯出具有空白引數`()`的getter方法。

4. 在`core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`開啟`CustomComponentImpl.java`。

   這是`CustomComponent`介面的實作。 `@Model`註解將Java™類別識別為Sling模型。 `@Exporter`註解可透過Sling模型匯出工具序列化及匯出Java™類別。

5. 更新靜態變數`RESOURCE_TYPE`以指向在上一個練習中建立的AEM元件`wknd-spa-angular/components/custom-component`。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   元件的資源型別會將Sling模型繫結至AEM元件，最終對應至Angular元件。

6. 將`getExportedType()`方法新增至`CustomComponentImpl`類別以傳回元件資源型別：

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   實作`ComponentExporter`介面並公開允許對應至Angular元件的資源型別時，需要此方法。

7. 更新`getMessage()`方法以傳回作者對話方塊儲存的`message`屬性值。 使用`@ValueMap`註解將JCR值`message`對應至Java™變數：

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

   新增一些額外的「商業邏輯」，以傳回訊息的值為大寫。 這可讓我們檢視製作對話方塊儲存的原始值與Sling模型公開的值之間的差異。

   >[!NOTE]
   >
   > 您可以在[&#128279;](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java)檢視已完成的CustomComponentImpl.java。

## 更新Angular元件

已建立自訂元件的Angular程式碼。 接下來，進行一些更新，以將Angular元件對應至AEM元件。

1. 在`ui.frontend`模組中，開啟檔案`ui.frontend/src/app/components/custom/custom.component.ts`
2. 觀察`@Input() message: string;`行。 轉換後的大寫值應對應至此變數。
3. 從AEM SPA Editor JS SDK匯入`MapTo`物件，並使用它來對應到AEM元件：

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 開啟`cutom.component.html`，並觀察到`{{message}}`的值顯示在`<h2>`標籤的一側。
5. 開啟`custom.component.css`並新增下列規則：

   ```css
   :host-context {
       display: block;
   }
   ```

   若要讓AEM編輯器預留位置在元件為空時正確顯示，`:host-context`或其他`<div>`需要設定為`display: block;`。

6. 使用您的Maven技能，從專案目錄的根將更新部署到本機AEM環境：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新範本原則

接下來，導覽至AEM以驗證更新，並允許將`Custom Component`新增至SPA。

1. 請瀏覽至[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)，以驗證新Sling模型的註冊。

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   您應該會看到以上兩行，指出`CustomComponentImpl`與`wknd-spa-angular/components/custom-component`元件相關聯，並已透過Sling模型匯出工具註冊。

2. 導覽至[http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)的SPA頁面範本。
3. 更新配置容器的原則以將新的`Custom Component`新增為允許的元件：

   ![更新配置容器原則](assets/custom-component/custom-component-allowed.png)

   儲存原則的變更，並將`Custom Component`視為允許的元件：

   ![自訂元件為允許的元件](assets/custom-component/custom-component-allowed-layout-container.png)

## 編寫自訂元件

接下來，使用AEM SPA編輯器編寫`Custom Component`。

1. 導覽至[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。
2. 在`Edit`模式中，將`Custom Component`新增至`Layout Container`：

   ![插入新元件](assets/custom-component/insert-custom-component.png)

3. 開啟元件的對話方塊，然後輸入包含一些小寫字母的訊息。

   ![設定自訂元件](assets/custom-component/enter-dialog-message.png)

   這是根據章節中先前的XML檔案建立的對話方塊。

4. 儲存變更。 請注意，顯示的訊息全部是大寫。

   ![訊息以全部大寫字顯示](assets/custom-component/message-displayed.png)

5. 導覽至[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)以檢視JSON模型。 搜尋`wknd-spa-angular/components/custom-component`：

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   請注意，根據Sling模型新增的邏輯，JSON值會設為所有大寫字母。

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何建立自訂AEM元件，以及Sling模型和對話方塊如何與JSON模型搭配運作。

您一律可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution)上檢視完成的程式碼，或切換至分支`Angular/custom-component-solution`在本機簽出程式碼。

### 後續步驟 {#next-steps}

[擴充核心元件](extend-component.md) — 瞭解如何擴充要與AEM SPA Editor搭配使用的現有核心元件。 瞭解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。
