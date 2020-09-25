---
title: SPA編輯器項目 | AEM SPA編輯器快速入門與回應
description: 瞭解如何使用Adobe Experience Manager(AEM)Maven專案做為與AEM SPA Editor整合之React應用程式的起點。
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 413
thumbnail: 413-spa-react.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '1128'
ht-degree: 1%

---


# SPA編輯器項目 {#spa-editor-project}

瞭解如何使用Adobe Experience Manager(AEM)Maven專案做為與AEM SPA Editor整合之React應用程式的起點。

## 目標

1. 瞭解以Maven原型建立的新AEM SPA Editor專案的結構。
2. 將起始專案部署至AEM的本機例項。

## 您將建立的

在本章中，將會根據 [AEM Project Archetype部署新的AEM專案](https://github.com/adobe/aem-project-archetype)。 AEM專案將會以React SPA的簡單起點啟動。 本章中使用的項目將作為實施WKND SPA的基礎，並將在今後各章中建立。

![WKND SPA React Starter Project](./assets/create-project/wknd-spa-react.png)

*啟動WKND SPA的網站階層。*

## 必備條件

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。 請確定在作者模式下啟動的Adobe Experience Manager新例 **項** ，正在本機執行。

## 取得專案

有數個選項可用來建立AEM的Maven Multi-module專案。 本教學課程使用最新 [的AEM Project Archetype](https://github.com/adobe/aem-project-archetype) ，做為教學課程程式碼的基礎。 已對專案程式碼進行修改，以支援多個AEM版本。 請參閱 [關於回溯相容性的附註](overview.md#compatibility)。

>[!CAUTION]
>
> 使用最新版原型 **來產生** ，以建立實際實作的新專案， [](https://github.com/adobe/aem-project-archetype) 是最佳實務。 AEM專案應使用原型的屬性來定 `aemVersion` 位單一版本的AEM。

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/create-project-start
   ```

2. 下列檔案夾和檔案結構代表由本機檔案系統上的Maven原型產生的AEM Project:

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

3. 從 [AEM Project原型產生AEM專案時，會使用下列屬性](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14):

   | 屬性 | 值 |
   |-----------------|-------------------------------------|
   | aemVersion | 雲 |
   | appTitle | WKND SPA反應 |
   | appId | wknd-spa-react |
   | groupId | com.adobe.aem.guides |
   | frontendModule | 反應 |
   | package | com.adobe.aem.guides.wknd.spa.react |
   | includeExamples | n |

   >[!NOTE]
   >
   > 請注意 `frontendModule=react` 屬性。 這會告訴AEM Project Archetype使用要與AEM SPA編輯器搭配使用的起始 [React程式碼庫](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) ，來引導專案。

## 建立專案

接著，使用Maven將專案程式碼編譯、建立並部署至AEM的本機執行個體。

1. 請確定AEM例項是在埠 **4502上本機執行**。
2. 從命令行終端驗證Maven是否已安裝：

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 從目錄執行下列Maven命 `aem-guides-wknd-spa` 令，以建立專案並部署至AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   應編譯專案的多個模組並部署至AEM。

   ```plain
    [INFO] ------------------------------------------------------------------------
    [INFO] Reactor Summary for wknd-spa-react 1.0.0-SNAPSHOT:
    [INFO] 
    [INFO] wknd-spa-react ..................................... SUCCESS [  0.523 s]
    [INFO] WKND SPA React - Core .............................. SUCCESS [  8.069 s]
    [INFO] wknd-spa-react.ui.frontend - UI Frontend ........... SUCCESS [01:23 min]
    [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  0.830 s]
    [INFO] WKND SPA React - UI apps ........................... SUCCESS [  4.654 s]
    [INFO] WKND SPA React - UI content ........................ SUCCESS [  1.607 s]
    [INFO] WKND SPA React - All ............................... SUCCESS [  0.384 s]
    [INFO] WKND SPA React - Integration Tests Bundles ......... SUCCESS [  0.770 s]
    [INFO] WKND SPA React - Integration Tests Launcher ........ SUCCESS [  1.407 s]
    [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.055 s]
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  01:44 min
   ```

   Maven描述檔 ***autoInstallSinglePackage*** 會編譯專案的個別模組，並將單一套件部署至AEM例項。 依預設，此套件將部署至在埠 **4502** ，並以 **admin:admin的認證在本機執行的AEM例項**。

4. 導覽至您 **[!UICONTROL 本機AEM例項上的Package Manager]** : [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. 您應該會看到三個 `wknd-spa-react.all`包 `wknd-spa-react.ui.apps` 和 `wknd-spa-react.ui.content`。

   ![WKND SPA套件](./assets/create-project/package-manager.png)

   *AEM Package Manager*

   專案所需的所有自訂程式碼都會整合在這些套件中，並安裝在AEM執行時期中。

6. 您還應看到和的幾個 `spa.project.core` 包 `core.wcm.components`。 原型會自動包含這些相依性。 如需 [AEM核心元件的詳細資訊，請參閱這裡](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)。

   `spa.project.core` 是產生SPA編輯器預期的JSON模型API所需的相依性。

## 作者內容

接著，開啟由原型產生的起始SPA，並更新部分內容。

1. 導覽至 **[!UICONTROL Sites]** Console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA包含基本的網站結構，包括國家、語言和首頁。 此層次結構基於和的原型的預設 `language_country` 值 `isSingleCountryWebsite`。 在產生專案時，可更新可 [用屬性](https://github.com/adobe/aem-project-archetype#available-properties) ，以覆寫這些值。

2. 選取頁 **[!DNL us]** 面並按一 **[!DNL en]** 下功能表列中的「編輯 **[!DNL WKND SPA React Home Page]** 」按鈕，以開啟> **** >頁面：

   ![site console](./assets/create-project/open-home-page.png)

3. 已 **[!UICONTROL 將Text]** 元件新增至頁面。 您可以像AEM中的任何其他元件一樣編輯此元件。

   ![更新文字元件](./assets/create-project/update-text-component.gif)

4. 新增其他 **[!UICONTROL Text]** 元件至頁面。

   請注意，製作體驗與傳統AEM Sites頁麵類似。 目前可使用的元件數目有限。 在教學課程中將會新增更多內容。

## 檢查單頁應用程式

接著，確認這是使用瀏覽器開發人員工具的單頁應用程式。

1. 在「頁 **[!UICONTROL 面編輯器]**」中，按一下「頁面 **[!UICONTROL 資訊]** 」按鈕> **[!UICONTROL 「檢視為已發佈]**:

   ![「檢視為已發佈」按鈕](./assets/create-project/view-as-published.png)

   這會開啟一個含有查詢參數的新標籤， `?wcmmode=disabled` 有效關閉AEM編輯器： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 檢視頁面的來源，並注意到找不到 **[!DNL Hello World]** 文字內容或其他任何內容。 您應該會看到HTML如下：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 是載入至頁面並負責轉譯內容的React SPA。

   但是， *內容來自何處？*

3. 返回頁籤： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 檢視 **XHR請求** :

   ![XHR請求](./assets/create-project/xhr-requests.png)

   應該有http://localhost:4502/content/wknd-spa-react/us/en.model.json的要 [求](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 這包含所有將驅動SPA的JSON格式化內容。

5. 在新索引標籤中，開啟 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   請求代 `en.model.json` 表將驅動應用程式的內容模型。 檢查JSON輸出，您就可以找到代表 **[!UICONTROL Text]** (s)元件的程式碼片段。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   在下一章中，我們將檢查此JSON內容如何從AEM元件對應至SPA元件，以建立AEM SPA編輯器體驗的基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴充功能以自動格式化JSON輸出可能會很有幫助。

## 恭喜！ {#congratulations}

恭喜您，您剛建立了第一個AEM SPA編輯器專案！

SPA很簡單。 在接下來的幾章中，將會新增更多功能。

### 後續步驟 {#next-steps}

[整合SPA](integrate-spa.md) —— 瞭解SPA原始碼如何與AEM Project整合，並瞭解可快速開發SPA的工具。
