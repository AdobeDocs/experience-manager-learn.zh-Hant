---
title: 開始使用AEM Sites — 專案設定
description: 建立Maven Multi Module專案以管理Experience Manager網站的程式碼和設定。
version: 6.5, Cloud Service
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
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1684'
ht-degree: 1%

---

# 專案設定 {#project-setup}

本教學課程涵蓋建立Maven Multi Module專案，以管理Adobe Experience Manager網站的程式碼和設定。

## 先決條件 {#prerequisites}

檢閱設定[本機開發環境](./overview.md#local-dev-environment)所需的工具和指示。 確保您有可在本機使用的新版Adobe Experience Manager執行個體，且未安裝其他範例/示範套件（必要Service Pack除外）。

## 目標 {#objective}

1. 瞭解如何使用Maven原型產生新的AEM專案。
1. 瞭解AEM專案原型產生的不同模組，以及它們如何共同運作。
1. 瞭解AEM核心元件如何包含在AEM專案中。

## 您即將建置的內容 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152?quality=12&learn=on)

在本章中，您使用[AEM專案原型](https://github.com/adobe/aem-project-archetype)產生新的Adobe Experience Manager專案。 您的AEM專案包含用於Sites實施的完整程式碼、內容和設定。 本章中產生的專案可作為WKND網站實施的基礎，並在未來的章節中建立。

**什麼是Maven專案？** - [Apache Maven](https://maven.apache.org/)是用於建置專案的軟體管理工具。 *所有Adobe Experience Manager*&#x200B;實作都使用Maven專案在AEM上建置、管理和部署自訂程式碼。

**什麼是Maven原型？** - [Maven原型](https://maven.apache.org/archetype/index.html)是用於產生新專案的範本或模式。 AEM專案原型有助於產生具有自訂名稱空間的新專案，並包括遵循最佳實務的專案結構，大幅加快專案開發。

## 建立專案 {#create}

建立適用於AEM的Maven多模組專案有幾個選項。 此教學課程使用[Maven AEM專案原型&#x200B;**35**](https://github.com/adobe/aem-project-archetype)。 Cloud Manager也[提供UI精靈](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/project-creation/using-the-wizard.html)，以啟動AEM應用程式專案的建立。 Cloud Manager UI產生的基礎專案與直接使用原型所產生的結構相同。

>[!NOTE]
>
>此教學課程使用原型的版本&#x200B;**35**。 使用原型的&#x200B;**最新**&#x200B;版本來產生新專案，永遠是最佳作法。

下一系列步驟將使用基於UNIX®的命令列終端機進行，但如果使用Windows終端機，則應該類似。

1. 開啟命令列終端機。 確認已安裝Maven：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 導覽至您要產生AEM專案的目錄。 這可以是任何您想要維護專案原始程式碼的目錄。 例如，在使用者主目錄下名為`code`的目錄：

   ```shell
   $ cd ~/code
   ```

1. 將下列內容貼到命令列，以[以批次模式](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html)產生專案：

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
   > 若要以AEM 6.5.14+為目標，請將`aemVersion="cloud"`取代為`aemVersion="6.5.14"`。
   >
   > 此外，請一律參照[AEM專案原型>使用方式](https://github.com/adobe/aem-project-archetype#usage)使用最新的`archetypeVersion`

   您可以在此處](https://github.com/adobe/aem-project-archetype#available-properties)找到設定專案[的可用屬性完整清單。

1. 下列資料夾和檔案結構是由本機檔案系統上的Maven原型所產生：

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

## 部署和建置專案 {#build}

建立專案程式碼並將其部署到AEM的本機執行個體。

1. 請確定您有AEM的作者執行個體在連線埠&#x200B;**4502**&#x200B;上本機執行。
1. 從命令列，瀏覽至`aem-guides-wknd`專案目錄。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 執行以下命令，建置整個專案並將其部署到AEM：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   建置需要約一分鐘的時間，並且應該以下列訊息結束：

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

   Maven設定檔`autoInstallSinglePackage`會編譯專案的個別模組，並將單一套件部署至AEM執行個體。 依預設，此封裝會部署至在本機執行於連線埠&#x200B;**4502**&#x200B;上且認證為`admin:admin`的AEM執行個體。

1. 導覽至本機AEM執行個體上的封裝管理員： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您應該會看到`aem-guides-wknd.ui.apps`、`aem-guides-wknd.ui.config`、`aem-guides-wknd.ui.content`和`aem-guides-wknd.all`的封裝。

1. 瀏覽至Sites主控台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content)。 WKND網站是其中一個網站。 其中包含具有美國和語言主版階層的網站結構。 此網站階層是以使用原型產生專案時`language_country`和`isSingleCountryWebsite`的值為基礎。

1. 選取頁面，然後按一下功能表列中的&#x200B;**編輯**&#x200B;按鈕，開啟&#x200B;**美國** `>` **英文**&#x200B;頁面：

   ![網站主控台](assets/project-setup/aem-sites-console.png)

1. 已建立入門內容，且有數個元件可新增至頁面。 嘗試使用這些元件以瞭解功能。 您將在下一章中學習元件的基本知識。

   ![家用入門內容](assets/project-setup/start-home-page.png)

   *由Archetype*&#x200B;產生的範例內容

## Inspect專案 {#project-structure}

產生的AEM專案由個別Maven模組組成，每個模組都有不同的角色。 本教學課程和大部分的開發工作聚焦於這些模組：

* [core](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java程式碼，主要是後端開發人員。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) — 包含CSS、JavaScript、Sass、TypeScript的原始程式碼，主要用於前端開發人員。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html) — 包含元件和對話方塊定義，將編譯的CSS和JavaScript嵌入為使用者端資料庫。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html) — 包含結構化內容與設定，例如可編輯的範本、中繼資料結構描述(/content、/conf)。

* **所有** — 這是空的Maven模組，它會將上述模組結合為單一套件，可部署至AEM環境。

![Maven專案圖表](assets/project-setup/project-pom-structure.png)

請參閱[AEM專案原型檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)，以瞭解&#x200B;**所有** Maven模組的更多詳細資料。

### 納入核心元件 {#core-components}

[AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)是一組適用於AEM的標準化網頁內容管理(WCM)元件。 這些元件提供一組基準功能，並針對個別專案進行樣式、自訂和延伸。

AEM as a Cloud Service環境包含最新版本的[AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)。 因此，針對AEM as a Cloud Service產生的專案&#x200B;**不**&#x200B;包含AEM核心元件的內嵌。

對於AEM 6.5/6.4產生的專案，原型會自動將[AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)內嵌在專案中。 AEM 6.5/6.4最佳實務是內嵌AEM核心元件，以確保最新版本可隨專案一起部署。 有關專案中如何[包含核心元件的詳細資訊，請參閱](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components)。

## Source控制管理 {#source-control}

最好是使用某種形式的原始檔控制來管理應用程式中的程式碼。 本教學課程使用Git和GitHub。 Maven和/或所選IDE會產生數個檔案，SCM應忽略這些檔案。

當您建置和安裝程式碼套件時，Maven會建立目標資料夾。 目標資料夾和內容應從SCM排除。

在底下，`ui.apps`模組觀察到已建立許多`.content.xml`檔案。 這些XML檔案會對應安裝在JCR中的節點型別和內容屬性。 這些檔案非常重要，因此&#x200B;**無法**&#x200B;被忽略。

AEM專案原型會產生範例`.gitignore`檔案，可作為可以安全忽略檔案的起點。 檔案產生於`<src>/aem-guides-wknd/.gitignore`。

## 恭喜！ {#congratulations}

恭喜，您已建立您的第一個AEM專案！

### 後續步驟 {#next-steps}

透過包含[元件基本知識](component-basics.md)教學課程的簡單`HelloWorld`範例，瞭解Adobe Experience Manager (AEM) Sites元件的基礎技術。

## 進階Maven命令（附加） {#advanced-maven-commands}

在開發期間，您可能只使用其中一個模組，並且想要避免建置整個專案以節省時間。 您也可以直接部署至AEM Publish執行個體，或部署至未在連線埠4502上執行的AEM執行個體。

接下來，讓我們檢閱一些其他Maven設定檔和命令，您可以在開發期間使用這些設定檔和命令，以獲得更大的彈性。

### 核心模組 {#core-module}

**[核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)**&#x200B;模組包含與專案相關聯的所有Java™程式碼。 **核心**&#x200B;模組的組建會將OSGi套件組合部署至AEM。 若要僅建置此模組：

1. 導覽至`core`資料夾（`aem-guides-wknd`下方）：

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

1. 導覽至[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)。 這是OSGi Web主控台，包含有關安裝在AEM執行個體上的所有套件組合的資訊。

1. 切換&#x200B;**Id**&#x200B;排序欄，您應該會看到已安裝且作用中的WKND組合。

   ![核心組合](assets/project-setup/wknd-osgi-console.png)

1. 您可以在[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar)中看到jar的「實體」位置：

   ![CRXDE Jar位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模組 {#apps-content-module}

**[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven模組包含`/apps`下方的網站所需的所有轉譯程式碼。 這包含以名為[clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)的AEM格式儲存的CSS/JS。 這也包括用於呈現動態HTML的[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/overview.html)指令碼。 您可以將&#x200B;**ui.apps**&#x200B;模組想成是JCR中結構的對應，但格式可以儲存在檔案系統上，並認可至原始檔控制。 **ui.apps**&#x200B;模組僅包含程式碼。

若要僅建置此模組：

1. 從命令列。 導覽至`ui.apps`資料夾（`aem-guides-wknd`下方）：

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

1. 導覽至[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)。 您應該會看到`ui.apps`套件是第一個安裝的套件，而且其時間戳記應比任何其他套件都新。

   已安裝![Ui.apps套件](assets/project-setup/ui-apps-package.png)

1. 返回命令列，然後執行下列命令（在`ui.apps`資料夾內）：

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

   設定檔`autoInstallPackagePublish`打算將封裝部署至在連線埠&#x200B;**4503**&#x200B;上執行的Publish環境。 如果找不到在http://localhost:4503上執行的AEM執行個體，則會發生上述錯誤。

1. 最後執行下列命令，將`ui.apps`封裝部署在連線埠&#x200B;**4504**&#x200B;上：

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

   若沒有在連線埠&#x200B;**4504**&#x200B;上執行的AEM執行個體可用，則同樣會發生建置失敗。 引數`aem.port`定義於`aem-guides-wknd/pom.xml`的POM檔案中。

**[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)**&#x200B;模組的結構與&#x200B;**ui.apps**&#x200B;模組相同。 唯一的差異是&#x200B;**ui.content**&#x200B;模組包含所謂的&#x200B;**可變**&#x200B;內容。 **可變**&#x200B;內容基本上是指非程式碼設定，例如儲存在原始檔控制&#x200B;**中的範本、原則或資料夾結構，但**&#x200B;可以直接在AEM執行個體上修改。 在頁面和範本一章中會更詳細地探討。

用來建置&#x200B;**ui.apps**&#x200B;模組的相同Maven命令可以用來建置&#x200B;**ui.content**&#x200B;模組。 您可以從&#x200B;**ui.content**&#x200B;資料夾中重複上述步驟。

## 疑難排解

如果使用AEM專案原型產生專案時發生問題，請參閱[已知問題](https://github.com/adobe/aem-project-archetype#known-issues)的清單和未完成[問題](https://github.com/adobe/aem-project-archetype/issues)的清單。

## 再次恭喜！ {#congratulations-bonus}

恭喜您繼續閱讀獎金材料。

### 後續步驟 {#next-steps-bonus}

透過包含[元件基本知識](component-basics.md)教學課程的簡單`HelloWorld`範例，瞭解Adobe Experience Manager (AEM) Sites元件的基礎技術。
