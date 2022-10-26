---
title: SPA編輯器專案 |開始使用AEM SPA編輯器和Angular
description: 了解如何使用Adobe Experience Manager(AEM)Maven專案，作為與AEM SPA編輯器整合之Angular應用程式的起點。
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

# SPA編輯器專案 {#create-project}

了解如何使用Adobe Experience Manager(AEM)Maven專案，作為與AEM SPA編輯器整合之Angular應用程式的起點。

## 目標

1. 了解從Maven原型建置的新AEM SPA編輯器專案的結構。
2. 將入門專案部署至本機的AEM例項。

## 您將建置的

在本章中，會根據 [AEM專案原型](https://github.com/adobe/aem-project-archetype). AEM專案正以AngularSPA的簡單起點啟動。 本章中使用的項目將作為實施WKND SPA的基礎，並在以後的章節中構建。

![WKND SPAAngular入門專案](./assets/create-project/what-you-will-build.png)

*經典的「你好世界」訊息。*

## 必備條件

檢閱設定 [本地開發環境](overview.md#local-dev-environment). 請確定新的Adobe Experience Manager例項已於 **作者** 模式，正在本地運行。

## 取得專案

為AEM建立Maven多模組專案有數個選項。 本教學課程使用最新 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 做為教學課程程式碼的基礎。 已修改專案程式碼，以支援多個版本的AEM。 請查閱 [關於回溯相容性的說明](overview.md#compatibility).

>[!CAUTION]
>
>最佳實務是使用 **最新** 版本 [原型](https://github.com/adobe/aem-project-archetype) 為實際實作產生新專案。 AEM專案應以單一版本的AEM為目標，使用 `aemVersion` 原型的屬性。

1. 透過Git下載本教學課程的起始點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 下列資料夾和檔案結構代表本機檔案系統上Maven原型產生的AEM專案：

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

3. 從中產生AEM專案時，已使用下列屬性 [AEM專案原型](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | 屬性 | 值 |
   |-----------------|---------------------------------------|
   | aemVersion | 雲端 |
   | appTitle | WKND SPAAngular |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | 套件 | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 請注意 `frontendModule=angular` 屬性。 這會告訴AEM專案原型使用啟動器引導專案 [Angular代碼庫](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) 與AEM SPA編輯器搭配使用。

## 建立專案

接下來，使用Maven編譯、建置專案程式碼，並將其部署至AEM的本機執行個體。

1. 確保AEM例項在本機的連接埠上執行 **4502**.
2. 從命令行終端驗證是否安裝了Maven:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 從 `aem-guides-wknd-spa` 建立專案並部署至AEM的目錄：

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   若使用 [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   應將專案的多個模組編譯並部署至AEM。

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

   Maven的個人資料 ***autoInstallSinglePackage*** 編譯專案的個別模組，並將單一套件部署至AEM例項。 依預設，此套件會部署至本機在連接埠上執行的AEM執行個體 **4502** 而且憑 **admin:admin**.

4. 導覽至 **[!UICONTROL 封裝管理員]** 在本機AEM例項上： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. 您應會看到 `wknd-spa-angular.all`, `wknd-spa-angular.ui.apps` 和 `wknd-spa-angular.ui.content`.

   ![WKND SPA套件](./assets/create-project/package-manager.png)

   專案所需的所有自訂程式碼都整合至這些套件中，並安裝在AEM執行階段。

6. 您也應該會看到 `spa.project.core` 和 `core.wcm.components`. 原型會自動包含這些相依性。 有關 [AEM核心元件位於此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html).

## 製作內容

接下來，開啟由原型產生的啟動器SPA，並更新部分內容。

1. 導覽至 **[!UICONTROL 網站]** 主控台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA包含基本網站結構，內含國家、語言和首頁。 此階層是以原型的預設值為基礎 `language_country` 和 `isSingleCountryWebsite`. 您可以更新 [可用屬性](https://github.com/adobe/aem-project-archetype#available-properties) 產生專案時。

2. 開啟 **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** 頁面，方法是選取頁面並按一下 **[!UICONTROL 編輯]** 按鈕（在菜單欄中）:

   ![網站主控台](./assets/create-project/open-home-page.png)

3. A **[!UICONTROL 文字]** 元件已新增至頁面。 您可以像編輯AEM中的其他元件一樣編輯此元件。

   ![更新文本元件](./assets/create-project/update-text-component.gif)

4. 新增其他 **[!UICONTROL 文字]** 元件。

   請注意，製作體驗與傳統AEM Sites頁面的類似。 目前可使用的元件數目有限。 教學課程中會新增更多內容。

## Inspect單頁應用程式

接下來，使用瀏覽器的開發人員工具確認這是單頁應用程式。

1. 在 **[!UICONTROL 頁面編輯器]**，按一下 **[!UICONTROL 頁面資訊]** 菜單> **[!UICONTROL 檢視為已發佈]**:

   ![查看為已發佈按鈕](./assets/create-project/view-as-published.png)

   這會開啟含有查詢參數的新索引標籤 `?wcmmode=disabled` 這會有效地關閉AEM編輯器： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 檢視頁面的來源，並注意文字內容 **[!DNL Hello World]** 或找不到任何其他內容。 反之，您應該會看到HTML如下：

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

   `clientlib-angular.min.js` 是載入至頁面的AngularSPA，負責轉譯內容。

   *內容來自何處？*

3. 返回索引標籤： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 檢視 **XHR** 要求：

   ![XHR請求](./assets/create-project/xhr-requests.png)

   應要求 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 這包含所有將驅動SPA的內容（以JSON格式）。

5. 在新索引標籤中，開啟 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   請求 `en.model.json` 代表將驅動應用程式的內容模型。 Inspect JSON輸出，您應該就能找到代表 **[!UICONTROL 文字]** 元件。

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

   在下一章中，我們將檢查JSON內容如何從AEM元件對應至SPA元件，以形成AEM SPA編輯器體驗的基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴充功能以自動格式化JSON輸出可能會很有幫助。

## 恭喜！ {#congratulations}

恭喜，您剛剛建立了第一個AEM SPA Editor專案！

現在相當簡單，但在接下來的幾章中，新增了更多功能。

### 後續步驟 {#next-steps}

[整合SPA](integrate-spa.md)  — 了解SPA原始碼如何與AEM專案整合，並了解可用來快速開發SPA的工具。
