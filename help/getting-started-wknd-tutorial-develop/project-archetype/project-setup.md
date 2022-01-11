---
title: AEM Sites快速入門 — 專案設定
seo-title: Getting Started with AEM Sites - Project Setup
description: 涵蓋建立Maven多模組專案，以管理AEM網站的程式碼和設定。
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 3418
thumbnail: 30152.jpg
exl-id: bb0cae58-79bd-427f-9116-d46afabdca59
source-git-commit: a366d485da3f473bd4c1ef31538231965acc825c
workflow-type: tm+mt
source-wordcount: '1843'
ht-degree: 2%

---

# 專案設定 {#project-setup}

本教學課程涵蓋建立Maven多模組專案，以管理Adobe Experience Manager網站的程式碼和設定。

## 必備條件 {#prerequisites}

檢閱設定 [本地開發環境](./overview.md#local-dev-environment). 請確定您已在本機取得最新的Adobe Experience Manager例項，且未安裝其他範例/示範套件（必要的Service Pack除外）。

## 目標 {#objective}

1. 了解如何使用Maven原型產生新的AEM專案。
1. 了解AEM專案原型產生的不同模組，以及它們如何共同運作。
1. 了解AEM核心元件如何納入AEM專案。

## 您將建置的 {#what-build}

>[!VIDEO](https://video.tv.adobe.com/v/30152/?quality=12&learn=on)

在本章中，您將使用 [AEM專案原型](https://github.com/adobe/aem-project-archetype). 您的AEM專案包含用於Sites實作的所有程式碼、內容和設定。 本章生成的項目將作為WKND站點實施的基礎，並將在以後的章節中構建。

**什麼是Maven專案？** - [阿帕奇·馬文](https://maven.apache.org/) 是用於建立項目的軟體管理工具。 *全部Adobe Experience Manager* 實作會使用Maven專案來建置、管理和部署自訂程式碼，並在AEM上完成。

**什麼是Maven原型？** - A [馬文原型](https://maven.apache.org/archetype/index.html) 是產生新專案的範本或模式。 AEM專案原型可讓我們使用自訂命名空間產生新專案，並包含遵循最佳實務的專案結構，大幅加速我們的專案。

## 建立專案 {#create}

為AEM建立Maven多模組專案有幾個選項。 本教學課程將運用 [Maven AEM專案原型 **26**](https://github.com/adobe/aem-project-archetype). Cloud Manager也 [提供UI精靈](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/getting-started/create-application-project/using-the-wizard.html) 以啟動建立AEM應用程式專案。 Cloud Manager UI產生的基礎專案結構與直接使用原型相同。

>[!NOTE]
>
>本教學課程使用版本 **26** 原型。 使用 **最新** 原型版本，以產生新專案。

下一系列步驟將使用基於UNIX的命令行終端進行，但如果使用Windows終端，則應類似。

1. 開啟命令行終端。 確認已安裝Maven:

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

1. 確認 **adobe-public** 配置檔案通過運行以下命令處於活動狀態：

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

   若您這麼做 **not** 請參閱 **adobe-public** 表示Adobe存放庫未在您的 `~/.m2/settings.xml` 檔案。 請重新造訪安裝和設定Apache Maven的步驟，位於 [本地開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html#install-apache-maven).

1. 導覽至您要產生AEM專案的目錄。 這可以是您要維護項目原始碼的任何目錄。 例如，名為的目錄 `code` 在用戶的首頁目錄下：

   ```shell
   $ cd ~/code
   ```

1. 將下列內容貼入命令列以 [以批模式生成項目](https://maven.apache.org/archetype/maven-archetype-plugin/examples/generate-batch.html):

   ```shell
   mvn -B archetype:generate \
       -D archetypeGroupId=com.adobe.aem \
       -D archetypeArtifactId=aem-project-archetype \
       -D archetypeVersion=26 \
       -D appTitle="WKND Sites Project" \
       -D appId="wknd" \
       -D groupId="com.adobe.aem.guides.wknd" \
       -D artifactId="aem-guides-wknd" \
       -D version="0.0.1-SNAPSHOT" \
       -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 如果以AEM 6.5.5+為目標取代 `aemVersion="cloud"` with `aemVersion="6.5.5"`. 如果目標為6.4.8+，請使用 `aemVersion="6.4.8"`.

   可用屬性的完整清單，用於配置項目 [可在此處找到](https://github.com/adobe/aem-project-archetype#available-properties).

1. 您的本機檔案系統上的Maven原型將產生下列資料夾和檔案結構：

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
           |--- analyse/
           |--- pom.xml
           |--- README.md
           |--- .gitignore
   ```

## 部署並建置專案 {#build}

建立專案程式碼並部署至AEM的本機執行個體。

1. 請確定您的AEM製作例項在本機的連接埠上執行 **4502**.
1. 從命令列導覽至 `aem-guides-wknd` 項目目錄。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 執行下列命令以建立整個專案並部署至AEM:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   組建需要約一分鐘的時間，且應會以下列訊息結束：

   ```
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.269 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  8.047 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [01:02 min]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  1.985 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  8.037 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  4.672 s]
   [INFO] WKND Sites Project - UI config ..................... SUCCESS [  0.313 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.270 s]
   [INFO] WKND Sites Project - Integration Tests ............. SUCCESS [ 15.571 s]
   [INFO] WKND Sites Project - Dispatcher .................... SUCCESS [  0.232 s]
   [INFO] WKND Sites Project - UI Tests ...................... SUCCESS [  0.728 s]
   [INFO] WKND Sites Project - Project Analyser .............. SUCCESS [ 33.398 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  02:18 min
   [INFO] Finished at: 2021-01-31T12:33:56-08:00
   [INFO] ------------------------------------------------------------------------
   ```

   Maven的個人資料 `autoInstallSinglePackage` 編譯專案的個別模組，並將單一套件部署至AEM例項。 依預設，此套件會部署至本機在連接埠上執行的AEM執行個體 **4502** 而且憑 `admin:admin`.

1. 導覽至您本機AEM執行個體上的套件管理器： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您應會看到 `aem-guides-wknd.ui.apps`, `aem-guides-wknd.ui.config`, `aem-guides-wknd.ui.content`，和 `aem-guides-wknd.all`.

1. 導覽至Sites Console: [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content). WKND站點將是其中一個站點。 其中將包含具有美國和語言主版階層的網站結構。 此網站階層是以 `language_country` 和 `isSingleCountryWebsite` 使用原型產生專案時。

1. 開啟 **US** `>` **英文** 頁面，方法是選取頁面並按一下 **編輯** 按鈕（在菜單欄中）:

   ![網站主控台](assets/project-setup/aem-sites-console.png)

1. 已建立入門內容，並可將多個元件新增至頁面。 試用這些元件，以了解功能。 您將在下一章中了解元件的基本知識。

   ![首頁入門內容](assets/project-setup/start-home-page.png)

   *原型產生的範例內容*

## Inspect專案 {#project-structure}

產生的AEM專案由個別Maven模組組成，每個模組具有不同的角色。 本教學課程和大部分開發工作都著重於這些模組：

* [核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html) - Java Code，主要是後端開發人員。
* [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)  — 包含CSS、JavaScript、Sass、類型指令碼的原始碼，主要供前端開發人員使用。
* [ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)  — 包含元件和對話方塊定義，內嵌編譯的CSS和JavaScript作為用戶端程式庫。
* [ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)  — 包含結構內容和設定，如可編輯的範本、中繼資料結構(/content, /conf)。

* **all**  — 這是空的Maven模組，將上述模組結合為可部署至AEM環境的單一套件。

![Maven專案圖](assets/project-setup/project-pom-structure.png)

請參閱 [AEM專案原型檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 若要深入了解 **all** Maven模組。

### 納入核心元件 {#core-components}

[AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant) 是一組適用於AEM的標準化網頁內容管理(WCM)元件。 這些元件提供功能的基準集，設計為針對個別專案進行樣式設定、自訂和延伸。

AEMas a Cloud Service環境包含 [AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). 因此，為AEMas a Cloud Service產生的專案會 **not** 包含AEM核心元件內嵌。

針對AEM 6.5/6.4產生的專案，原型會自動內嵌 [AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html) 在專案中。 AEM 6.5/6.4內嵌AEM核心元件是最佳實務，可確保專案部署最新版本。 核心元件的相關資訊 [包含在專案中，可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/using.html#core-components).

## 原始碼管理 {#source-control}

最好使用某種形式的原始碼控制來管理應用程式中的代碼。 本教學課程使用Git和GitHub。 由Maven和/或所選的IDE生成的多個檔案應被SCM忽略。

每當您建置和安裝程式碼套件時，Maven都會建立目標資料夾。 目標資料夾和內容應從SCM中排除。

下方 `ui.apps` 請注意 `.content.xml` 檔案。 這些XML檔案映射JCR中安裝的內容的節點類型和屬性。 這些檔案很重要，應 **not** 被忽略。

AEM專案原型將產生範例 `.gitignore` 檔案，可作為安全忽略檔案的起點。 檔案是在 `<src>/aem-guides-wknd/.gitignore`.

## 恭喜！ {#congratulations}

恭喜，您剛剛建立了第一個AEM專案！

### 後續步驟 {#next-steps}

透過簡單的 `HelloWorld` 範例，搭配 [元件基本知識](component-basics.md) 教學課程。

## 高級Maven命令（額外） {#advanced-maven-commands}

在開發期間，您可能只使用其中一個模組，並且想要避免建立整個專案，以節省時間。 您也可以直接部署至AEM Publish執行個體，或部署至未在連接埠4502上執行的AEM執行個體。

接下來，我們將介紹一些額外的Maven配置檔案和命令，這些配置檔案和命令可在開發過程中提供更大的靈活性。

### 核心模組 {#core-module}

此 **[核心](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/core.html)** 模組包含與專案相關聯的所有Java程式碼。 建置後，會將OSGi套件組合部署至AEM。 若要僅建置此模組：

1. 導覽至 `core` 資料夾（在下方） `aem-guides-wknd`):

   ```shell
   $ cd core/
   ```

1. 執行下列命令：

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

1. 導覽至 [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles). 這是OSGi Web控制台，包含AEM執行個體上安裝之所有套件組合的相關資訊。

1. 切換 **Id** 排序列，您應該會看到WKND套件已安裝且已啟用。

   ![核心套件](assets/project-setup/wknd-osgi-console.png)

1. 您可以在 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/wknd-packages/application/install/aem-guides-wknd.core-1.0.0-SNAPSHOT.jar):

   ![Jar的CRXDE位置](assets/project-setup/jcr-bundle-location.png)

### Ui.apps和Ui.content模組 {#apps-content-module}

此 **[ui.apps](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uiapps.html)** maven模組包含網站下方所需的所有轉譯程式碼 `/apps`. 這包括將以AEM格式儲存的CSS/JS，稱為 [clientlibs](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html). 這也包括 [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/overview.html?lang=zh-Hant) 用於呈現動態HTML的指令碼。 您可以想到 **ui.apps** 模組，作為JCR中結構的映射，但格式可儲存在檔案系統上並提交到原始碼控制。 此 **ui.apps** 模組僅包含程式碼。

若要建立此模組：

1. 從命令列。 導覽至 `ui.apps` 資料夾（在下方） `aem-guides-wknd`):

   ```shell
   $ cd ../ui.apps
   ```

1. 執行下列命令：

   ```shell
   $ mvn clean install -PautoInstallPackage
   ...
   Package installed in 122ms.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.972 s
   [INFO] Finished at: 2019-12-06T14:44:12-08:00
   [INFO] ------------------------------------------------------------------------
   ```

1. 導覽至 [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). 您應會看到 `ui.apps` 軟體包作為第一個安裝的軟體包，它應具有比任何其他軟體包更新的時間戳。

   ![已安裝Ui.apps套件](assets/project-setup/ui-apps-package.png)

1. 返回命令行並運行以下命令(在 `ui.apps` 資料夾):

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

   設定檔 `autoInstallPackagePublish` 是要將套件部署至在連接埠上執行的發佈環境 **4503**. 如果找不到在http://localhost:4503上執行的AEM例項，就會出現上述錯誤。

1. 最後，運行以下命令以部署 `ui.apps` 埠上的包 **4504**:

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

   如果埠上未執行任何AEM執行個體，則預計會發生組建失敗 **4504** 的URL區段。 參數 `aem.port` 在的POM檔案中定義 `aem-guides-wknd/pom.xml`.

此 **[ui.content](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uicontent.html)** 模組的結構與 **ui.apps** 模組。 唯一的區別是 **ui.content** 模組包含所謂的 **可變** 內容。 **可變** 內容主要是指儲存在原始碼控制項中的非代碼配置，如模板、策略或資料夾結構 **但** 可直接在AEM例項上修改。 有關詳細資訊，請參閱頁面和範本一章。

用來建立 **ui.apps** 模組可用來建置 **ui.content** 模組。 歡迎在 **ui.content** 檔案夾。
