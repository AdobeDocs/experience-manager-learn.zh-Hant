---
title: 建立自定義元件 |編輯器和AEMAngularSPA入門
description: 瞭解如何建立要與編輯器一起使用的自AEM定義SPA元件。 瞭解如何開發作者對話框和Sling模型以擴展JSON模型以填充自定義元件。
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
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1471'
ht-degree: 1%

---

# 建立自定義元件 {#custom-component}

瞭解如何建立要與編輯器一起使用的自AEM定義SPA元件。 瞭解如何開發作者對話框和Sling模型以擴展JSON模型以填充自定義元件。

## 目標

1. 瞭解Sling模型在操作由提供的JSON模型API中的角AEM色。
2. 瞭解如何創AEM建元件對話框。
3. 學習建立 **自定義** 與AEM編輯器框架兼SPA容的元件。

## 您將構建的

前幾章的重點是開發各SPA個元件並將其映射到 *現有* 核AEM心元件。 本章重點介紹如何建立和擴展 *新* 組AEM件和操作由提供的JSON模AEM型。

簡單 `Custom Component` 說明了建立net-new元件所需的步AEM驟。

![所有大寫中顯示的消息](assets/custom-component/message-displayed.png)

## 必備條件

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。

### 獲取代碼

1. 通過Git下載本教程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/custom-component-start
   ```

2. 使用Maven將代碼庫部署到AEM本地實例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) 添加 `classic` 配置檔案：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

3. 為傳統 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest)。 提供的影像 [WKND參考站點](https://github.com/adobe/aem-guides-wknd/releases/latest) 在WKND上重SPA用。 可以使用 [包管AEM理器](http://localhost:4502/crx/packmgr/index.jsp)。

   ![包管理器安裝wknd.all](./assets/map-components/package-manager-wknd-all.png)

您始終可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 或通過切換到分支本地檢出代碼 `Angular/custom-component-solution`。

## 定義元AEM件

元件AEM定義為節點和屬性。 在項目中，這些節點和屬性在 `ui.apps` 中。 接下來，在AEM中建立元件 `ui.apps` 中。

>[!NOTE]
>
> 快速複習 [元件AEM的基礎](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html)。

1. 開啟 `ui.apps` 資料夾。
2. 導航到 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/components` 建立名為 `custom-component`。
3. 建立名為 `.content.xml` 在下面 `custom-component` 的子菜單。 填充 `custom-component/.content.xml` 下面列出：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Custom Component"
       componentGroup="WKND SPA Angular - Content"/>
   ```

   ![建立自定義元件定義](assets/custom-component/aem-custom-component-definition.png)

   `jcr:primaryType="cq:Component"`  — 標識此節點是組AEM件。

   `jcr:title` 是顯示給「內容作者」的值， `componentGroup` 確定創作UI中元件的分組。

4. 在 `custom-component` 資料夾，建立另一個名為 `_cq_dialog`。
5. 在 `_cq_dialog` 資料夾建立名為 `.content.xml` 並用以下方式填充：

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

   ![自定義元件定義](assets/custom-component/dialog-custom-component-defintion.png)

   上述XML檔案為 `Custom Component`。 檔案的關鍵部分是內部 `<message>` 的下界。 此對話框包含一個簡單 `textfield` 命名 `Message` 並將textifeld的值保留為 `message`。

   將在下面建立「吊帶模型」以顯示 `message` 屬性。

   >[!NOTE]
   >
   > 您可以查看更多 [通過查看核心元件定義的對話框示例](https://github.com/adobe/aem-core-wcm-components/tree/master/content/src/content/jcr_root/apps/core/wcm/components)。 您還可以查看其他表單域，如 `select`。 `textarea`。 `pathfield`在 `/libs/granite/ui/components/coral/foundation/form` 在 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/libs/granite/ui/components/coral/foundation/form)。

   使用傳統AEM元件， [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 通常需要指令碼。 由於呈SPA現元件，因此不需要HTL指令碼。

## 建立吊具模型

Sling模型是注釋驅動的Java™「POJO」(Plain Old Java™ Objects)，可方便將資料從JCR映射到Java™變數。 [吊具模型](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/component-basics.html#sling-models) 通常用於封裝元件的複雜伺服器端業務AEM邏輯。

在編輯器的上SPA下文中，Sling Models通過使用的 [Sling模型導出器](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/develop-sling-model-exporter.html)。

1. 在您選擇的IDE中，開啟 `core` 中。 `CustomComponent.java` 和 `CustomComponentImpl.java` 已建立並發佈為章節起始代碼的一部分。

   >[!NOTE]
   >
   > 如果使用Visual Studio Code IDE，則安裝可能會有所幫助 [Java™擴展](https://code.visualstudio.com/docs/java/extensions)。

2. 開啟Java™介面 `CustomComponent.java` 在 `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/CustomComponent.java`:

   ![CustomComponent.java介面](assets/custom-component/custom-component-interface.png)

   這是由Sling Model實現的Java™介面。

3. 更新 `CustomComponent.java` 這樣它就可以 `ComponentExporter` 介面：

   ```java
   package com.adobe.aem.guides.wknd.spa.angular.core.models;
   import com.adobe.cq.export.json.ComponentExporter;
   
   public interface CustomComponent extends ComponentExporter {
   
       public String getMessage();
   
   }
   ```

   執行 `ComponentExporter` 介面是JSON模型API自動提取Sling模型的要求。

   的 `CustomComponent` 介面包括單個getter方法 `getMessage()`。 這是通過JSON模型顯示作者對話框值的方法。 僅具有空參數的getter方法 `()` 在JSON模型中導出。

4. 開啟 `CustomComponentImpl.java` 在 `core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java`。

   這是 `CustomComponent` 。 的 `@Model` 注釋將Java™類標識為Sling模型。 的 `@Exporter` 批注使Java™類能夠通過Sling Model Exporter進行序列化和導出。

5. 更新靜態變數 `RESOURCE_TYPE` 指向組AEM件 `wknd-spa-angular/components/custom-component` 在上一個練習中建立。

   ```java
   static final String RESOURCE_TYPE = "wknd-spa-angular/components/custom-component";
   ```

   元件的資源類型是將Sling模型綁定到元件AEM並最終映射到Angular元件的類型。

6. 添加 `getExportedType()` 方法 `CustomComponentImpl` 類：

   ```java
   @Override
   public String getExportedType() {
       return CustomComponentImpl.RESOURCE_TYPE;
   }
   ```

   在執行 `ComponentExporter` 並顯示允許映射到Angular元件的資源類型。

7. 更新 `getMessage()` 返回的值 `message` 「作者」對話框保留的屬性。 使用 `@ValueMap` 注釋是映射JCR值 `message` 到Java™變數：

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

   添加一些附加的「業務邏輯」以返回消息值作為大寫。 這允許我們查看作者對話框儲存的原始值與Sling模型暴露的值之間的差。

   >[!NOTE]
   您可以查看 [已在此處完成CustomComponentImpl.java](https://github.com/adobe/aem-guides-wknd-spa/blob/Angular/custom-component-solution/core/src/main/java/com/adobe/aem/guides/wknd/spa/angular/core/models/impl/CustomComponentImpl.java)。

## 更新Angular元件

已建立自定義元件的Angular代碼。 接下來，進行幾次更新以將Angular元件映射到AEM元件。

1. 在 `ui.frontend` 模組開啟檔案 `ui.frontend/src/app/components/custom/custom.component.ts`
2. 觀察 `@Input() message: string;` 。 應將轉換的大寫值映射到此變數。
3. 導入 `MapTo` Editor JS SDKAEM中的SPA對象，並使用它映射到AEM元件：

   ```diff
   + import {MapTo} from '@adobe/cq-angular-editable-components';
   
    ...
    export class CustomComponent implements OnInit {
        ...
    }
   
   + MapTo('wknd-spa-angular/components/custom-component')(CustomComponent, CustomEditConfig);
   ```

4. 開啟 `cutom.component.html` 並觀察 `{{message}}` 在 `<h2>` 標籤。
5. 開啟 `custom.component.css` 並添加以下規則：

   ```css
   :host-context {
       display: block;
   }
   ```

   當組AEM件為空時，編輯器佔位符可正確顯示 `:host-context` 或 `<div>` 需要設定為 `display: block;`。

6. 使用Maven技能將AEM更新從項目目錄的根部部署到本地環境：

   ```shell
   $ cd aem-guides-wknd-spa
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 更新模板策略

接下來，導航AEM至驗證更新並允許 `Custom Component` 的上界SPA。

1. 通過導航到 [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)。

   ```plain
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl - wknd-spa-angular/components/custom-component
   
   com.adobe.aem.guides.wknd.spa.angular.core.models.impl.CustomComponentImpl exports 'wknd-spa-angular/components/custom-component' with selector 'model' and extension '[Ljava.lang.String;@6fb4a693' with exporter 'jackson'
   ```

   您應看到上面兩行，這些行表示 `CustomComponentImpl` 與 `wknd-spa-angular/components/custom-component` 元件，並通過Sling模型導出器註冊。

2. 導航到SPA頁面模板 [http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-angular/settings/wcm/templates/spa-page-template/structure.html)。
3. 更新佈局容器的策略以添加新 `Custom Component` 作為允許的元件：

   ![更新佈局容器策略](assets/custom-component/custom-component-allowed.png)

   保存對策略的更改，並觀察 `Custom Component` 作為允許的元件：

   ![自定義元件作為允許的元件](assets/custom-component/custom-component-allowed-layout-container.png)

## 建立自定義元件

接下來，編寫 `Custom Component` 使用編AEM輯器SPA。

1. 導航到 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。
2. 在 `Edit` 模式，添加 `Custom Component` 到 `Layout Container`:

   ![插入新元件](assets/custom-component/insert-custom-component.png)

3. 開啟元件的對話框並輸入包含一些小寫字母的消息。

   ![配置自定義元件](assets/custom-component/enter-dialog-message.png)

   這是基於本章前面的XML檔案建立的對話框。

4. 儲存變更。請注意顯示的消息已全部大寫。

   ![所有大寫中顯示的消息](assets/custom-component/message-displayed.png)

5. 導航至 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。 搜尋 `wknd-spa-angular/components/custom-component`:

   ```json
   "custom_component_208183317": {
       "message": "HELLO WORLD",
       ":type": "wknd-spa-angular/components/custom-component"
   }
   ```

   請注意，JSON值根據添加到Sling模型的邏輯設定為所有大寫字母。

## 恭喜！ {#congratulations}

恭喜，您學習了如何建立自AEM定義元件，以及Sling Models和對話框如何與JSON模型配合工作。

您始終可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/custom-component-solution) 或通過切換到分支本地檢出代碼 `Angular/custom-component-solution`。

### 後續步驟 {#next-steps}

[擴展核心元件](extend-component.md)  — 瞭解如何擴展要與編輯器一起使用的現有核AEM心組SPA件。 瞭解如何將屬性和內容添加到現有元件是擴展編輯器實現功能的AEM強SPA大技術。
