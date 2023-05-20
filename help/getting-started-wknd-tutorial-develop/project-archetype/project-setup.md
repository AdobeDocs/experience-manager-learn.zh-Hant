---
title: 開始使用AEM Sites — 項目設定
description: 建立Maven多模組項目以管理Experience Manager站點的代碼和配置。
version: 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '1839'
ht-degree: 3%

---

# 專案設定 {#project-setup}

本教程介紹如何建立Maven多模組項目，以管理Adobe Experience Manager站點的代碼和配置。

## 必備條件 {#prerequisites}

查看所需的工具和設定 [地方開發環境](./overview.md#local-dev-environment)。 確保您在本地提供了Adobe Experience Manager的新實例，並且沒有安裝其他示例/演示包（所需的Service Pack除外）。

## 目標 {#objective}

1. 瞭解如何使用Maven原型AEM生成新項目。
1. 瞭解項目原型生成的AEM不同模組以及它們如何協同工作。
1. 瞭解AEM核心元件如何包含在項AEM目中。

## 您要構建的 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

在本章中，您使用 [項AEM目原型](https://github.com/adobe/aem-project-archetype)。 您的AEM項目包含用於站點實施的完整代碼、內容和配置。 本章中生成的項目是實施WKND網站的基礎，並在今後各章中加以建立。

**什麼是Maven項目？** - [阿帕奇·馬文](https://maven.apache.org/) 是用於構建項目的軟體管理工具。 *全Adobe Experience Manager* 實現使用Maven項目在上面構建、管理和部署自定義代AEM碼。

**什麼是馬文原型？** - A [馬文原型](https://maven.apache.org/archetype/index.html) 是用於生成新項目的模板或模式。 項目AEM原型有助於生成具有自定義命名空間的新項目，並包括遵循最佳實踐的項目結構，從而大大加快項目開發。

## 建立項目 {#create}

為建立Maven多模組項目有幾個選AEM項。 本教程使用 [馬文AEM計畫原型 **35**](https://github.com/adobe/aem-project-archetype)。 雲管理器 [提供UI嚮導](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html) 啟動應用程式項AEM目的建立。 由Cloud Manager UI生成的基礎項目與直接使用原型的結構相同。

>[!NOTE]
>
>本教程使用版本 **35** 原型。 使用 **最新** 原型版本以生成新項目。

接下來的一系列步驟將使用基於UNIX®的命令行終端進行，但如果使用Windows終端，則應類似。

1. 開啟命令行終端。 驗證是否安裝了Maven:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 導航到要在其中生成項目的AEM目錄。 這可以是要維護項目原始碼的任何目錄。 例如，名為 `code` 在用戶的主目錄下：

   ```shell
   $ cd ~/code
   ```

1. 將以下內容貼上到命令行中以 [以批處理模式生成項目](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=39 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides" \
       -D artifactId="aem-guides-wknd" \
       -D package="com.adobe.aem.guides.wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 目標AEM6.5.14+替換 `aemVersion="cloud"` 與 `aemVersion="6.5.14"`。
   >
   > 另外，始終使用 `archetypeVersion` 指 [項AEM目原型>使用](https://github.com/adobe/aem-project-archetype#usage)

   用於配置項目的可用屬性的完整清單 [可在此處找到](https://github.com/adobe/aem-project-archetype#available-properties)。

1. 以下資料夾和檔案結構由本地檔案系統上的Maven原型生成：

   ```plain
    ~/code/
       |--- aem-guides-wknd/
           |--- all/
           |--- core/
           |--- ui.apps/
           |--- ui.apps.structure/
           |--- ui.config/
           |--- ui.content/
           |--- ui.frontend/
           |--- ui.tests /
           |--- it.tests/
           |--- dispatcher/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 部署和生成項目 {#build}

生成項目代碼並將其部署到的本地實例AEM。

1. 確保有一個作者實例AEM在埠上本地運行 **4502**。
1. 從命令行導航到 `aem-guides-wknd` 項目目錄。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 運行以下命令以構建和部署整個項目AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   生成大約需要一分鐘時間，應以以下消息結束：

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for WKND Sites Project 0.0.1-SNAPSHOT:
   [INFO] 
   [INFO] WKND Sites Project ................................. SUCCESS [  0.113 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  3.136 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [  4.461 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.359 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  1.732 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  0.956 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.064 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  8.229 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [  3.329 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.027 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.032 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  23.189 s
   [INFO] Finished at: 2023-01-10T11:12:23-05:00
   [INFO] ------------------------------------------------------------------------    
   ```

   馬文檔案 `autoInstallSinglePackage` 編譯項目的各個模組，並將單個包部署到實AEM例。 預設情況下，此包部署到AEM埠上本地運行的實例 **4502** 還有 `admin:admin`。

1. 導航到本地實例上的包管AEM理器： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您應看到的包 `aem-guides-wknd.ui.apps`。 `aem-guides-wknd.ui.config`。 `aem-guides-wknd.ui.content`, `aem-guides-wknd.all`。

1. 導航到「站點」控制台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。 WKND站點是其中一個站點。 它包括一個包含美國和語言母版層次結構的網站結構。 此站點層次結構基於的 `language_country` 和 `isSingleCountryWebsite` 使用原型生成項目時。

1. 開啟 **美國** `>` **英語** 頁面 **編輯** 按鈕：

   ![站點控制台](assets/project-setup/aem-sites-console.png)

1. 已建立啟動程式內容，並且有幾個元件可添加到頁面。 對這些元件進行實驗，以瞭解其功能。 您將在下一章中學習元件的基本知識。

   ![首頁入門級內容](assets/project-setup/start-home-page.png)

   *原型生成的樣本內容*

## Inspect項目 {#project-structure}

生成的AEM項目由各個Maven模組組成，每個模組具有不同的角色。 本教程和大部分開發都側重於以下模組：

* [核](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java代碼，主要是後端開發人員。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)  — 包含CSS、JavaScript、Sass、TypeScript的原始碼，主要用於前端開發人員。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)  — 包含元件和對話框定義，將編譯的CSS和JavaScript嵌入為客戶端庫。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 包含結構內容和配置，如可編輯模板、元資料架構(/content、/conf)。

* **全部**  — 這是一個空的Maven模組，它將上述模組合併到一個可以部署到環境的AEM包中。

![Maven項目圖](assets/project-setup/project-pom-structure.png)

查看 [項AEM目原型文檔](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant) 瞭解更多詳細資訊 **全部** 馬文模組。

### 包含核心元件 {#core-components}

[核AEM心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 是一組標準化的Web內容管理(WCM)組AEM件。 這些元件提供了功能的基線集，並為各個項目設定樣式、自定義和擴展。

AEMas a Cloud Service環境包括 [核AEM心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。 因此為as a Cloud Service生成AEM的項目 **不** 包括嵌入的AEM核心元件。

對於AEM6.5/6.4生成的項目，原型自動嵌入 [核AEM心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 的下界。 嵌入核心元件AEM是6.5/6.4的最AEM佳做法，可確保將最新版本與您的項目一起部署。 有關核心元件的詳細資訊 [包含在項目中，可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components)。

## 原始碼管理 {#source-control}

使用某種形式的原始碼管理來管理應用程式中的代碼始終是一個好主意。 本教程使用git和GitHub。 由Maven和/或所選IDE生成的檔案有幾個應被SCM忽略。

Maven在生成和安裝代碼包時建立目標資料夾。 目標資料夾和內容應從SCM中排除。

在， `ui.apps` 模組觀察到 `.content.xml` 檔案。 這些XML檔案映射JCR中安裝的內容的節點類型和屬性。 這些檔案非常重要， **不能** 被忽略。

項目AEM原型生成示例 `.gitignore` 可用作檔案可安全忽略的起始點的檔案。 檔案生成於 `<src>/aem-guides-wknd/.gitignore`。

## 恭喜！ {#congratulations}

恭喜，您建立了第一個AEM項目！

### 後續步驟 {#next-steps}

通過簡單易懂的功能，瞭解Adobe Experience Manager(AEM)站點元件的底層技術 `HelloWorld` 示例 [元件基礎](component-basics.md) 教程。

## 高級Maven命令（附加） {#advanced-maven-commands}

在開發過程中，您可能只使用其中一個模組，並希望避免構建整個項目以節省時間。 您可能還希望直接部署到AEM發佈實例，或者可能部署到AEM埠4502上未運行的實例。

接下來，我們來查看一些額外的Maven配置檔案和命令，這些配置檔案和命令可在開發過程中獲得更大的靈活性。

### 核心模組 {#core-module}

的 **[核](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** 模組包含與項目關聯的所有Java™代碼。 構建 **核** 模組將OSGi捆綁部署到AEM。 要僅構建此模組，請執行以下操作：

1. 導航到 `core` 資料夾 `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. 運行以下命令：

   ```shell
   $ mvn clean install -PautoInstallBundle
   ...
   [INFO] --- sling-maven-plugin:2.4.0:install (install-bundle) @ aem-guides-wknd.core ---
   [INFO] Installing Bundle aem-guides-wknd.core(~/code/aem-guides-wknd/core/target/aem-guides-wknd.core-0.0.1-SNAPSHOT.jar) to http://localhost:4502/system/console via WebConsole
   [INFO] Bundle installed
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  8.558 s
   ```

1. 導航到 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)。 這是OSGi Web控制台，包含有關實例上安裝的所有捆綁包AEM的資訊。

1. 切換 **ID** 排序列，您應看到WKND捆綁包已安裝並處於活動狀態。

   ![核心束](assets/project-setup/wknd-osgi-console.png)

1. 您可以在中查看罐的「物理」位置 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![CRXDE的Jar位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模組 {#apps-content-module}

的 **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven模組包含下面站點所需的所有呈現代碼 `/apps`。 這包括以名為「CSS/JS」的格AEM式儲存的CSS/JS [客戶端](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)。 這還包括 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html) 用於呈現動態HTML的指令碼。 你可以想到 **ui.apps** 模組作為指向JCR中結構的映射，但格式可以儲存在檔案系統上並提交到原始碼控制。 的 **ui.apps** 模組僅包含代碼。

要僅構建此模組，請執行以下操作：

1. 從命令行。 導航到 `ui.apps` 資料夾 `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. 運行以下命令：

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 70ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.987 s
   [INFO] Finished at: 2023-01-10T11:35:28-05:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 導航到 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您應該看到 `ui.apps` 軟體包作為第一個已安裝的軟體包，它的時間戳應比其他任何軟體包都更新。

   ![已安裝Ui.apps包](assets/project-setup/ui-apps-package.png)

1. 返回到命令行並運行以下命令(在 `ui.apps` 資料夾):

   ```shell
   $ mvn -PautoInstallPackagePublish clean install
   ...
   [INFO] --- content-package-maven-plugin:1.0.2:install (install-package-publish) @ aem-guides-wknd.ui.apps ---
   [INFO] Installing aem-guides-wknd.ui.apps (/Users/sachinmali/Desktop/code/wknd-tutorial/aem-guides-wknd/ui.apps/target/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip) to http://localhost:4503/crx/packmgr/service.jsp
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] I/O exception (java.net.ConnectException) caught when processing request: Connection refused (Connection refused)
   [INFO] Retrying request
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  2.812 s
   [INFO] Finished at: 2023-01-10T11:37:28-05:00
   [INFO] ------------------------------------------------------------------------
   [ERROR] Failed to execute goal com.day.jcr.vault:content-package-maven-plugin:1.0.2:install (install-package-publish) on project aem-guides-wknd.ui.apps: Connection refused (Connection refused) -> [Help 1]
   ```

   配置檔案 `autoInstallPackagePublish` 旨在將包部署到埠上運行的發佈環境 **4503**。 如果找不到運行在http://localhost:4503上AEM的實例，則會出現上述錯誤。

1. 最後運行以下命令以部署 `ui.apps` 埠上的包 **4504**:

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

   如果埠上沒有運行實例，則預計會AEM發生生成失敗 **4504** 的子菜單。 參數 `aem.port` 在POM檔案中定義 `aem-guides-wknd/pom.xml`。

的 **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** 模組的結構與 **ui.apps** 中。 唯一的區別是 **ui.content** 模組包含所謂的 **可變** 內容。 **可變** 內容實質上是指儲存在原始碼管理中的非代碼配置，如模板、策略或資料夾結構 **但** 可以直接在實例AEM上修改。 在「頁面和模板」一章中詳細探討了這一點。

用於生成 **ui.apps** 模組可用於構建 **ui.content** 中。 您可以在 **ui.content** 的子菜單。

## 疑難排解

如果使用「項目原型」生成項AEM目存在問題，請參閱 [已知問題](https://github.com/adobe/aem-project-archetype#known-issues) 和開啟清單 [問題](https://github.com/adobe/aem-project-archetype/issues)。

## 再次祝賀！ {#congratulations-bonus}

恭喜你，翻閱獎金材料。

### 後續步驟 {#next-steps-bonus}

通過簡單易懂的功能，瞭解Adobe Experience Manager(AEM)站點元件的底層技術 `HelloWorld` 示例 [元件基礎](component-basics.md) 教程。
