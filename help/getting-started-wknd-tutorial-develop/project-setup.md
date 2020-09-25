---
title: AEM Sites快速入門——專案設定
seo-title: AEM Sites快速入門——專案設定
description: 涵蓋建立Maven Multi Module Project以管理AEM網站的程式碼和設定。
sub-product: sites
feature: maven-archetype
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '2468'
ht-degree: 1%

---


# 專案設定 {#project-setup}

本教學課程涵蓋建立Maven Multi Module專案，以管理Adobe Experience Manager網站的程式碼和組態。

## 必備條件 {#prerequisites}

檢視建立本機開發環境所需的工 [具和指示](overview.md#local-dev-environment)。 請確定您在本機有新的Adobe Experience Manager實例，且未安裝其他範例／示範套件（除必要的Service Pack外）。

## 目標 {#objective}

1. 瞭解如何使用Maven原型產生新的AEM專案。
1. 瞭解AEM Project Archetype產生的不同模組，以及它們如何搭配運作。
1. 瞭解AEM Core Components如何包含在AEM Project中。

## 您將建立的 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

在本章中，您將使用 [AEM Project Archetype產生新的Adobe Experience Manager專案](https://github.com/adobe/aem-project-archetype)。 您的AEM專案包含用於網站實作的所有程式碼、內容和設定。 本章中生成的項目將作為實施WKND網站的基礎，並將在今後各章中建立。

## 背景 {#background}

**什麼是Maven專案？** - [Apache Maven](https://maven.apache.org/) 是用於建立專案的軟體管理工具。 *所有Adobe Experience Manager實作都會使用Maven專案，在AEM之上建立、管理和部署自訂程式碼。*

**什麼是馬文原型？** - Maven原 [型](https://maven.apache.org/archetype/index.html) ，是產生新專案的範本或模式。 AEM Project原型可讓我們使用自訂命名空間產生新專案，並包含遵循最佳實務的專案結構，大幅加速我們的專案。

## 建立專案 {#create}

有幾個選項可用來建立適用於AEM的Maven Multi-module專案。 本教學課程將運用 [Maven AEM Project Archetype **22**](https://github.com/adobe/aem-project-archetype)。 Cloud Manager也提 [供UI精靈](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/create-an-application-project.html) ，以開始建立AEM應用程式專案。 由Cloud Manager UI產生的基礎專案會產生與直接使用原型相同的結構。

>[!NOTE]
>
>為進行本教學課程之目的，請使用 **原型** 22版。 不過，使用最新版原型來產生新專案 **永遠是** 最佳的作法。

下一系列步驟將使用基於UNIX的命令行終端進行，但使用Windows終端時應類似。

1. 開啟命令行終端並驗證Maven是否已安裝並添加到命令行路徑：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 執行下列命 **令，確認adobe-public** 描述檔是否作用中：

   ```shell
   $ mvn help:effective-settings
       ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   如果您 **未看** 到 **adobe-public** ，表示您的檔案中未正確參考Adobe回 `~/.m2/settings.xml` 購。 請重新造訪在本機開發環境中安裝和設 [定Apache Maven的步驟](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven)。

1. 導覽至您要產生AEM專案的目錄。 此目錄可以是您要維護專案原始碼的任何目錄。 例如，在用戶的 `code` 主目錄下命名的目錄：

   ```shell
   $ cd ~/code
   ```

1. 將下列內容貼入命令列， [以批次模式產生專案](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   $ mvn archetype:generate -B \
       -DarchetypeGroupId=com.adobe.granite.archetypes \
       -DarchetypeArtifactId=aem-project-archetype \
       -DarchetypeVersion=22 \
       -DgroupId=com.adobe.aem.guides \
       -Dversion=0.0.1-SNAPSHOT \
       -DappsFolderName=wknd \
       -DartifactId=aem-guides-wknd \
       -Dpackage=com.adobe.aem.guides.wknd \
       -DartifactName="WKND Sites Project" \
       -DcomponentGroupName=WKND \
       -DconfFolderName=wknd \
       -DcontentFolderName=wknd \
       -DcssId=wknd \
       -DisSingleCountryWebsite=n \
       -Dlanguage_country=en_us \
       -DoptionAemVersion=6.5.0 \
       -DoptionDispatcherConfig=none \
       -DoptionIncludeErrorHandler=n \
       -DoptionIncludeExamples=y \
       -DoptionIncludeFrontendModule=y \
       -DpackageGroup=wknd \
       -DsiteName="WKND Site"
   ```

   >[!NOTE]
   >
   >根據預設，從Maven原型產生專案會使用互動模式。 為避免使用批次模式產生任何值的大分割。 您也可以使用 [AEM Developer Tools plugin for Eclipse來建立Maven AEM專案](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/aem-eclipse.html)。

   >[!CAUTION]
   >
   >如果您收到類似下列的錯誤： *無法在專案standalone-pom上執行goal org.apache.maven.plugins:maven-archetype-plugin:3.1.1:generate(default-cli):所需的原型不存在*。 這表示您的檔案中未正確參考Adobe回購 `~/.m2/settings.xml` 程式。 請重新造訪之前的步驟，並驗證settings.xml檔案是否參照Adobe repo。

   下表列出本教學課程所使用的值：

   | 名稱 | 值 | 說明 |
   |-----------------------------|---------|--------------------|
   | groupId | com.adobe.aem.guides | 基本Maven groupId |
   | artifactId | aem-guides-wknd | 基本Maven ArtifactId |
   | 版本 | 0.0.1-快照 | 版本 |
   | package | com.adobe.aem.guides.wknd | Java源包 |
   | appsFolderName | wknd | /apps資料夾名稱 |
   | artifactName | WKND Sites Project | Maven項目名稱 |
   | componentGroupName | WKND | AEM元件群組名稱 |
   | contentFolderName | wknd | /content資料夾名稱 |
   | confFolderName | wknd | /conf資料夾名稱 |
   | cssId | wknd | 生成的css中使用的前置詞 |
   | packageGroup | wknd | 內容套件群組名稱 |
   | siteName | WKND網站 | AEM網站名稱 |
   | optionAemVersion | 6.5.0 | Target AEM版本 |
   | language_country | en_us | 語言／國家／地區代碼，以建立內容結構（例如en_us） |
   | optionIncludeExamples | y | 包含元件庫範例網站 |
   | optionIncludeErrorHandler | n | 包含自訂404回應頁面 |
   | optionIncludeFrontendModule | y | 包含專用的前端模組 |
   | isSingleCountryWebsite | n | 在範例內容中建立語言主版結構 |
   | optionDispatcherConfig | 無 | 生成調度器配置模組 |

1. 以下資料夾和檔案結構將由本地檔案系統上的Maven原型生成：

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.content/
           |--- ui.frontend /
           |--- it.launcher/
           |--- it.tests/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 建立專案 {#build}

現在，我們已產生新專案，可以將專案程式碼部署至AEM的本機執行個體。

1. 請確定您有AEM執行個體在埠 **4502上執行**。
1. 從命令行導航到項目 `aem-guides-wknd` 目錄。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 執行下列命令以建立整個專案並部署至AEM:

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven描述檔 `autoInstallSinglePackage` 會編譯專案的個別模組，並將單一套件部署至AEM例項。 依預設，此套件將部署至在埠 **4502** ，且憑證為的本機執行的AEM例項 `admin:admin`。

1. 導覽至您本機AEM例項上的Package Manager: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您應該看到三個包 `aem-guides-wknd.ui.apps`，包 `aem-guides-wknd.ui.content`括、和 `aem-guides-wknd.all`。

   您也應該會看到 [AEM Core Components的多個套件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html) ，這些套件會依原型包含在專案中。 這將在教學課程的稍後部分介紹。

1. 導覽至「網站」主控台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). WKND網站將是其中一個網站。 其中將包含具有美國和語言碩士階層的網站結構。 此網站階層是以使用原型產生專 `language_country` 案時 `isSingleCountryWebsite` 的值為基礎。

1. 選取頁 **面並按一下功** 能表列中的「編 `>` 輯 ******** 」按鈕，以開啟「美國英文」頁面：

   ![site console](assets/project-setup/aem-sites-console.png)

1. 有些內容已經建立，而有數個元件可供新增至頁面。 嘗試這些元件，以瞭解其功能。 本教學課程稍後將詳細探討本頁面和元件的設定方式。

## 檢查專案 {#project-structure}

AEM原型由個別的Maven模組組成：

* [core](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html) - Java套件包含所有核心功能，例如OSGi服務、監聽程式或排程程式，以及元件相關的Java程式碼，例如servlet或請求篩選器。
* [ui.apps](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html) —— 包含專案的/apps部分，即JS&amp;CSSclientlibs、元件和OSGi設定
* [ui.content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.html) —— 包含結構內容和組態，例如可編輯範本、中繼資料結構(/content、/conf)
* ui.tests —— 包含伺服器端執行的JUnit測試的Java包。 此套件不會部署在生產上。
* ui.launcher —— 包含將ui.tests捆綁包（和從屬捆綁包）部署到伺服器並觸發遠程JUnit執行的粘合代碼
* [ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html) -（可選）包含使用基於Webpack的前端構建模組所需的對象。
* all —— 此為空的Maven模組，將上述模組結合為可部署至AEM環境的單一套件。

![Maven項目圖](assets/project-setup/project-pom-structure.png)

請參閱 [AEM Project Archetype檔案](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html) ，以進一步瞭解Maven模組的詳細資訊。

## 進階的Maven命令 {#advanced-maven-commands}

在開發期間，您可能只使用其中一個模組，並想要避免建立整個專案，以節省時間。 您也可以直接部署至AEM Publish執行個體，或是部署至未在埠4502上執行的AEM執行個體。

接下來，我們將討論一些額外的Maven描述檔和指令，讓您在開發時有更大的彈性。

### 核心模組 {#core-module}

核 **[心模組](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/core.html)** ，包含與項目關聯的所有Java代碼。 建立後，它會將OSGi搭售部署至AEM。 要僅構建此模組：

1. 導覽至資 `core` 料夾( `aem-guides-wknd`下方):

   ```shell
   $ cd core/
   ```

1. 運行以下命令：

   ```shell
   $ mvn -PautoInstallBundle clean install
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   [INFO] Finished at: 2019-12-06T13:40:21-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 導覽至 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)。 這是OSGi Web主控台，並包含AEM例項上所有已安裝之套件的相關資訊。

1. 切換 **Id排序欄** ，您應該會看到WKND搭售已安裝並啟用。

   ![核心套件](assets/project-setup/wknd-osgi-console.png)

1. 在 [CRXDE-Lite中，您可以看到jar的「物理」位置](http://localhost:4502/crx/de/index.jsp#/apps/wknd/install/wknd-sites-guide.core-0.0.1-SNAPSHOT.jar):

   ![Jar的CRXDE位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模組 {#apps-content-module}

ui.apps **[maven模組包含下方網站所需的所有轉換程式碼](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uiapps.html)**`/apps`。 這包括將儲存為AEM格式（稱為clientlibs）的CSS/ [JS](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)。 這也包含 [用於轉譯](https://docs.adobe.com/docs/en/htl/overview.html) 動態HTML的HTL指令碼。 您可以將 **** ui.apps模組視為JCR中結構的映射，但格式可儲存在檔案系統上並提交至來源控制項。 ui. **apps模組僅包含程式碼** 。

要構建此模組：

1. 從命令行。 導覽至資 `ui.apps` 料夾( `aem-guides-wknd`下方):

   ```shell
   $ cd ../ui.apps
   ```

1. 運行以下命令：

   ```shell
   $ mvn -PautoInstallPackage clean install
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 導覽至 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您應該將包看 `ui.apps` 作第一個安裝的包，它應具有比其他任何包更新的時間戳。

   ![已安裝UI.apps套件](assets/project-setup/ui-apps-package.png)

1. 返回到命令行並運行以下命令(在資料夾 `ui.apps` 中):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.717 s
   [INFO] Finished at: 2019-12-06T14:51:45-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   配置文 `autoInstallPackagePublish` 件用於將軟體包部署到在埠 **4503上運行的發佈環境**。 如果找不到在http://localhost:4503上執行的AEM例項，則會出現上述錯誤。

1. 最後，運行以下命令將軟體包部 `ui.apps` 署在埠 **4504上**:

   ```shell
   $ mvn -PautoInstallPackage clean install -Daem.port=4504
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/dgordon/code/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4504/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] --------------------------------------------------------------------
   ```

   如果埠4504上沒有運行的AEM實例可用，則預計會發 **生建立** 失敗。 參數 `aem.port` 在POM檔案中定義於 `aem-guides-wknd/pom.xml`。

ui. **[content](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uicontent.htm)** 模組的結構與 **ui.apps模組相同** 。 唯一的不同是 **ui.content** 模組包含所謂的可 **變內容** 。 **可變內容** (Mutable content)主要是指非程式碼組態，例如儲存在原始碼控制項中，但可直接在AEM例項上修改的範本、原則 **** ，或資料夾結構。 在「頁面與範本」一章中，將會更詳細地探討此問題。 目前，重要的一點是，用來建立 **ui.apps模組的相同Maven指令可用來建立** ui.content **** 模組。 您可從ui.content資料夾重複上述 **步驟** 。

### UI.frontend模組 {#ui-frontend-module}

ui. **[frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模組是Maven模組，實際上是 [webpack](https://webpack.js.org/) 專案。 此模組設定為專用的前端建置系統，可輸出JavaScript和CSS檔案，然後再部署至AEM。 The **ui.frontend** module for developers to code with languages [Sass](https://sass-lang.com/), [TypeScript](https://www.typescriptlang.org/), use [](https://www.npmjs.com/) npm modules and integrate the output directly into AEM.

在 **** 用戶端程式庫和前端開發的章節中，將會更詳細地介紹ui.frontend模組。 現在，讓我們來看看它是如何與項目整合的。

1. 從命令行。 導覽至資 `ui.frontend` 料夾( `aem-guides-wknd`下方):

   ```shell
   $ cd ../ui.frontend
   ```

1. 運行以下命令：

   ```shell
   $ mvn clean install
   ...
   [INFO] write clientlib asset txt file (type: js): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js.txt
   [INFO] copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   [INFO]
   [INFO] write clientlib asset txt file (type: css): ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt
   [INFO] copy: dist/clientlib-site/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   [INFO]
   [INFO] --- maven-assembly-plugin:3.1.1:single (default) @ aem-guides-wknd.ui.frontend ---
   [INFO] Reading assembly descriptor: assembly.xml
   [INFO] Building zip: /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO]
   [INFO] --- maven-install-plugin:2.5.2:install (default-install) @ aem-guides-wknd.ui.frontend ---
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/pom.xml to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.pom
   [INFO] Installing /Users/dgordon/code/aem-guides-wknd/ui.frontend/target/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip to /Users/dgordon/.m2/repository/com/adobe/aem/guides/aem-guides-wknd.ui.frontend/0.0.1-SNAPSHOT/aem-guides-wknd.ui.frontend-0.0.1-SNAPSHOT.zip
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  13.520 s
   [INFO] Finished at: 2019-12-06T15:26:16-08:00
   ```

   請注意這些行 `copy: dist/clientlib-site/site.js ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js`。 這表示已編譯的CSS和JS正被複製到資料 `ui.apps` 夾中。

1. 查看檔案的修改時間戳 `aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css.txt`。 它應該會比模組中其他檔案更新得 `ui.apps` 多。

   與我們檢視的其他模組不同， **ui.frontend** 模組不會直接部署至AEM。 CSS和JS會複製到 **ui.apps模組中** ，然後 **** ui.apps模組會部署至AEM。 如果您從第一個Maven指令檢視建置順序，您會看到 **ui.frontend** 一律是在ui *.apps之* 前 **建置**。

   在教學課程的稍後部份，我們將檢視 **ui.frontend** 模組和內嵌webpack開發伺服器的進階功能，以進行快速開發。

## 包含核心元件 {#core-components}

原型會自動內嵌 [專案中的AEM核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html) 。 之前，在檢查部署的封裝至AEM時，會包含多個與核心元件相關的封裝。 核心元件是一組基本元件，旨在加速AEM Sites專案的開發。 核心元件是開放原始碼，可在 [GitHub上使用](https://github.com/adobe/aem-core-wcm-components)。 有關核心元件如何包含在專 [案中的詳細資訊，請參閱這裡](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html#core-components)。

1. 使用您最愛的文字編輯器開啟 `aem-guides-wknd/pom.xml`。

1. 搜尋 `core.wcm.components.version`. 這將顯示所包含的核心元件版本：

   ```xml
       <core.wcm.components.version>2.x.x</core.wcm.components.version>
   ```

   >[!NOTE]
   >
   > AEM Project Archetype將包含AEM Core Components版本，但這些專案有不同的發行週期，因此隨附的核心元件版本可能不是最新版本。 最佳實務是，您應始終希望運用最新版的核心元件。 新功能和錯誤修正會經常更新。 您可在 [GitHub上找到最新的發行資訊](https://github.com/adobe/aem-core-wcm-components/releases)。

1. 如果向下滾動到該部分， `dependencies` 您應看到各個核心元件從屬關係：

   ```xml
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.core</artifactId>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.content</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.config</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   <dependency>
       <groupId>com.adobe.cq</groupId>
       <artifactId>core.wcm.components.examples</artifactId>
       <type>zip</type>
       <version>${core.wcm.components.version}</version>
   </dependency>
   ```

## 原始碼控制管理 {#source-control}

使用某種形式的原始碼控制來管理應用程式中的程式碼，總是很好的主意。 本教學課程使用git和GitHub。 Maven和／或所選IDE生成的多個檔案應由SCM忽略。

每當您建立並安裝程式碼套件時，Maven會建立目標資料夾。 目標資料夾和內容應從SCM中排除。

在ui.apps下方，您也會注意到許多已建立的。content.xml檔案。 這些XML檔案映射JCR中安裝內容的節點類型和屬性。 這些檔案很重要， **不應** 忽略。

AEM專案原型將產生範例檔 `.gitignore` 案，以做為可安全忽略檔案的起點。 檔案會在中產生 `<src>/aem-guides-wknd/.gitignore`。

## 評論 {#chapter-review}

>[!VIDEO](https://video.tv.adobe.com/v/30153/?quality=12&learn=on)

## 恭喜！ {#congratulations}

恭喜您，您剛建立了第一個AEM專案！

### 後續步驟 {#next-steps}

透過元件基礎教學課程的簡單範例，瞭解Adobe Experience Manager(AEM)Sites元 `HelloWorld` 件的 [基礎技術](component-basics.md) 。
