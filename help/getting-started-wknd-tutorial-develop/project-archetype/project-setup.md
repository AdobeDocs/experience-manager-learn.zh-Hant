---
title: 開始使用 AEM Sites - 專案設定
description: 建立一個 Maven 多模組專案來管理 Experience Manager Site 的程式碼和設定。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-3418
thumbnail: 30152.jpg
doc-type: Tutorial
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
recommendations: noDisplay, noCatalog
duration: 502
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1695'
ht-degree: 100%

---

# 專案設定 {#project-setup}

本教學課程說明如何建立 Maven 多模組專案來管理 Adobe Experience Manager Site 的程式碼和設定。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](./overview.md#local-dev-environment)所需的工具與指示。確認您在本機擁有 Adobe Experience Manager 的全新實例，並且沒有安裝其他範例/示範封裝 (必要的 Service Pack 除外)。

## 目標 {#objective}

1. 了解如何使用 Maven 原型產生新的 AEM 專案。
1. 了解 AEM 專案原型產生的不同模組以及其如何搭配運作。
1. 了解如何將 AEM 核心元件包含在 AEM 專案中。

## 您將要建置的內容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

在本章中，您將使用 [AEM 專案原型](https://github.com/adobe/aem-project-archetype)產生一個新的 Adobe Experience Manager 專案。您的 AEM 專案包含可供 Sites 實施使用的完整程式碼、內容和設定。在本章中產生的專案將用作實施 WKND 網站的基礎，並將在未來章節中繼續建置。

**Maven 專案是什麼？** - [Apache Maven](https://maven.apache.org/) 是用於建置專案的軟體管理工具。*所有 Adobe Experience Manager* 實施均使用 Maven 專案在 AEM 之上建置、管理和部署自訂程式碼。

**Maven 原型是什麼？** -  [Maven 原型](https://maven.apache.org/archetype/index.html)是用來產生新專案的範本或模式。AEM 專案原型可協助建立具有自訂命名空間的新專案，而且所包含的專案結構符合最佳做法，從而大幅加快專案開發。

## 建立專案 {#create}

要建立 AEM 的 Maven 多模組專案，有多個選項可供選擇。本教學課程使用 [Maven AEM 專案原型 **35**](https://github.com/adobe/aem-project-archetype)。Cloud Manager 也[提供 UI 精靈](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html?lang=zh-Hant)以便開始建立 AEM 應用程式專案。由 Cloud Manager UI 產生的基礎專案與直接使用原型建立的專案擁有相同的結構。

>[!NOTE]
>
>本教學課程使用原型的 **35** 版本。使用&#x200B;**最新**&#x200B;版的原型來產生新專案，永遠是最佳做法。

接下來將使用 Unix® 型命令列終端機進行一系列步驟，但如果使用 Windows 終端機執行，應該是相似的情形。

1. 開啟命令列終端機。驗證 Maven 已經安裝：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 導覽到您想要產生 AEM 專案的目錄。這可能是您想要維持其中專案原始碼的任何目錄。例如，使用者主目錄下一個名為「`code`」的目錄：

   ```shell
   $ cd ~/code
   ```

1. 將以下內容貼到命令列，以便[使用批次模式產生專案](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)：

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
   > 若專案的目標是 AEM 6.5.14+，請將 `aemVersion="cloud"` 取代為 `aemVersion="6.5.14"`。
   >
   > 此外，請務必使用最新的 `archetypeVersion`，做法是參照至 [AEM 專案原型 > 使用情況](https://github.com/adobe/aem-project-archetype#usage)

   設定專案的可用屬性完整清單請[參閱這裡](https://github.com/adobe/aem-project-archetype#available-properties)。

1. Maven 原型在您的本機檔案系統上產生以下資料夾和檔案結構：

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

## 部署與建置專案 {#build}

建置專案程式碼並部署到 AEM 本機實例。

1. 請確認在本機的連接埠 **4502** 上正在執行 AEM 的作者實例。
1. 從命令列導覽到 `aem-guides-wknd` 專案目錄。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 執行以下命令來建置整個專案並部署到 AEM：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   建置費時大約一分鐘，並應在結束時顯示以下訊息：

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

   Maven 設定檔 `autoInstallSinglePackage` 編譯專案的個別模組並將單一封裝部署到 AEM 實例。依照預設，此封裝會部署到在本機的連接埠 **4502** 上執行的 AEM 實例，並使用 `admin:admin` 認證。

1. 導覽到本機 AEM 實例上的封裝管理員：[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。您應該會看到 `aem-guides-wknd.ui.apps`、`aem-guides-wknd.ui.config`、`aem-guides-wknd.ui.content` 和 `aem-guides-wknd.all` 的封裝。

1. 導覽至 Sites 控制台：[http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。WKND 網站就是其中一個網站。其包含一個具有「US」和「Language Masters」階層的網站結構。此網站階層是基於使用原型產生專案時 `language_country` 和 `isSingleCountryWebsite` 的值。

1. 選取頁面並按一下選單列中的「**編輯**」按鈕，開啟「**US** `>` **EN**」頁面：

   ![網站主控台](assets/project-setup/aem-sites-console.png)

1. 已經建立入門內容，並提供數個可供新增至頁面的元件。使用這些元件進行實驗，以便對其功能有所了解。在下一章，您將了解元件的基礎知識。

   ![首頁入門內容](assets/project-setup/start-home-page.png)

   *由原型產生的範例內容*

## 檢查專案 {#project-structure}

所產生的 AEM 專案由個別的 Maven 模組組成，且每個模組都有不同角色。本教學課程和大多數開發主要使用以下模組：

* [核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=zh-Hant) - Java 程式碼，主要適用於後端開發人員。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hant) - 包含 CSS、JavaScript、Sass、TypeScript 的原始碼，主要適用於前端開發人員。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=zh-Hant)  - 包含元件和對話框定義，嵌入已編譯的 CSS 和 JavaScript 作為用戶端程式庫。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=zh-Hant) - 包含結構化內容和設定，例如可編輯的範本、後設資料結構描述 (/content、/conf)。

* **所有** - 這是一個空的 Maven 模組，將上述模組結合成一個可以部署到 AEM 環境的封裝。

![Maven 專案圖表](assets/project-setup/project-pom-structure.png)

請參閱 [AEM 專案原型文件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)以了解更多關於&#x200B;**所有** Maven 模組的詳細資訊。

### 包含核心元件 {#core-components}

[AEM 核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)是一組適用於 AEM 的標準化網站內容管理 (WCM) 元件。這些元件提供一套基礎功能，並針對個別專案進行樣式設定、自訂與擴充。

AEM as a Cloud Service 環境包含最新版的 [AEM 核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)。因此，為 AEM as a Cloud Service 產生的專案&#x200B;**不會**&#x200B;包含 AEM 核心元件的嵌入。

針對 AEM 6.5/6.4 產生的專案，原型會自動在專案中嵌入 [AEM 核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)。對於 AEM 6.5/6.4 來說，最佳做法是嵌入 AEM 核心元件，以確保最新版本隨您的專案一起部署。如需如何將核心元件[包含在專案中的更多資訊，請參閱這裡](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html?lang=zh-Hant#core-components)。

## 原始碼控制管理 {#source-control}

使用某種形式的原始碼控制來管理應用程式中的程式碼，一直是非常好的做法。本教學課程使用 git 和 GitHub。SCM 應該忽略由 Maven 和/或所選 IDE 產生的數個檔案。

每當您建置和安裝程式碼封裝時，Maven 都會建立一個目標資料夾。SCM 不應該包含目標資料夾和內容。

您可以看到在 `ui.apps` 模組之下已建立許多 `.content.xml` 檔案。這些 XML 檔案對應到安裝在 JCR 中的節點類型和內容屬性。這些檔案皆非常重要，**不能**&#x200B;被忽略。

AEM 專案原型會產生範例 `.gitignore` 檔案，可用作可安全忽略之檔案的起點。該檔案會產生在`<src>/aem-guides-wknd/.gitignore` 之下。

## 恭喜！ {#congratulations}

恭喜，您已經建立第一個 AEM 專案！

### 後續步驟 {#next-steps}

透過簡單的 `HelloWorld` 範例搭配[元件基礎知識](component-basics.md)教學課程，了解 Adobe Experience Manager (AEM) Sites 元件的基礎技術。

## 進階 Maven 命令 (額外內容) {#advanced-maven-commands}

在開發過程中，您可能只會使用其中一個模組，而且為了節省時間，想要避免建置整個專案。您也可能想要直接部署到 AEM Publish 實例，或者部署到不是在連接埠 4502 上執行的 AEM 實例。

我們來回顧更多您在開發過程中可以使用的 Maven 設定檔和命令，以便享受更大的彈性。

### 核心模組 {#core-module}

**[核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html?lang=zh-Hant)**&#x200B;模組含有與專案相關的所有 Java™ 程式碼。建置&#x200B;**核心**&#x200B;模組會將一個 OSGi 套件部署到 AEM。僅建置此模組：

1. 導覽至 `core` 資料夾 (在 `aem-guides-wknd` 下方)：

   ```shell
   $ cd core/
   ```

1. 執行以下命令：

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

1. 導覽至 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)。這是 OSGi 網頁控制台，其包含安裝在 AEM 實例上所有套件的相關資訊。

1. 切換 **ID** 排序欄，您應該會看到 WKND 套件已安裝而且是使用中。

   ![核心套件](assets/project-setup/wknd-osgi-console.png)

1. 您可以在 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar) 中看到 jar 的「實體」位置：

   ![CRXDE Jar 的位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps 和 Ui.content 模組 {#apps-content-module}

**[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html?lang=zh-Hant)** maven 模組包含 `/apps` 路徑之下網站所需要的全部轉譯程式碼。其中包括以 AEM 格式儲存的 CSS/JS，稱為 [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hant)。還包括用來轉譯動態 HTML 的 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html?lang=zh-Hant) 指令碼。您可以把 **ui.apps** 模組視為 JCR 結構的對應，但此模組採用可以儲存在檔案系統中並提交給原始碼控制的格式。**ui.apps** 模組僅包含程式碼。

僅建置此模組：

1. 從命令列開始。導覽至 `ui.apps` 資料夾 (在 `aem-guides-wknd` 下方)：

   ```shell
   $ cd ../ui.apps
   ```

1. 執行以下命令：

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

1. 導覽至 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。您應該會看到第一個安裝的封裝就是 `ui.apps` 封裝，而且其時間戳記比任何其他封裝更接近現在。

   ![已安裝 Ui.apps 封裝](assets/project-setup/ui-apps-package.png)

1. 返回命令列並執行以下命令 (在 `ui.apps` 資料夾中)：

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

   此設定檔 `autoInstallPackagePublish` 想要把封裝部署到在連接埠 **4503** 上執行的 Publish 環境。如果找不到在 http://localhost:4503 上執行的 AEM 實例，便會出現上述錯誤。

1. 最後，執行以下命令將 `ui.apps` 封裝部署到連接埠 **4504**：

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

   同樣地，如果在連接埠 **4504** 上執行的 AEM 實例皆不可用，便會建置失敗。位在 `aem-guides-wknd/pom.xml` 的 POM 檔案會定義參數 `aem.port`。

**[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html?lang=zh-Hant)** 模組的結構與 **ui.apps** 模組相同。唯一的差異是 **ui.content** 模組包含所謂的&#x200B;**可變**&#x200B;內容。**可變**&#x200B;內容基本上是指非程式碼設定，例如範本、原則或資料夾結構，這些設定儲存在原始碼控制&#x200B;**但是**&#x200B;可以在 AEM 實例上直接修改。在「頁面和範本」一章中會詳細討論其中細節。

用於建置 **ui.apps** 模組的同一組 Maven 指令可以用來建置 **ui.content** 模組。您可以在 **ui.content** 資料夾中隨意重複上述步驟。

## 疑難排解

如果使用 AEM 專案原型產生專案時發生問題，請參閱[已知問題](https://github.com/adobe/aem-project-archetype#known-issues)清單和未解決[問題](https://github.com/adobe/aem-project-archetype/issues)清單。

## 再次恭喜！ {#congratulations-bonus}

恭喜您讀完了額外的內容。

### 後續步驟 {#next-steps-bonus}

透過簡單的 `HelloWorld` 範例搭配[元件基礎知識](component-basics.md)教學課程，了解 Adobe Experience Manager (AEM) Sites 元件的基礎技術。
