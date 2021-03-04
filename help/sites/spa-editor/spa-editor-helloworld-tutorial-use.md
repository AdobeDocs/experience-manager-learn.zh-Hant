---
title: 使用編輯AEM器開SPA發-Hello World教學課程
description: SPA EditorAEM支援「單頁應用程式」或的內容內容編輯SPA。 本教學課程是與SPAEditor JS AEM SDK搭配使用的開SPA發簡介。 本教學課程將新增自訂Hello World元件，以擴充We.Retail Journal應用程式。 使用者可使用React或Angular架構完成教學課程。
sub-product: 網站，內容服務
feature: Spa編輯器
topics: development, single-page-applications
audience: developer
doc-type: tutorial
activity: use
version: 6.3, 6.4, 6.5
topic: SPA
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3176'
ht-degree: 1%

---


# 使用編輯器AEM開SPA發-Hello World教學課程{#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> 本教學課程為&#x200B;**不再提倡**。 建議您執行下列任一操作：[開始使用AEMEditorSPA和Angular](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-angular-tutorial/overview.html)或[開始使用Editor和React&lt;a3/AEM>](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-react-tutorial/overview.html)

SPA EditorAEM支援「單頁應用程式」或的內容內容編輯SPA。 本教學課程是與SPAEditor JS AEM SDK搭配使用的開SPA發簡介。 本教學課程將新增自訂Hello World元件，以擴充We.Retail Journal應用程式。 使用者可使用React或Angular架構完成教學課程。

>[!NOTE]
>
> 單頁應用程式(SPA)編輯器功能需AEM要6.4 Service Pack 2或更新版本。
>
> EditorSPA是建議的專案解決方案，適用於需SPA要以架構為基礎的用戶端轉換(例如，React或Angular)。

## 先決條件閱讀{#prereq}

本教學課程旨在反白顯示將元件對應至元SPA件以啟AEM用內容內容編輯所需的步驟。 開始本教學課程的使用者應熟悉與Adobe Experience Manager合作開發的基本概念AEM，以及與Angular架構的Reaccest合作開發。 本教學課程涵蓋後端和前端開發工作。

建議在開始本教學課程之前先檢閱下列資源：

* [SPA Editor Feature Video](spa-editor-framework-feature-video-use.md)  - Editor和We.Retail Journal應用程SPA式的影片概觀。
* [React.js教學課程](https://reactjs.org/tutorial/tutorial.html) -使用React架構進行開發的簡介。
* [Angular教學課程](https://angular.io/tutorial) -使用Angular進行開發的簡介

## 本機開發環境 {#local-dev}

本教學課程的適用對象：

[Adobe Experience Manager6.5](https://helpx.adobe.com/tw/experience-manager/6-5/release-notes.html) 或 [Adobe Experience Manager6.4](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) +  [Service Pack 5](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)

在本教學課程中，應安裝下列技術與工具：

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) 和npm 5.6.0+（npm隨node.js一起安裝）

開啟新的終端並執行下列動作，以再次檢查上述工具的安裝：

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## 概覽 {#overview}

基本概念是將元件對SPA應至元AEM件。 元AEM件，執行伺服器端，以JSON格式匯出內容。 JSON內容會由在瀏覽器SPA中執行用戶端的使用者使用。 將建立元件和組SPA件之間AEM的1:1映射。

![SPA元件映射](assets/spa-editor-helloworld-tutorial-use/mapto.png)

常用的架構[React JS](https://reactjs.org/)和[Angular](https://angular.io/)現成可用。 使用者可以在Angular或React中完成本教學課程，不論他們最熟悉的架構為何。

## 項目設定{#project-setup}

發SPA展一腳AEM，一腳踏開。 其目標是讓發SPA展能夠獨立進行，並且（大多）不受AEM限。

* 在前SPA端開發中，項AEM目可獨立於項目運作。
* 前端建置工具和技術（例如Webpack、NPM、[!DNL Grunt]和[!DNL Gulp]）將繼續使用。
* 要為生成項AEM目，將編SPA譯項目並自動將項目包AEM含在項目中。
* 用AEM於部署到的標SPA準包AEM。

![對象和部署概述](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*發SPA展有一腳AEM發展，另一腳腳——允許發SPA展獨立進行，（大部分）不可知AEM。*

本教學課程的目標是使用新元件來擴充We.Retail Journal應用程式。 首先，下載We.Retail Journal應用程式的原始碼，並部署至本機AEM。

1. **從** GitHub下載最 [新的We.Retail日誌代碼](https://github.com/adobe/aem-sample-we-retail-journal)。

   或者，從命令行克隆儲存庫：

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >本教程將針對&#x200B;**master**&#x200B;分支和&#x200B;**1.2.1-SNAPSHOT**&#x200B;版本的項目使用。

1. 下列結構應可見：

   ![專案資料夾結構](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   項目包含下列主要模組：

   * `all`:將整個專案內嵌並安裝在單一套件中。
   * `bundles`:包含兩個OSGi組合：commons和core，其中包含 [!DNL Sling Models] 和其他Java程式碼。
   * `ui.apps`:包含專案的/apps部分，即JS和CSSclientlibs、元件、執行模式特定設定。
   * `ui.content`:包含結構內容和配置(`/content`, `/conf`)
   * `react-app`:We.Retail Journal React Application.這既是Maven模組，也是Webpack項目。
   * `angular-app`:We.Retail JournalAngular應用程式。這既是[!DNL Maven]模組，也是Webpack項目。

1. 開啟新的終端視窗並執行下列命令，以建立整個應用程式並將其部署AEM至在[http://localhost:4502](http://localhost:4502)上執行的本機例項。

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > 在此項目中，用於構建和封裝整個項目的Maven配置檔案是`autoInstallSinglePackage`

   >[!CAUTION]
   >
   > 如果在構建過程中收到錯誤， [請確保Maven settings.xml檔案包含Adobe的Maven對象儲存庫](https://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html)。

1. 導航到:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   We.Retail Journal應用程式應顯示在AEM Sites編輯器中。

1. 在[!UICONTROL 編輯]模式中，選擇要編輯的元件並對內容進行更新。

   ![編輯元件](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. 選擇[!UICONTROL 頁面屬性]表徵圖以開啟[!UICONTROL 頁面屬性]。 選擇[!UICONTROL 編輯模板]以開啟頁面的模板。

   ![頁面屬性功能表](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. 在最新版的SPAEditor中，[Editable templates](https://helpx.adobe.com/tw/experience-manager/6-5/sites/developing/using/page-templates-editable.html)可與傳統Sites實作相同的方式使用。 我們稍後會使用自訂元件來重新檢視此項目。

   >[!NOTE]
   >
   > 只有AEM6.5和AEM6.4 + **Service Pack 5**&#x200B;支援可編輯範本。

## 開發概觀{#development-overview}

![概觀開發](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

開SPA發迭代獨立於AEM。 當準備SPA部署至下列高階AEM步驟時（如上圖所示）。

1. 調AEM用項目構建，這反過來又觸發項目的SPA構建。 We.Retail Journal使用&#x200B;[**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin)。
1. 專SPA案的&#x200B;[**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator)將編譯為SPAAEMClient Library內嵌在專案AEM中。
1. 專AEM案會產生包AEM括已編譯的SPA套件，以及任何其他支AEM援程式碼。

## 建立AEM元件{#aem-component}

**角色：開AEM發人員**

首先AEM將建立一個元件。 元AEM件負責轉譯由React元件讀取的JSON屬性。 該組AEM件還負責為元件的任何可編輯屬性提供對話框。

使用[!DNL Eclipse]或其他[!DNL IDE]匯入We.Retail Journal Maven專案。

1. 更新反應器&#x200B;**pom.xml**&#x200B;以移除[!DNL Apache Rat]外掛程式。 此外掛程式會檢查每個檔案，以確保有授權標題。 為了我們的目的，我們不需要擔心此功能。

   在&#x200B;**aem-sample-we-retail-journal/pom.xml**&#x200B;中，移除&#x200B;**apache-rate-plugin**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. 在&#x200B;**we-retail-journal-content**(`<src>/aem-sample-we-retail-journal/ui.apps`)模組中，在`ui.apps/jcr_root/apps/we-retail-journal/components`下面建立一個名為&#x200B;**helloworld**&#x200B;的新節點，該節點類型為&#x200B;**cq:Component**。
1. 將下列屬性新增至&#x200B;**helloworld**&#x200B;元件，以下列XML(`/helloworld/.content.xml`)表示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello World元件](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > 為了說明「可編輯的範本」功能，我們特意設定`componentGroup="Custom Components"`。 在實際專案中，最好將元件群組數目減到最小，因此較佳的群組會是&quot;[!DNL We.Retail Journal]&quot;，以搭配其他內容元件。
   >
   > 只AEM有AEM6.5和6.4 + **Service Pack 5**&#x200B;支援可編輯範本。

1. 接下來將建立一個對話框，允許為&#x200B;**Hello World**&#x200B;元件配置自定義消息。 在`/apps/we-retail-journal/components/helloworld`下添加&#x200B;**cq:dialog**&#x200B;的&#x200B;**nt:antructured**&#x200B;節點名稱。
1. **cq:dialog**&#x200B;將顯示單一文字欄位，將文字保留至名為&#x200B;**[!DNL message]**&#x200B;的屬性。 在新建立的&#x200B;**cq:dialog**&#x200B;下面添加以下節點和屬性，以XML表示(`helloworld/_cq_dialog/.content.xml`):

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
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
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
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

   ![檔案結構](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   上述XML節點定義將建立一個對話框，其中包含一個文本欄位，允許用戶輸入「消息」。 請注意`<message />`節點中的`name="./message"`屬性。 此名稱是將儲存在中的JCR中的屬性的名稱AEM。

1. 接下來將建立空策略對話框(`cq:design_dialog`)。 需要「策略」對話框才能在模板編輯器中查看元件。 對於這個簡單的使用案例，它將是空白對話方塊。

   在`/apps/we-retail-journal/components/helloworld`下添加`nt:unstructured`的節點名`cq:design_dialog`。

   配置以下的XML表示(`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. 從命令行將代碼AEM庫部署到：

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   在[CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld)中，通過檢查`/apps/we-retail-journal/components:`下的資料夾來驗證元件是否已部署

   ![在CRXDE Lite中部署的元件結構](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## Create Sling Model {#create-sling-model}

**角色：開AEM發人員**

接下來，將建立[!DNL Sling Model]以備份[!DNL Hello World]元件。 在傳統WCM使用案例中，[!DNL Sling Model]實作任何商業邏輯，而伺服器端轉譯指令碼(HTL)將呼叫[!DNL Sling Model]。 如此可讓轉譯指令碼相對簡單。

[!DNL Sling Models] 也用於使SPA用案例，以實作伺服器端商業邏輯。區別在於，在[!DNL SPA]使用案例中，[!DNL Sling Models]會將其方法顯示為序號JSON。

>[!NOTE]
>
>最佳實務是，開發人員應盡可能使用[AEM核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)。 核心元件提供[!DNL Sling Models]的JSON輸出(「SPA-ready」)功能，讓開發人員將更多心力放在前端簡報上。

1. 在您選擇的編輯器中，開啟&#x200B;**we-retail-journal-commons**&#x200B;專案(`<src>/aem-sample-we-retail-journal/bundles/commons`)。
1. 在包`com.adobe.cq.sample.spa.commons.impl.models`中：
   * 建立名為`HelloWorld`的新類。
   * 為`com.adobe.cq.export.json.ComponentExporter.`添加實現介面

   ![新建Java類嚮導](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   `ComponentExporter`介面必須實作，[!DNL Sling Model]才能與Content ServicesAEM相容。

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. 添加名為`RESOURCE_TYPE`的靜態變數以標識[!DNL HelloWorld]元件的資源類型：

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. 添加`@Model`和`@Exporter`的OSGi注釋。 `@Model`注釋將類註冊為[!DNL Sling Model]。 `@Exporter`註解會使用[!DNL Jackson Exporter]架構，將方法顯示為序列化JSON。

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. 實作方法`getDisplayMessage()`以傳回JCR屬性`message`。 使用`@ValueMapValue`的[!DNL Sling Model]注釋，可輕鬆擷取儲存在元件下方的`message`屬性。 `@Optional`註解很重要，因為當元件首次新增至頁面時，`message`將不會填入。

   作為業務邏輯的一部分，字串&quot;**Hello**&quot;將優先於消息。

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > 方法名稱`getDisplayMessage`很重要。 當[!DNL Sling Model]以[!DNL Jackson Exporter]序號化時，它會以JSON屬性的形式公開：`displayMessage`。 [!DNL Jackson Exporter]會序列化並公開所有未採用參數的`getter`方法（除非明確標籤為忽略）。 稍後在React /Angular應用程式中，我們會讀取此屬性值，並將它顯示為應用程式的一部分。

   方法`getExportedType`也很重要。 元件`resourceType`的值將用來「映射」JSON資料至前端元件(Angular/反應)。 我們將在下一節中探討此問題。

1. 實施方法`getExportedType()`以返回`HelloWorld`元件的資源類型。

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   [**HelloWorld.java**&#x200B;的完整程式碼可在這裡找到。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. 將程式碼部署AEM為使用Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   導覽至OSGi主控台中的[[!UICONTROL Status] > [!UICONTROL Sling Models]](http://localhost:4502/system/console/status-slingmodels)，以驗證[!DNL Sling Model]的部署與註冊。

   您應該會看到`HelloWorld` Sling Model系結至`we-retail-journal/components/helloworld` Sling資源類型，且已註冊為[!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## 建立React元件{#react-component}

**角色：前端開發人員**

接下來，將建立React元件。 使用您選擇的編輯器開啟&#x200B;**react-app**&#x200B;模組(`<src>/aem-sample-we-retail-journal/react-app`)。

>[!NOTE]
>
> 如果您只對[Angular開發](#angular-component)感興趣，請免費略過本節。

1. 在`react-app`資料夾內，導覽至其src資料夾。 展開元件資料夾以檢視現有的React元件檔案。

   ![Reacte元件檔案結構](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. 在名為`HelloWorld.js`的元件資料夾下添加新檔案。
1. 開啟 `HelloWorld.js`. 新增匯入陳述式以匯入React元件庫。 添加第二個import語句以導入Adobe提供的`MapTo`幫助程式。 `MapTo`協助程式提供Rearcect元件與元件AEMJSON的對應。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. 在導入下面建立一個名為`HelloWorld`的新類，該類擴展了React `Component`介面。 將所需的`render()`方法添加到`HelloWorld`類。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. `MapTo`幫助程式會自動包含名為`cqModel`的對象，作為React元件prop的一部分。 `cqModel`包含[!DNL Sling Model]公開的所有屬性。

   請記住，先前建立的[!DNL Sling Model]包含方法`getDisplayMessage()`。 `getDisplayMessage()` 會轉譯為輸出時命名 `displayMessage` 的JSON金鑰。

   實作`render()`方法以輸出包含`displayMessage`值的`h1`標籤。 [JSX](https://reactjs.org/docs/introducing-jsx.html)是JavaScript的語法擴充功能，可用來傳回元件的最終標籤。

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. 實施編輯配置方法。 此方法會透過`MapTo`協助程式傳遞，並提供編輯器AEM資訊，以在元件為空白時顯示預留位置。 當元件新增至但尚未編寫時，SPA就會發生此情況。 在`HelloWorld`類別下方新增下列項目：

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. 在檔案結尾，呼叫`MapTo`協助程式，傳遞`HelloWorld`類別和`HelloWorldEditConfig`。 這將根據元件的資源類AEM型將React Component映AEM射到元件：`we-retail-journal/components/helloworld`。

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   [**HelloWorld.js**&#x200B;的完成代碼可在此處找到。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. 開啟檔案`ImportComponents.js`。 可在`<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`找到。

   在編譯的JavaScript套件中，新增一行以要求`HelloWorld.js`與其他元件一起使用：

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. 在`components`資料夾中，建立名為`HelloWorld.css`的新檔案，作為`HelloWorld.js.`的同級檔案，以下列方式填入檔案，為`HelloWorld`元件建立一些基本樣式：

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 重新開啟`HelloWorld.js`並更新匯入陳述式下方的`HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. 將程式碼部署AEM為使用Apache Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. 在[中，CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js)開啟`/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`。 在app.js中快速搜尋HelloWorld，以確認已編譯的應用程式中已包含React元件。

   >[!NOTE]
   >
   > **app.** jsis搭售的React應用程式。程式碼不再是人類可讀的。 `npm run build`命令已觸發最佳化組建版本，可輸出可由現代瀏覽器解譯的已編譯JavaScript。


## 建立Angular元件{#angular-component}

**角色：前端開發人員**

>[!NOTE]
>
> 如果您只對React開發感興趣，請免費略過本節。

接下來，將建立Angular元件。 使用您選擇的編輯器開啟&#x200B;**angular-app**&#x200B;模組(`<src>/aem-sample-we-retail-journal/angular-app`)。

1. 在`angular-app`資料夾內，導覽至其`src`資料夾。 展開元件資料夾以查看現有的Angular元件檔案。

   ![Angular檔案結構](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. 在名為`helloworld`的元件資料夾下添加一個新資料夾。 在`helloworld`資料夾下添加名為`helloworld.component.css, helloworld.component.html, helloworld.component.ts`的新檔案。

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. 開啟 `helloworld.component.ts`. 添加導入語句以導入Angular`Component`和`Input`類。 建立新元件，將`styleUrls`和`templateUrl`指向`helloworld.component.css`和`helloworld.component.html`。 最後，使用預期的輸入`displayMessage`匯出類別`HelloWorldComponent`。

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > 如果您回想之前建立的[!DNL Sling Model]，會有一個方法&#x200B;**getDisplayMessage()**。 此方法的序號化JSON將是&#x200B;**displayMessage**，我們現在正在Angular應用程式中閱讀。

1. 開啟`helloworld.component.html`以包含將列印`displayMessage`屬性的`h1`標籤：

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. 更新`helloworld.component.css`以包含元件的一些基本樣式。

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 使用下列測試台更新`helloworld.component.spec.ts`:

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. 下次更新`src/components/mapping.ts`以包含`HelloWorldComponent`。 添加`HelloWorldEditConfig`，該將在元件配置前在編AEM輯器中標籤佔位符。 最後，使用`MapTo`幫AEM助程式添加一行以將元件映射到Angular元件。

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   [**mapping.ts**&#x200B;的完整程式碼可在此找到。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. 更新`src/app.module.ts`以更新&#x200B;**NgModule**。 將&#x200B;**`HelloWorldComponent`**&#x200B;添加為&#x200B;**屬於** AppModule **的聲明**。 此外，也將`HelloWorldComponent`新增為&#x200B;**entryComponent**，以便在處理JSON模型時，將它編譯並動態包含在應用程式中。

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   [**app.module.ts**&#x200B;的完成程式碼可在此處找到。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. 將程式碼部署AEM為使用Maven:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. 在[中，CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js)開啟`/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`。 在`main.js`中對&#x200B;**HelloWorld**&#x200B;執行快速搜索，以驗證是否已包含Angular元件。

   >[!NOTE]
   >
   > **main.** jsis搭售的Angular應用程式。程式碼不再是人類可讀的。 npm run build命令已觸發最佳化組建，可輸出已編譯的JavaScript，可供現代瀏覽器解譯。

## 更新模板{#template-update}

1. 導覽至React和／或Angular版本的可編輯範本：

   * (Angular)[http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React)[http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. 選擇主[!UICONTROL 佈局容器]並選擇[!UICONTROL 策略]表徵圖以開啟其策略：

   ![選取版面原則](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   在「**[!UICONTROL 屬性]** > **[!UICONTROL 允許的元件]**」下，執行&#x200B;**[!DNL Custom Components]**&#x200B;搜索。 您應該看到&#x200B;**[!DNL Hello World]**&#x200B;元件，選擇它。 按一下右上角的核取方塊，儲存您所做的變更。

   ![配置容器策略配置](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. 儲存後，您應會在[!UICONTROL 版面容器]中將&#x200B;**[!DNL HelloWorld]**&#x200B;元件視為允許的元件。

   ![允許的元件已更新](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > 只有AEM6.5和AEM6.4.5支援編輯器的「可編輯模板」SPA功能。 如果使AEM用6.4，則需要通過CRXDE Lite手動配置允許的元件策略：`/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`或`/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite顯示[!UICONTROL 佈局容器]中[!UICONTROL 允許的元件]的更新策略配置：

   ![CRXDE Lite顯示版面容器中允許的元件的更新原則組態](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## 將所有內容整合在一起{#putting-together}

1. 導覽至「Angular」或「回應」頁面：

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. 尋找&#x200B;**[!DNL Hello World]**&#x200B;元件，並將&#x200B;**[!DNL Hello World]**&#x200B;元件拖放至頁面。

   ![hello world drag + drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   預留位置應會出現。

   ![Hello world place holder](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. 選取元件並在對話方塊中新增訊息，例如「World」或「Your Name」。 儲存變更。

   ![渲染元件](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   請注意，字串&quot;Hello&quot;一律會先於訊息。 這是`HelloWorld.java` [!DNL Sling Model]中邏輯的結果。

## 後續步驟{#next-steps}

[HelloWorld元件的完整解決方案](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* GitHub](https://github.com/adobe/aem-sample-we-retail-journal)上[[!DNL We.Retail Journal] 的完整原始碼
* 檢視有關開發React with [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)的更深入教學課程

## 疑難排解 {#troubleshooting}

### 無法在Eclipse {#unable-to-build-project-in-eclipse}中建立專案

**錯誤：將** 專案匯入Eclipse以執行無 [!DNL We.Retail Journal] 法辨識的目標時發生錯誤：

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![eclipse錯誤嚮導](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**解析度**:按一下「完成」以稍後解決這些問題。這不應妨礙教學課程的完成。

**錯誤**:React模組在Maven `react-app`構建過程中無法成功構建。

**解析度：** 嘗試刪除 `node_modules` react-app下 **方的資料夾**。從項目的根目錄重新運行Apache Maven命令`mvn  clean install -PautoInstallSinglePackage`。

### {#unsatisfied-dependencies-in-aem}中AEM未滿足的依賴項

![包管理器相依性錯誤](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

如果不AEM滿足相關性，在&#x200B;**[!UICONTROL AEM Package Manager]**&#x200B;或&#x200B;**[!UICONTROL AEM Web Console]**(Felix Console)中，這表示「編SPA輯器功能」不可用。

### 元件不顯示

**錯誤**:即使在成功部署並驗證編譯版React/Angular應用程式是否有更新的元件後，我將元件拖曳至頁面時，仍不會顯示它。 `helloworld` 我可以在UI中看到元AEM件。

**解析度**:清除瀏覽器的歷史記錄／快取和／或開啟新瀏覽器或使用Incognito模式。如果不起作用，則使本地實例上的客戶端庫快取失效AEM。 嘗AEM試快取大型clientlibraries，以提高效率。 有時需要手動取消驗證快取，以修正快取過期代碼的問題。

導覽至：[http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)，然後按一下「使快取失效」。 返回您的React/Angular頁面並重新整理頁面。

![重建客戶端庫](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
