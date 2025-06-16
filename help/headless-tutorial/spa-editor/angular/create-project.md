---
title: SPA編輯器專案 | AEM SPA Editor and Angular快速入門
description: 瞭解如何使用Adobe Experience Manager (AEM) Maven專案，開始整合Angular應用程式與AEM SPA Editor。
feature: SPA Editor, AEM Project Archetype
version: Experience Manager as a Cloud Service
jira: KT-5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
duration: 252
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 1%

---

# SPA編輯器專案 {#create-project}

{{spa-editor-deprecation}}

瞭解如何使用Adobe Experience Manager (AEM) Maven專案，開始整合Angular應用程式與AEM SPA Editor。

## 目標

1. 瞭解從Maven原型建立的新AEM SPA Editor專案結構。
2. 將入門專案部署到AEM的本機執行個體。

## 您將建置的內容

在本章中，已根據[AEM專案原型](https://github.com/adobe/aem-project-archetype)部署新的AEM專案。 AEM專案是透過一個非常簡單的Angular SPA起點來啟動。 本章中使用的專案將作為WKND SPA實施的基礎，並在未來的章節中建置。

![WKND SPA Angular入門專案](./assets/create-project/what-you-will-build.png)

*傳統Hello World訊息。*

## 先決條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。 請確定以&#x200B;**作者**&#x200B;模式啟動的Adobe Experience Manager全新執行個體正在本機執行。

## 取得專案

有幾個選項可為AEM建立Maven多模組專案。 此教學課程使用最新的[AEM專案原型](https://github.com/adobe/aem-project-archetype)作為教學課程程式碼的基礎。 已修改專案程式碼，以支援多個AEM版本。 請檢閱[關於回溯相容性的備註](overview.md#compatibility)。

>[!CAUTION]
>
>最佳實務是使用[原型](https://github.com/adobe/aem-project-archetype)的&#x200B;**最新**&#x200B;版本來產生實作的新專案。 AEM專案應該使用原型的`aemVersion`屬性，以單一版本的AEM為目標。

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 以下資料夾和檔案結構代表本機檔案系統上Maven原型產生的AEM專案：

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

3. 從[AEM專案原型](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14)產生AEM專案時，使用了下列屬性：

   | 屬性 | 值 |
   |-----------------|---------------------------------------|
   | aemVersion | 雲端 |
   | appTitle | WKND SPA ANGULAR |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | 封裝 | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 注意`frontendModule=angular`屬性。 這會告訴AEM專案原型使用要與AEM SPA編輯器搭配使用的入門程式[Angular程式碼基底](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html?lang=zh-Hant)，來啟動專案。

## 建置專案

接下來，使用Maven編譯、建置專案程式碼，並將其部署至AEM的本機執行個體。

1. 請確定AEM的執行個體正在連線埠&#x200B;**4502**&#x200B;上本機執行。
2. 從命令列終端機，驗證Maven是否已安裝：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 從`aem-guides-wknd-spa`目錄執行以下Maven命令以建置專案並將其部署到AEM：

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   如果使用[AEM 6.x](overview.md#compatibility)：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   專案的多個模組應編譯並部署至AEM。

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

   Maven設定檔&#x200B;***autoInstallSinglePackage***&#x200B;會編譯專案的個別模組，並將單一套件部署至AEM執行個體。 依預設，此套件會部署至在本機執行於連線埠&#x200B;**4502**&#x200B;上的AEM執行個體，並使用&#x200B;**admin：admin**&#x200B;的認證。

4. 導覽至本機AEM執行個體上的&#x200B;**[!UICONTROL 封裝管理員]**： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

5. 您應該會看到`wknd-spa-angular.all`、`wknd-spa-angular.ui.apps`和`wknd-spa-angular.ui.content`的三個套件。

   ![WKND SPA套件](./assets/create-project/package-manager.png)

   專案所需的所有自訂程式碼都隨附在這些套件中，並安裝在AEM執行階段上。

6. 您應該也會看到`spa.project.core`和`core.wcm.components`的多個套件。 這些是原型自動包含的相依性。 如需[AEM核心元件的詳細資訊，請參閱此處](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)。

## 作者內容

接下來，開啟原型產生的入門SPA並更新部分內容。

1. 導覽至&#x200B;**[!UICONTROL 網站]**&#x200B;主控台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND SPA包含基本網站結構，其中包含國家/地區、語言和首頁。 此階層是根據`language_country`和`isSingleCountryWebsite`的原型預設值。 產生專案時，可以更新[可用的屬性](https://github.com/adobe/aem-project-archetype#available-properties)來覆寫這些值。

2. 選取頁面，然後按一下功能表列中的&#x200B;**[!UICONTROL 編輯]**&#x200B;按鈕，開啟&#x200B;**[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]**&#x200B;頁面：

   ![網站主控台](./assets/create-project/open-home-page.png)

3. **[!UICONTROL Text]**&#x200B;元件已新增至頁面。 您可以像在AEM中編輯任何其他元件一樣編輯此元件。

   ![更新文字元件](./assets/create-project/update-text-component.gif)

4. 新增額外的&#x200B;**[!UICONTROL Text]**&#x200B;元件至頁面。

   請注意，製作體驗類似於傳統AEM Sites頁面的製作體驗。 目前可用的元件數量有限。 在本教學課程中新增更多內容。

## 檢查單頁應用程式

接下來，確認這是使用瀏覽器開發人員工具的單頁應用程式。

1. 在&#x200B;**[!UICONTROL 頁面編輯器]**&#x200B;中，按一下&#x200B;**[!UICONTROL 頁面資訊]**&#x200B;功能表> **[!UICONTROL 以發佈的形式檢視]**：

   ![以發佈的形式檢視按鈕](./assets/create-project/view-as-published.png)

   這將開啟一個具有查詢引數`?wcmmode=disabled`的新索引標籤，這會有效地關閉AEM編輯器： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 檢視頁面的來源，並注意找不到文字內容&#x200B;**[!DNL Hello World]**&#x200B;或任何其他內容。 相反地，您應該會看到類似以下的HTML：

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

   `clientlib-angular.min.js`是載入到頁面上的Angular SPA，負責轉譯內容。

   *內容來自何處？*

3. 返回標籤： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 檢視&#x200B;**XHR**&#x200B;要求：

   ![XHR要求](./assets/create-project/xhr-requests.png)

   應該有對[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)的請求。 這包含所有將驅動SPA的內容（以JSON格式化）。

5. 在新索引標籤中，開啟[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   請求`en.model.json`代表將驅動應用程式的內容模型。 請檢查JSON輸出，您應該能夠找到代表&#x200B;**[!UICONTROL Text]**&#x200B;元件的程式碼片段。

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

   在下一章中，我們將檢查JSON內容如何從AEM元件對應至SPA元件，以構成AEM SPA Editor體驗的基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴充功能以自動格式化JSON輸出可能會有幫助。

## 恭喜！ {#congratulations}

恭喜，您剛才已建立您的第一個AEM SPA Editor專案！

現在相當簡單，但在接下來的幾個章節中會新增更多功能。

### 後續步驟 {#next-steps}

[整合SPA](integrate-spa.md) — 瞭解SPA原始程式碼如何與AEM專案整合，並瞭解可用於快速開發SPA的工具。
