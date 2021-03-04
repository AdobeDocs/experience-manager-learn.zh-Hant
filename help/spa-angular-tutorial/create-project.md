---
title: 編SPA輯器專案 |編輯與AngularAEM入SPA門
description: 瞭解如何使用Adobe Experience Manager(AEM)Maven專案，做為與編輯器整合的Angular應用程式的起SPA點。
sub-product: Sites
feature: 編SPA輯，AEM專案原型
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: 開發人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1117'
ht-degree: 3%

---


# 編SPA輯器項目{#create-project}

瞭解如何使用Adobe Experience Manager(AEM)Maven專案，做為與編輯器整合的Angular應用程式的起SPA點。

## 目標

1. 瞭解從Maven原型建AEM立的SPA新編輯器專案結構。
2. 將起始專案部署至本機例項AEM。

## 您將建立的

在本章中，將根AEM據[項目原型&lt;a1/AEM>部署新項目。 ](https://github.com/adobe/aem-project-archetype)這AEM個項目將啟動，為Angular提供一個非常簡單的起點SPA。 本章中使用的項目將作為執行《世界發展綱領》的基礎SPA，並將在今後各章中建立。

![WKND SPA Angular入門專案](./assets/create-project/what-you-will-build.png)

*經典的Hello World訊息。*

## 必備條件

檢閱設定[本機開發環境](overview.md#local-dev-environment)所需的工具和指示。 請確定在&#x200B;**author**&#x200B;模式下啟動的Adobe Experience Manager新實例正在本地運行。

## 取得專案

為建立Maven多模組項目有幾個選項AEM。 本教學課程使用最新的[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)作為教學課程程式碼的基礎。 已對專案程式碼進行修改，以支援多個版本AEM。 請參閱[關於向後相容性的說明](overview.md#compatibility)。

>[!CAUTION]
>
>使用[archetype](https://github.com/adobe/aem-project-archetype)的&#x200B;**最新**&#x200B;版本，為實際實施產生新專案是最佳實務。 專AEM案應針對使用AEM原型的`aemVersion`屬性的單一版本。

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 以下資料夾和檔案結構表AEM示由本地檔案系統上的Maven原型生成的項目：

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

3. 從[Project archetypeAEM](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14)生成項目時AEM使用了以下屬性：

   | 屬性 | 值 |
   |-----------------|---------------------------------------|
   | aemVersion | 雲 |
   | appTitle | WKND SPAAngular |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | package | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 請注意`frontendModule=angular`屬性。 這會告AEM訴「專案原型」，使用要與編輯器一起使用的起始程式碼庫[Angular程式碼庫&lt;a1/AEMSPA來引導專案。](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)

## 建立專案

接著，編譯、建立專案程式碼，並將它部署至使用Maven的本機AEM執行個體。

1. 確保埠&lt;AEMa0/>4502 **上的實例在本地運行。**
2. 從命令行終端驗證Maven是否已安裝：

   ```shell
   $ mvn --version
    Apache Maven 3.6.2
    Maven home: /Library/apache-maven-3.6.2
    Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 從`aem-guides-wknd-spa`目錄運行以下Maven命令，以生成項目並將其部署到AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   如果使用[AEM 6.x](overview.md#compatibility):

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   應將項目的多個模組編譯並部署到AEM。

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

   Maven配置檔案&#x200B;***autoInstallSinglePackage***&#x200B;將編譯項目的各個模組並將單個軟體包部署到實AEM例。 依預設，此套件將部署至在AEM埠&#x200B;**4502**&#x200B;本機執行的執行個體，並具有&#x200B;**admin:admin**&#x200B;的認證。

4. 導航到您本地實例上的&#x200B;**[!UICONTROL Package Manager]** AEM:[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

5. 您應看到`wknd-spa-angular.all`、`wknd-spa-angular.ui.apps`和`wknd-spa-angular.ui.content`的三個軟體包。

   ![WKND包SPA裝](./assets/create-project/package-manager.png)

   專案所需的所有自訂程式碼都會整合在這些套件中，並安裝在執AEM行時期。

6. 您還應看到`spa.project.core`和`core.wcm.components`的幾個軟體包。 這些是原型自動包含的依賴項。 有關[核心元件AEM的詳細資訊，請參閱此處](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)。

## 作者內容

接著，開啟原SPA型產生的啟動器並更新部分內容。

1. 導覽至&#x200B;**[!UICONTROL Sites]**&#x200B;主控台：[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND包SPA含基本網站結構，包含國家、語言和首頁。 此層次結構基於`language_country`和`isSingleCountryWebsite`原型的預設值。 生成項目時，可通過更新[可用屬性](https://github.com/adobe/aem-project-archetype#available-properties)來覆蓋這些值。

2. 選擇頁面並按一下菜單欄中的&#x200B;**[!UICONTROL 編輯]**&#x200B;按鈕，開啟&#x200B;**[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]**&#x200B;頁：

   ![site console](./assets/create-project/open-home-page.png)

3. **[!UICONTROL Text]**&#x200B;元件已新增至頁面。 您可以像中的任何其他元件一樣編輯此元AEM件。

   ![更新文字元件](./assets/create-project/update-text-component.gif)

4. 新增額外的&#x200B;**[!UICONTROL Text]**&#x200B;元件至頁面。

   請注意，製作體驗與傳統的AEM Sites頁麵類似。 目前可使用的元件數目有限。 在教學課程中將會新增更多內容。

## Inspect單頁應用程式

接著，確認這是使用瀏覽器開發人員工具的單頁應用程式。

1. 在&#x200B;**[!UICONTROL 頁面編輯器]**&#x200B;中，按一下&#x200B;**[!UICONTROL 頁面資訊]**&#x200B;菜單> **[!UICONTROL 查看為發佈]**:

   ![「檢視為已發佈」按鈕](./assets/create-project/view-as-published.png)

   這將開啟一個帶有查詢參數`?wcmmode=disabled`的新頁籤，該參數有效地關閉了編AEM輯器：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 檢視頁面的來源，並注意到找不到文字內容&#x200B;**[!DNL Hello World]**&#x200B;或其他任何內容。 您應該會看到HTML如下：

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

   `clientlib-angular.min.js` 是載入SPA至頁面並負責轉譯內容的Angular。

   *內容來自何處？*

3. 返回頁籤：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 查看&#x200B;**XHR**&#x200B;請求：

   ![XHR請求](./assets/create-project/xhr-requests.png)

   應該有對[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)的請求。 這包含所有將會驅動的JSON格式化內容SPA。

5. 在新標籤中，開啟[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   請求`en.model.json`代表將驅動應用程式的內容模型。 InspectJSON輸出，您應該可以找到代表&#x200B;**[!UICONTROL Text]**&#x200B;元件的程式碼片段。

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

   在下一章中，我們將檢視JSON內容如何從「元件」對應AEM至「元SPA件」，以形成AEMEditor體SPA驗。

   >[!NOTE]
   >
   > 安裝瀏覽器擴充功能以自動格式化JSON輸出可能會很有幫助。

## 恭喜！{#congratulations}

恭喜您，您剛建立了您的第一AEM個編輯SPA專案！

現在非常簡單，但在接下來的幾章中，將添加更多功能。

### 後續步驟{#next-steps}

[整合SPA-了SPA解原始碼與專案的整AEM合方式，並瞭解可快速開發的SPA工具。](integrate-spa.md) 
