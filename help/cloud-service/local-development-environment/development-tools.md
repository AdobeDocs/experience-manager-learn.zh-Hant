---
title: 將AEM的開發工具設定為雲端服務開發
description: 使用針對本機AEM進行開發所需的所有基準工具，來設定本機開發機器。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
translation-type: tm+mt
source-git-commit: cb5f3c323c433c9321ba26ac1194be0cd225a405
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 0%

---


# 設定開發工具

Adobe Experience Manager(AEM)開發需要在開發人員機器上安裝和設定最少的開發工具集。 這些工具支援AEM Projects的開發與建立。

請注意， `~` 這是用戶目錄的速記。 在Windows中，這相當於 `%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK來支援開發和AEM做為雲端服務SDK。

1. [下載並安裝最新版Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+111%7E&amp;orderby=%40jcry%3E&amp;orderby=%40jcr%3Econtent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 執行下列命令以確認Java 11 SDK是否已安裝：
   + Windows: `java -version`
   + macOS / Linux: `java --version`

![Java](./assets/development-tools/java.png)

## 安裝Homebrew

_使用Homebrew是選用的，但建議使用。_

Homebrew是適用於macOS、Windows和Linux的開放原始碼封裝管理程式。 所有支援工具皆可個別安裝，Homebrew提供方便的方式來安裝和更新Experience Manager開發所需的各種開發工具。

1. 開啟您的終端機
1. 運行以下命令檢查Homebrew是否已安裝： `brew --version`.
1. 如果未安裝Homebrew，請安裝Homebrew
   + [在macOS上安裝Homebrew](https://brew.sh/)
      + 在macOS上的Homebrew需 [要Xcode](https://apps.apple.com/us/app/xcode/id497799835)[或命令列工具](https://developer.apple.com/download/more/)，可透過下列命令安裝：
         + `xcode-select --install`
   + [在Linux上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [在Windows 10上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 通過運行以下命令來驗證Homebrew是否已安裝： `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

如果您使用Homebrew，請依照下 __列章節中的__ 「使用Homebrew安裝」指示進行。 如果您不 __使用__ Homebrew，請使用作業系統特定連結來安裝工具。

## 安裝Git

[Git](https://git-scm.com/) 是 [](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html)Adobe Cloud Manager使用的來源控制管理系統，因此開發時需要用到它。

+ 使用Homebrew安裝Git
   1. 開啟您的終端／命令提示
   1. 執行命令： `brew install git`
   1. 使用以下命令驗證Git是否已安裝： `git --version`
+ 或者，下載並安裝Git（macOS、Linux或Windows）
   1. [下載和安裝Git](https://git-scm.com/downloads)
   1. 開啟您的終端／命令提示
   1. 使用以下命令驗證Git是否已安裝： `git --version`

![Git](./assets/development-tools/git.png)

## 安裝Node.js（和npm）{#node-js}

[Node.js](https://nodejs.org) 是JavaScript執行時期環境，用來處理AEM專案的 ____ ui.frontend子專案的前端資產。 Node.js與 [npm](https://www.npmjs.com/)一起分發，實際上是Node.js包管理器，用於管理JavaScript相依性。

+ 使用Homebrew安裝Node.js
   1. 開啟您的終端／命令提示
   1. 執行命令： `brew install node`
   1. 使用以下命令驗證Node.js是否已安裝： `node -v`
   1. 使用以下命令驗證是否已安裝npm: `npm -v`
+ 或者，下載並安裝Node.js（macOS、Linux或Windows）
   1. [下載並安裝Node.js](https://nodejs.org/en/download/)
   1. 開啟您的終端／命令提示
   1. 使用以下命令驗證Node.js是否已安裝： `node -v`
   1. 使用以下命令驗證是否已安裝npm: `npm -v`

![Node.js和npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)-based AEM Projects會在建立時安裝Node.js的隔離版本。 最好讓本機開發系統的版本與AEM Maven專案Reactor pom.xml中指定的Node.js和npm版本保持同步（或接近）。
>
>請參閱此范 [例AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) ，以取得Node.js和npm組建版本的位置。

## 安裝Maven

Apache Maven是開放原始碼Java命令列工具，用來建立由AEM Project Maven Archetype產生的AEM專案。 所有主要IDE([IntelliJ IDEA](https://www.jetbrains.com/idea/)、 [Visual Studio Code](https://code.visualstudio.com/)、 [Eclipse](https://www.eclipse.org/)等) 已整合Maven支援。

+ 使用Homebrew安裝Maven
   1. 開啟您的終端／命令提示
   1. 執行命令： `brew install maven`
   1. 使用以下命令驗證Maven是否已安裝： `mvn -v`
+ 或者，下載並安裝Maven（macOS、Linux或Windows）
   1. [下載Maven](https://maven.apache.org/download.cgi)
   1. [安裝Maven](https://maven.apache.org/install.html)
   1. 開啟您的終端／命令提示
   1. 使用以下命令驗證Maven是否已安裝： `mvn -v`

![馬文](./assets/development-tools/maven.png)

## 設定Adobe I/O CLI{#aio-cli}

[Adobe I/O CLI](https://github.com/adobe/aio-cli)，或 `aio`，可讓您存取多種Adobe服務的命令列，包括 [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) 和 [Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute)。 Adobe I/O CLI在AEM的雲端服務開發中扮演了不可或缺的角色，因為它可讓開發人員：

+ 從AEM以雲端服務的形式追蹤記錄
+ 從CLI管理Cloud Manager管線

### 安裝Adobe I/O CLI

1. 確保 [Node.js已安裝](#node-js) ，因為Adobe I/O CLI是npm模組
   + 執行 `node --version` 以確認
1. 執 `npm install -g @adobe/aio-cli` 行以全局 `aio` 安裝npm模組

### 設定Adobe I/O CLI Cloud Manager外掛程式{#aio-cloud-manager}

Adobe I/O Cloud Manager增效模組可讓aio CLI透過命令與Adobe Cloud Manager互 `aio cloudmanager` 動。

1. 執 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 行以安 [裝aio Cloud Manager外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

### 設定Adobe I/O CLI Asset Compute插件{#aio-asset-compute}

Adobe I/O Cloud Manager插件允許aio CLI通過命令生成並運行資產計算工 `aio asset-compute` 作器。

1. 執 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 行以安 [裝aio Asset Compute外掛程式](https://github.com/adobe/aio-cli-plugin-asset-compute)。

### 設定Adobe I/O CLI身份驗證

為了讓Adobe I/O CLI與Cloud Manager通訊，必須在Adobe I/O Console中建立Cloud Manager整合，並取得憑證才能成功驗證。

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. 登入 [console.adobe.io](https://console.adobe.io)
1. 確保您的組織（包含要連線至的Cloud Manager產品）在Adobe組織切換器中處於作用中
1. 建立新的或開啟現有 [Adobe I/O計畫](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O Console計畫只是組織整合群組、建立或使用現有計畫，視您管理整合的方式而定
   + 如果建立新專案，如果出現提示，請選取「空白專案」（與從範本建立）
   + Adobe I/O Console程式與Cloud Manager程式有不同的概念
1. 建立與「開發人員——雲端服務」設定檔整合的新Cloud Manager API
1. 取得需要填入Adobe I/O CLI [config.json的服務帳戶(JWT)憑證](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 將文 `config.json` 件載入到Adobe I/O CLI
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. 將文 `private.key` 件載入到Adobe I/O CLI
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

開 [始通過](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) Adobe I/O CLI執行Cloud Manager的命令。

## 設定開發IDE

AEM開發主要包含Java和前端（JavaScript、CSS等）開發與XML管理。 以下是AEM開發中最常用的IDE。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ 是功能強大的Java開發IDE。 IntelliJ IDEA提供兩種不同的版本：免費社群版和商業版（付費）旗艦版。 免費的社群版本已足以開發AEM，但Ultimate會擴充 [其功能集](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [下載IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下載回購工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio代碼

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)是免費的開放原始碼工具，適用於前端開發人員。 您可以設定Visual Studio程式碼，借由Adobe工具repo的協助，將內容同步與AEM __[整合](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__。

Visual Studio Code是前端開發人員最理想的選擇，主要是建立前端程式碼；JavaScript、CSS和HTML。 雖然VS程式碼透過擴充功能 [提供Java支援](https://code.visualstudio.com/docs/java/java-tutorial)，但可能缺乏更多Java專用的進階功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下載Visual Studio代碼](https://code.visualstudio.com/Download)
+ [下載回購工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下載Aemfed VS Code extension](https://aemfed.io/)
+ [下載AEM Sync VS Code extension](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ 是Java開發的常用IDE，支援Adobe提供的 __[AEM Developer Tools](https://eclipse.adobe.com/aem/dev-tools/)__ plug-in，提供IDE內建GUI以製作JCR內容，並與本機AEM例項同步化。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下載Eclipse](https://www.eclipse.org/ide/)
+ [下載Eclipse Dev工具](https://eclipse.adobe.com/aem/dev-tools/)
