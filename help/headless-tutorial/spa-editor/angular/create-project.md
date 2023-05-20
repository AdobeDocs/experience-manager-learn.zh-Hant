---
title: 編輯SPA器項目 |編輯器和AEMAngularSPA入門
description: 瞭解如何將Adobe Experience Manager(AEM)Maven項目用作與編輯器整合的Angular應用程AEM序的SPA起點。
feature: SPA Editor, AEM Project Archetype
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 2%

---

# 編輯SPA器項目 {#create-project}

瞭解如何將Adobe Experience Manager(AEM)Maven項目用作與編輯器整合的Angular應用程AEM序的SPA起點。

## 目標

1. 瞭解由Maven原型AEM構SPA建的新編輯器項目的結構。
2. 將啟動程式項目部署到的本地實例AEM。

## 您將構建的

在本章中，將根AEM據 [項AEM目原型](https://github.com/adobe/aem-project-archetype)。 該項AEM目以一個非常簡單的Angular起點啟動SPA。 本章所用項目將作為執行《世界知識和人口公約》的基礎SPA，並將在今後各章中加以建立。

![WKND SPAAngular啟動器項目](./assets/create-project/what-you-will-build.png)

*經典Hello World留言。*

## 必備條件

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。 確保新的Adobe Experience Manager實例 **作者** 模式，正在本地運行。

## 獲取項目

有幾個選項可為建立Maven多模組項目AEM。 本教程使用了 [項AEM目原型](https://github.com/adobe/aem-project-archetype) 作為教程代碼的基礎。 已對項目代碼進行了修改，以支援多個版本的AEM。 請查看 [關於向後相容性的注釋](overview.md#compatibility)。

>[!CAUTION]
>
>使用 **最新** 版本 [原型](https://github.com/adobe/aem-project-archetype) 為現實世界的實施創造新項目。 項AEM目應以使用 `aemVersion` 原型的屬性。

1. 通過Git下載本教程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 以下資料夾和檔案結構表示AEM由本地檔案系統上的Maven原型生成的Project:

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. 從生成項目時使用AEM了以下屬性 [項AEM目原型](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | 屬性 | 值 |
   |-----------------|---------------------------------------|
   | aem版本 | 雲 |
   | appTitle | WKNDSPAAngular |
   | 應用ID | wknd-spa-angular |
   | 組ID | com.adobe.aem.guides |
   | 前端模組 | angular |
   | 包 | com.adobe.aem.guides.wknd.spa.angular |
   | include示例 | n |

   >[!NOTE]
   >
   > 注意 `frontendModule=angular` 屬性。 這將指示項AEM目原型使用啟動程式引導項目 [Angular碼基](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) 與編輯器一AEM起SPA使用。

## 生成項目

接下來，編譯、生成項目代碼並將其部署到使用Maven的本AEM地實例。

1. 確保埠上AEM的實例正在本地運行 **4502**。
2. 從命令行終端驗證Maven是否已安裝：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 從 `aem-guides-wknd-spa` 要生成和部署項目的目錄AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   如果使用 [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   項目的多個模組應編譯並部署到AEM。

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   馬文檔案 ***自動安裝SinglePackage*** 編譯項目的各個模組，並將單個包部署到實AEM例。 預設情況下，此包部署到AEM埠上本地運行的實例 **4502** 還有 **管理員：管理員**。

4. 導航到 **[!UICONTROL 包管理器]** 在本地實例AEM上： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

5. 您應看到三個包 `wknd-spa-angular.all`。 `wknd-spa-angular.ui.apps` 和 `wknd-spa-angular.ui.content`。

   ![WKND包SPA](./assets/create-project/package-manager.png)

   項目所需的所有自定義代碼都捆綁到這些軟體包中，並安裝在運行AEM時。

6. 您還應看到以下幾個包： `spa.project.core` 和 `core.wcm.components`。 這些是原型自動包括的從屬關係。 有關 [可AEM在此處找到核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。

## 作者內容

接下來，開啟SPA原型生成的啟動程式並更新部分內容。

1. 導航到 **[!UICONTROL 站點]** 控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND包括SPA一個基本的站點結構，包括國家/地區、語言和首頁。 此層次結構基於的原型的預設值 `language_country` 和 `isSingleCountryWebsite`。 通過更新 [可用屬性](https://github.com/adobe/aem-project-archetype#available-properties) 生成項目時。

2. 開啟 **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** 頁面 **[!UICONTROL 編輯]** 按鈕：

   ![站點控制台](./assets/create-project/open-home-page.png)

3. A **[!UICONTROL 文本]** 元件已添加到頁面。 可以像中的任何其他元件一樣編輯此組AEM件。

   ![更新文本元件](./assets/create-project/update-text-component.gif)

4. 添加附加 **[!UICONTROL 文本]** 元件。

   請注意，創作體驗與傳統的AEM Sites頁麵類似。 目前可用的元件數量有限。 在本教程中將添加更多內容。

## Inspect單頁應用程式

接下來，使用瀏覽器的開發人員工具驗證這是單頁應用程式。

1. 在 **[!UICONTROL 頁面編輯器]**，按一下 **[!UICONTROL 頁面資訊]** 菜單> **[!UICONTROL 查看為已發佈]**:

   ![「查看為已發佈」按鈕](./assets/create-project/view-as-published.png)

   這將開啟一個帶有查詢參數的新頁籤 `?wcmmode=disabled` 有效地關閉了編AEM輯器： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 查看頁面的源並注意文本內容 **[!DNL Hello World]** 或找不到任何其他內容。 相反，您應看到HTML，如下所示：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` 是載入SPA到頁面並負責呈現內容的Angular。

   *內容從何而來？*

3. 返回到頁籤： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在刷新期間檢查頁面的網路流量。 查看 **XHR** 請求：

   ![XHR請求](./assets/create-project/xhr-requests.png)

   應該有一個 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。 它包含所有將驅動的JSON格式內SPA容。

5. 在新頁籤中，開啟 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   請求 `en.model.json` 表示驅動應用程式的內容模型。 InspectJSON輸出，您應該能夠找到代表 **[!UICONTROL 文本]** 元件。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   在下一章中，我們將檢查JSON內容如何從元件映AEM射到SPA元件，以形成編輯器體AEM驗的SPA基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴展以自動格式化JSON輸出可能會有所幫助。

## 恭喜！ {#congratulations}

祝賀您，您剛剛建立了第AEM一個SPA編輯項目！

現在非常簡單，但在接下來的幾章中增加了更多功能。

### 後續步驟 {#next-steps}

[整合SPA](integrate-spa.md)  — 瞭解源SPA代碼如何與項目集AEM成，並瞭解可用於快速開發的工SPA具。
