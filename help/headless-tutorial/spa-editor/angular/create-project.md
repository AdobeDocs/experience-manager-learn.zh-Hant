---
title: SPA編輯器專案 |開始使用AEM SPA編輯器和Angular
description: 了解如何使用Adobe Experience Manager(AEM)Maven專案，作為與AEM SPA編輯器整合之Angular應用程式的起點。
sub-product: Sites
feature: SPA編輯器， AEM專案原型
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: bf9ab30f57faa23721d7d27b837d8e0f0e8cf4f1
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 2%

---


# SPA編輯器專案{#create-project}

了解如何使用Adobe Experience Manager(AEM)Maven專案，作為與AEM SPA編輯器整合之Angular應用程式的起點。

## 目標

1. 了解從Maven原型建置的新AEM SPA編輯器專案的結構。
2. 將入門專案部署至本機的AEM例項。

## 您將建置的

在本章中，將根據[AEM專案原型](https://github.com/adobe/aem-project-archetype)部署新的AEM專案。 AEM專案將以AngularSPA的簡單起點啟動。 本章中使用的項目將作為實施WKND SPA的基礎，並將在以後的章節中構建。

![WKND SPAAngular入門專案](./assets/create-project/what-you-will-build.png)

*經典的「你好世界」訊息。*

## 必備條件

查看設定[本地開發環境](overview.md#local-dev-environment)所需的工具和說明。 請確定以&#x200B;**author**&#x200B;模式啟動的新Adobe Experience Manager例項在本機執行。

## 取得專案

為AEM建立Maven多模組專案有數個選項。 本教學課程使用最新的[AEM專案原型](https://github.com/adobe/aem-project-archetype)作為教學課程程式碼的基礎。 已修改專案程式碼，以支援多個版本的AEM。 請查看[關於向後相容性的說明](overview.md#compatibility)。

>[!CAUTION]
>
>最佳實務是使用[原型](https://github.com/adobe/aem-project-archetype)的&#x200B;**最新**&#x200B;版本，為實際實作產生新專案。 AEM專案應使用原型的`aemVersion`屬性來鎖定單一AEM版本。

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

3. 從[AEM專案原型](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14)產生AEM專案時，已使用下列屬性：

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
   > 請注意`frontendModule=angular`屬性。 這會告訴AEM專案原型引導專案，使用要與AEM SPA編輯器搭配使用的起始程式碼基底[Angular程式碼基底](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

## 建立專案

接下來，使用Maven編譯、建置專案程式碼，並將其部署至AEM的本機執行個體。

1. 請確定AEM例項在連接埠&#x200B;**4502**&#x200B;上本機執行。
2. 從命令行終端驗證是否安裝了Maven:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 從`aem-guides-wknd-spa`目錄中運行以下Maven命令以生成項目並將其部署到AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   如果使用[AEM 6.x](overview.md#compatibility):

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

   Maven配置檔案&#x200B;***autoInstallSinglePackage***&#x200B;編譯項目的各個模組，並將單個包部署到AEM實例。 依預設，此套件會部署至本機在連接埠&#x200B;**4502**&#x200B;上執行，且憑證為&#x200B;**admin:admin**&#x200B;的AEM執行個體。

4. 導覽至您本機AEM例項上的&#x200B;**[!UICONTROL 套件管理器]**:[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。

5. 您應該會看到`wknd-spa-angular.all`、`wknd-spa-angular.ui.apps`和`wknd-spa-angular.ui.content`的三個套件。

   ![WKND SPA套件](./assets/create-project/package-manager.png)

   專案所需的所有自訂程式碼都會整合至這些套件中，並安裝在AEM執行階段。

6. 您也應該會看到`spa.project.core`和`core.wcm.components`的數個套件。 原型會自動包含這些相依性。 如需[AEM核心元件的詳細資訊，請前往這裡](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)。

## 製作內容

接下來，開啟由原型產生的啟動器SPA，並更新部分內容。

1. 導覽至&#x200B;**[!UICONTROL Sites]**&#x200B;主控台：[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。

   WKND SPA包含基本網站結構，內含國家、語言和首頁。 此階層是以`language_country`和`isSingleCountryWebsite`的原型預設值為基礎。 在產生專案時，可更新[可用屬性](https://github.com/adobe/aem-project-archetype#available-properties)以覆寫這些值。

2. 選擇頁面並按一下菜單欄中的&#x200B;**[!UICONTROL Edit]**&#x200B;按鈕，開啟&#x200B;**[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]**&#x200B;頁：

   ![網站主控台](./assets/create-project/open-home-page.png)

3. 已將&#x200B;**[!UICONTROL Text]**&#x200B;元件新增至頁面。 您可以像編輯AEM中的其他元件一樣編輯此元件。

   ![更新文本元件](./assets/create-project/update-text-component.gif)

4. 將其他&#x200B;**[!UICONTROL Text]**&#x200B;元件新增至頁面。

   請注意，製作體驗與傳統AEM Sites頁面的類似。 目前可使用的元件數目有限。 在本教學課程中將新增更多內容。

## Inspect單頁應用程式

接下來，使用瀏覽器的開發人員工具確認這是單頁應用程式。

1. 在&#x200B;**[!UICONTROL 頁面編輯器]**&#x200B;中，按一下&#x200B;**[!UICONTROL 頁面資訊]**&#x200B;功能表> **[!UICONTROL 檢視為已發佈]**:

   ![查看為已發佈按鈕](./assets/create-project/view-as-published.png)

   這會開啟一個查詢參數`?wcmmode=disabled`的新索引標籤，有效地關閉AEM編輯器：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 檢視頁面的來源，並注意找不到文字內容&#x200B;**[!DNL Hello World]**&#x200B;或其他任何內容。 反之，您應該會看到如下的HTML:

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

3. 返回索引標籤：[http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 檢視&#x200B;**XHR**&#x200B;請求：

   ![XHR請求](./assets/create-project/xhr-requests.png)

   應該會有[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)的要求。 這包含所有將驅動SPA的內容（以JSON格式）。

5. 在新索引標籤中，開啟[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   請求`en.model.json`代表將驅動應用程式的內容模型。 Inspect JSON輸出，您應該能夠找到代表&#x200B;**[!UICONTROL Text]**&#x200B;元件的程式碼片段。

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

現在相當簡單，但在接下來的幾章中，將新增更多功能。

### 後續步驟{#next-steps}

[整合SPA](integrate-spa.md)  — 了解SPA原始碼如何與AEM專案整合，並了解可用來快速開發SPA的工具。