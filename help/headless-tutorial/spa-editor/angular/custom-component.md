---
title: 建立自訂元件 |開始使用AEM SPA編輯器和Angular
description: 了解如何建立要與AEM SPA編輯器搭配使用的自訂元件。 了解如何開發製作對話方塊和Sling模型，以擴充JSON模型以填入自訂元件。
sub-product: sites
feature: SPA Editor
doc-type: tutorial
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 5831
thumbnail: 5831-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 6c1c7f2b-f574-458c-b744-b92419c46f23
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 1%

---

# 建立自訂元件 {#custom-component}

了解如何建立要與AEM SPA編輯器搭配使用的自訂元件。 了解如何開發製作對話方塊和Sling模型，以擴充JSON模型以填入自訂元件。

## 目標

1. 了解Sling模型在操控AEM提供的JSON模型API中的角色。
2. 了解如何建立AEM元件對話方塊。
3. 了解如何建立 **自訂** AEM元件與SPA編輯器架構相容。

## 您將建置的

前幾章的重點是開發SPA元件，並將其對應至 *現有* AEM核心元件。 本章重點介紹如何建立和擴展 *new* AEM元件和操作AEM提供的JSON模型。

簡單 `Custom Component` 說明建立新AEM元件所需的步驟。

![全大寫顯示的訊息](assets/custom-component/message-displayed.png)

## 必備條件

檢閱設定 [本地開發環境](overview.md#local-dev-environment).

### 取得程式碼

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. 使用Maven將程式碼基底部署至本機AEM例項：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   若使用 [AEM 6.x](overview.md#compatibility) 新增 `classic` 設定檔：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 安裝傳統 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest). 提供的影像 [WKND參考網站](https://github.com/adobe/aem-guides-wknd/releases/latest) 在WKND SPA上重複使用。 可使用 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp).

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 或切換到分支以本地簽出代碼 `Angular/custom-component-solution`.

## 定義AEM元件

AEM元件定義為節點和屬性。 在專案中，這些節點和屬性在 `ui.apps` 模組。 接下來，在 `ui.apps` 模組。

>[!NOTE]
>
> 快速重新整理 [AEM元件的基本概念可能會很實用](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html).

1. 開啟 `ui.apps` 檔案夾。
2. 導覽至 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` 並建立名為 `custom-component`.
3. 建立名為 `.content.xml` 在下面 `custom-component` 檔案夾。 填入 `custom-component/.content.xml` 並搭配下列項目：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![建立自訂元件定義](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"`  — 標識此節點是AEM元件。

   `jcr:title` 是顯示給內容作者的值，而 `componentGroup` 決定製作UI中元件的分組。

4. 在 `custom-component` 資料夾，建立另一個名為 `_cq_dialog`.
5. 在 `_cq_dialog` 資料夾建立名為 `.content.xml` 並填入下列項目：

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

   上述XML檔案會為 `Custom Component`. 檔案的關鍵部分是內部 `<message>` 節點。 此對話方塊包含簡單 `textfield` 已命名 `Message` 並將textifeld的值保留至 `message`.

   系統會在旁邊建立Sling模型，以公開 `message` 屬性。

   >[!NOTE]
   >
   > 您可以檢視更多 [檢視核心元件定義以進行對話的範例](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components). 您也可以檢視其他表單欄位，例如 `select`, `textarea`, `pathfield`，可在下方使用 `/libs/granite/ui/components/coral/foundation/form` in [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form).

   若使用傳統AEM元件， [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 指令碼通常為必要項。 由於SPA會轉譯元件，因此不需要HTL指令碼。

## 建立Sling模型

Sling模型是註解導向的Java™ &quot;POJOs&quot;(純舊Java™物件)，可方便將資料從JCR對應至Java™變數。 [Sling模型](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) 通常可封裝AEM元件的複雜伺服器端業務邏輯。

在SPA編輯器的內容中，Sling模型會透過JSON模型，透過使用 [Sling模型導出器](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html).

1. 在所選IDE中，開啟 `core` 模組。 `CustomComponent.java` 和 `CustomComponentImpl.java` 已建立，並在章節起始程式碼中加以儲存。

   >[!NOTE]
   >
   > 如果使用Visual Studio Code IDE，則安裝可能會很有幫助 [Java™擴充功能](https://code.visualstudio.com/docs/java/extensions).

2. 開啟Java™介面 `CustomComponent.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java介面](assets/custom-component/custom-component-interface.png)

   這是由Sling模型實作的Java™介面。

3. 更新 `CustomComponent.java` 這樣它就能延伸 `ComponentExporter` 介面：

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   實作 `ComponentExporter` 介面是JSON模型API自動擷取Sling模型的需求。

   此 `CustomComponent` 介面包括單個getter方法 `getMessage()`. 這是透過JSON模型公開製作對話方塊值的方法。 僅含空參數的getter方法 `()` 會匯出至JSON模型中。

4. 開啟 `CustomComponentImpl.java` at `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`.

   這是 `CustomComponent` 介面。 此 `@Model` 註解將Java™類標識為Sling模型。 此 `@Exporter` 批注使Java™類能夠通過Sling模型導出器進行序列化和導出。

5. 更新靜態變數 `RESOURCE_TYPE` 指向AEM元件 `wknd-spa-angular/components/custom-component` 在上一個練習中建立。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   元件的資源類型是將Sling Model系結至AEM元件，並最終對應至Angular元件。

6. 新增 `getExportedType()` 方法 `CustomComponentImpl` 要返回元件資源類型的類：

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   實作 `ComponentExporter` 介面並公開允許映射到Angular元件的資源類型。

7. 更新 `getMessage()` 傳回 `message` 由作者對話方塊保存的屬性。 使用 `@ValueMap` 註解是映射JCR值 `message` 至Java™變數：

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

   新增一些額外的「業務邏輯」，以傳回訊息的值作為大寫。 這可讓我們查看製作對話方塊所儲存的原始值與Sling模型公開的值之間的差異。

   >[!NOTE]
   您可以檢視 [已完成CustomComponentImpl.java，請參閱這裡](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java).

## 更新Angular元件

自訂元件的Angular程式碼已建立。 接下來，進行一些更新，將Angular元件對應至AEM元件。

1. 在 `ui.frontend` 模組開啟檔案 `ui.frontend/src/app/components/custom/custom.component.ts`
2. 觀察 `@Input() message: string;` 行。 轉換後的大寫值應對應至此變數。
3. 匯入 `MapTo` 物件，並使用它來對應至AEM元件：

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 開啟 `cutom.component.html` 並觀察 `{{message}}` 並排顯示 `<h2>` 標籤。
5. 開啟 `custom.component.css` 並新增下列規則：

   ```css
   :host-context {
       display: block;
   }
   ```

   當元件為空時，AEM編輯器預留位置會正確顯示 `:host-context` 或其他 `<div>` 需要設定為 `display: block;`.

6. 使用您的Maven技能，從專案目錄的根目錄部署更新至本機AEM環境：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新模板策略

接著，導覽至AEM以驗證更新並允許 `Custom Component` 新增至SPA。

1. 導覽至 [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels).

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   您應會看到以上兩行，指出 `CustomComponentImpl` 與 `wknd-spa-angular/components/custom-component` 元件，並透過Sling模型匯出工具註冊。

2. 導覽至SPA頁面範本，網址為 [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html).
3. 更新「配置容器」的策略以添加新 `Custom Component` 作為允許的元件：

   ![更新佈局容器策略](assets/custom-component/custom-component-allowed.png)

   儲存對原則的變更，並觀察 `Custom Component` 作為允許的元件：

   ![自訂元件作為允許的元件](assets/custom-component/custom-component-allowed-layout-container.png)

## 製作自訂元件

接下來，編寫 `Custom Component` 使用AEM SPA編輯器。

1. 導覽至 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html).
2. 在 `Edit` 模式，添加 `Custom Component` 到 `Layout Container`:

   ![插入新元件](assets/custom-component/insert-custom-component.png)

3. 開啟元件的對話方塊，然後輸入包含某些小寫字母的訊息。

   ![設定自訂元件](assets/custom-component/enter-dialog-message.png)

   這是在章節前面根據XML檔案建立的對話框。

4. 儲存變更。請注意，顯示的訊息會以所有大寫表示。

   ![全大寫顯示的訊息](assets/custom-component/message-displayed.png)

5. 導覽至 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 搜尋 `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   請注意，JSON值會根據新增至Sling模型的邏輯而設為所有大寫字母。

## 恭喜！ {#congratulations}

恭喜您，您已學會如何建立自訂AEM元件，以及Sling模型和對話方塊如何與JSON模型搭配運作。

您一律可以在上檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 或切換到分支以本地簽出代碼 `Angular/custom-component-solution`.

### 後續步驟 {#next-steps}

[擴充核心元件](extend-component.md)  — 了解如何擴充現有的核心元件以與AEM SPA編輯器搭配使用。 了解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。
