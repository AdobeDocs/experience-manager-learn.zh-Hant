---
title: 建立專案 |開始使用AEM SPA Editor and React
description: 了解如何產生Adobe Experience Manager(AEM)Maven專案，作為與AEM SPA編輯器整合的React應用程式的起點。
sub-product: Sites
feature: SPA編輯器， AEM專案原型
version: cloud-service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 1%

---


# 建立專案 {#spa-editor-project}

了解如何產生Adobe Experience Manager(AEM)Maven專案，作為與AEM SPA編輯器整合的React應用程式的起點。

## 目標

1. 使用SPA專案原型產生已啟用AEM編輯器的專案。
2. 將入門專案部署至本機的AEM例項。

## 您將建置的 {#what-build}

在本章中，將根據[AEM專案原型](https://github.com/adobe/aem-project-archetype)產生新的AEM專案。 AEM專案將以React SPA的簡單起點啟動。

**什麼是Maven專案？** -  [Apache ](https://maven.apache.org/) Mavenis是用於建立專案的軟體管理工具。*所有Adobe Experience Manager實* 作都會使用Maven專案，在AEM上建置、管理和部署自訂程式碼。

**什麼是Maven原型？** - Maven原 [型](https://maven.apache.org/archetype/index.html) 是產生新專案的範本或模式。AEM專案原型可讓我們使用自訂命名空間產生新專案，並包含遵循最佳實務的專案結構，大幅加速我們的專案。

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。 請確定以&#x200B;**author**&#x200B;模式啟動的新Adobe Experience Manager例項在本機執行。

## 建立專案 {#create}

>[!NOTE]
>
>本教學課程使用原型的&#x200B;**27**&#x200B;版本。 使用原型的&#x200B;**最新**&#x200B;版本產生新專案，一律是最佳作法。

1. 開啟命令行終端並輸入以下Maven命令：

   ```shell
   mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=27 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 如果目標AEM 6.5.5+將`aemVersion="cloud"`取代為`aemVersion="6.5.5"`。 如果目標為6.4.8+，請使用`aemVersion="6.4.8"`。

   請注意`frontendModule=react`屬性。 這會告訴AEM專案原型以使用要與AEM SPA編輯器搭配使用的起始程式碼基底[React程式碼基底](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)引導專案。 `appTitle`、`appId`、`artifactId`和`groupId`等屬性用於識別專案和用途。

   可在此](https://github.com/adobe/aem-project-archetype#available-properties)找到用於配置項目[的可用屬性的完整清單。

1. 您的本機檔案系統上的Maven原型將產生下列資料夾和檔案結構：

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- all/
       |--- analyse/
       |--- core/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- it.tests/
       |--- dispatcher/
       |--- analyse/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
   ```

   每個資料夾代表個別的Maven模組。 在本教學課程中，我們將主要使用`ui.frontend`模組（即React應用程式）。 如需個別模組的詳細資訊，請參閱[AEM專案原型檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)。

## 部署並建置專案

接下來，使用Maven編譯、建置專案程式碼，並將其部署至AEM的本機執行個體。

1. 請確定AEM例項在連接埠&#x200B;**4502**&#x200B;上本機執行。
1. 從命令行導航到`aem-guides-wknd-spa.react`項目目錄。

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 執行下列命令以建立整個專案並部署至AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   組建需要約一分鐘的時間，且應會以下列訊息結束：

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven設定檔`autoInstallSinglePackage`會編譯專案的個別模組，並將單一套件部署至AEM例項。 預設情況下，此包將部署到本地運行在埠&#x200B;**4502**&#x200B;上且憑據為`admin:admin`的AEM實例。

1. 導覽至您本機AEM例項上的&#x200B;**套件管理器**:[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

1. 應該會看到多個包的前置詞為`aem-guides-wknd-spa.react`。

   ![WKND SPA套件](assets/create-project/package-manager.png)

   *AEM Package Manager*

   專案所需的所有自訂程式碼都會整合至這些套件中，並安裝在AEM環境中。

## 製作內容

接下來，開啟由原型產生的啟動器SPA，並更新部分內容。

1. 導覽至&#x200B;**Sites**&#x200B;主控台：[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND SPA包含基本網站結構，內含國家、語言和首頁。 此階層是以`language_country`和`isSingleCountryWebsite`的原型預設值為基礎。 在產生專案時，可更新[可用屬性](https://github.com/adobe/aem-project-archetype#available-properties)以覆寫這些值。

2. 通過選擇該頁並按一下菜單欄中的&#x200B;**編輯**&#x200B;按鈕，開啟&#x200B;**us** > **en** > **WKND SPA React首頁**&#x200B;頁：

   ![網站主控台](./assets/create-project/open-home-page.png)

3. 已將&#x200B;**Text**&#x200B;元件新增至頁面。 您可以像編輯AEM中的其他元件一樣編輯此元件。

   ![更新文本元件](./assets/create-project/update-text-component.gif)

4. 將其他&#x200B;**Text**&#x200B;元件新增至頁面。

   請注意，製作體驗與傳統AEM Sites頁面的類似。 目前可使用的元件數目有限。 在本教學課程中將新增更多內容。

## Inspect單頁應用程式

接下來，使用瀏覽器的開發人員工具確認這是單頁應用程式。

1. 在&#x200B;**頁面編輯器**&#x200B;中，按一下&#x200B;**頁面資訊**&#x200B;按鈕> **檢視為已發佈**:

   ![查看為已發佈按鈕](./assets/create-project/view-as-published.png)

   這會開啟一個查詢參數`?wcmmode=disabled`的新索引標籤，有效地關閉AEM編輯器：[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 檢視頁面的來源，並注意找不到文字內容&#x200B;**[!DNL Hello World]**&#x200B;或其他任何內容。 反之，您應該會看到如下的HTML:

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 是載入至頁面並負責轉譯內容的React SPA。

   不過，*內容來自何處？*

3. 返回索引標籤：[http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 檢視&#x200B;**XHR**&#x200B;請求：

   ![XHR請求](./assets/create-project/xhr-requests.png)

   應該會有[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)的要求。 這包含所有將驅動SPA的內容（以JSON格式）。

5. 在新索引標籤中，開啟[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   請求`en.model.json`代表將驅動應用程式的內容模型。 Inspect JSON輸出，您應該能夠找到代表&#x200B;**[!UICONTROL Text]**&#x200B;元件的程式碼片段。

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

   在下一章中，我們將檢查此JSON內容如何從AEM元件對應至SPA元件，以形成AEM SPA編輯器體驗的基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴充功能以自動格式化JSON輸出可能會很有幫助。

## 恭喜！ {#congratulations}

恭喜，您剛剛建立了第一個AEM SPA Editor專案！

SPA很簡單。 在接下來的幾章中，將新增更多功能。

### 後續步驟 {#next-steps}

[整合SPA](integrate-spa.md)  — 了解SPA原始碼如何與AEM專案整合，並了解可用來快速開發SPA的工具。
