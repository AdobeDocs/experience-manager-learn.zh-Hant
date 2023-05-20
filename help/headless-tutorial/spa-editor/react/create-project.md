---
title: 建立項目 |從編輯器AEM開始SPA並反應
description: 瞭解如何將Adobe Experience Manager(AEM)Maven項目作為與編輯器整合的React應用程式的AEM起點SPA。
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: c489a033f34aecaa0af10e3868c258feba6aaae6
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 2%

---

# 建立專案 {#spa-editor-project}

瞭解如何將Adobe Experience Manager(AEM)Maven項目作為與編輯器整合的React應用程式的AEM起點SPA。

## 目標

1. 使用「項SPA目原型」生成啟用「編輯AEM器」的項目。
2. 將啟動程式項目部署到的本地實例AEM。

## 您將構建的 {#what-build}

在本章中，將根AEM據 [項AEM目原型](https://github.com/adobe/aem-project-archetype)。 該項AEM目正在啟動，其起點非常簡單SPA。

**什麼是Maven項目？** - [阿帕奇·馬文](https://maven.apache.org/) 是用於構建項目的軟體管理工具。 *全Adobe Experience Manager* 實現使用Maven項目在上面構建、管理和部署自定義代AEM碼。

**什麼是馬文原型？** - A [馬文原型](https://maven.apache.org/archetype/index.html) 是用於生成新項目的模板或模式。 「項AEM目」原型允許我們使用自定義命名空間生成新項目，並包括遵循最佳實踐的項目結構，這大大加快了我們的項目。

## 必備條件

查看所需的工具和設定 [地方開發環境](overview.md#local-dev-environment)。 確保新的Adobe Experience Manager實例 **作者** 模式，正在本地運行。

## 建立項目 {#create}

>[!NOTE]
>
>本教程使用版本 **35** 原型。

1. 開啟命令行終端並輸入以下Maven命令：

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=35 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 如果以AEM6.5.5+替換為目標 `aemVersion="cloud"` 與 `aemVersion="6.5.5"`。 如果目標為6.4.8+，請使用 `aemVersion="6.4.8"`。

   注意 `frontendModule=react` 屬性。 這將指示項AEM目原型使用啟動程式引導項目 [反應代碼基](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) 與編輯器一AEM起SPA使用。 類似屬性 `appTitle`。 `appId`。 `artifactId`, `groupId` 用於標識項目和目的。

   用於配置項目的可用屬性的完整清單 [可在此處找到](https://github.com/adobe/aem-project-archetype#available-properties)。

1. 以下資料夾和檔案結構由本地檔案系統上的Maven原型生成：

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   每個資料夾都代表單個Maven模組。 在本教程中，我們將主要使用 `ui.frontend` 模組，即React應用。 有關各個模組的詳細資訊，請參閱 [項AEM目原型文檔](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)。

## 部署和生成項目

接下來，編譯、生成項目代碼並將其部署到使用Maven的本AEM地實例。

1. 確保埠上AEM的實例正在本地運行 **4502**。
1. 從命令行導航到 `aem-guides-wknd-spa.react` 項目目錄。

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 運行以下命令以構建和部署整個項目AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   生成大約需要一分鐘時間，應以以下消息結束：

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

   馬文檔案 `autoInstallSinglePackage` 編譯項目的各個模組，並將單個包部署到實AEM例。 預設情況下，此包部署到AEM埠上本地運行的實例 **4502** 還有 `admin:admin`。

1. 導航到 **包管理器** 在本地實例AEM上： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

1. 您應看到多個包的前置詞 `aem-guides-wknd-spa.react`。

   ![WKND包SPA](assets/create-project/package-manager.png)

   *包管AEM理器*

   項目所需的所有自定義代碼都捆綁到這些軟體包中並安裝在AEM環境中。

## 作者內容

接下來，開啟SPA原型生成的啟動程式並更新部分內容。

1. 導航到 **站點** 控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND包括SPA一個基本的站點結構，包括國家/地區、語言和首頁。 此層次結構基於的原型的預設值 `language_country` 和 `isSingleCountryWebsite`。 通過更新 [可用屬性](https://github.com/adobe/aem-project-archetype#available-properties) 生成項目時。

2. 開啟 **美國** > **恩** > **WKND反SPA應首頁** 頁面 **編輯** 按鈕：

   ![站點控制台](./assets/create-project/open-home-page.png)

3. A **文本** 元件已添加到頁面。 可以像中的任何其他元件一樣編輯此組AEM件。

   ![更新文本元件](./assets/create-project/update-text-component.gif)

4. 添加附加 **文本** 元件。

   請注意，創作體驗與傳統的AEM Sites頁麵類似。 目前可用的元件數量有限。 在本教程中將添加更多內容。

## Inspect單頁應用程式

接下來，使用瀏覽器的開發人員工具驗證這是單頁應用程式。

1. 在 **頁面編輯器**，按一下 **頁面資訊** 按鈕> **查看為已發佈**:

   ![「查看為已發佈」按鈕](./assets/create-project/view-as-published.png)

   這將開啟一個帶有查詢參數的新頁籤 `?wcmmode=disabled` 有效地關閉了編AEM輯器： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 查看頁面的源並注意文本內容 **[!DNL Hello World]** 或找不到任何其他內容。 相反，您應看到HTML，如下所示：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 是載入SPA到頁面並負責呈現內容的React。

   但是， *內容從何而來？*

3. 返回到頁籤： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在刷新期間檢查頁面的網路流量。 查看 **XHR** 請求：

   ![XHR請求](./assets/create-project/xhr-requests.png)

   應該有一個 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。 它包含所有將驅動的JSON格式內SPA容。

5. 在開啟的新頁籤中 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   請求 `en.model.json` 表示驅動應用程式的內容模型。 InspectJSON輸出，您應該能夠找到代表 **[!UICONTROL 文本]** 元件。

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

   在下一章中，我們將檢查此JSON內容如何從元件映AEM射到SPA元件，以形成編輯器體AEM驗的SPA基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴展以自動格式化JSON輸出可能會有所幫助。

## 恭喜！ {#congratulations}

祝賀您，您剛剛建立了第AEM一個SPA編輯項目！

很SPA簡單。 在接下來的幾章中添加了更多功能。

### 後續步驟 {#next-steps}

[集SPA成](integrate-spa.md)  — 瞭解源SPA代碼如何與項目集AEM成，並瞭解可用於快速開發的工SPA具。
