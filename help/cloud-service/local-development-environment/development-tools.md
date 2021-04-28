---
title: 設定開發工具，AEM做為Cloud Service開發
description: 建立具備針對本機進行開發所需所有基準工具的本機開發AEM機器。
feature: 開發人員工具
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4267
thumbnail: 25907.jpg
topic: 開發
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '1426'
ht-degree: 1%

---


# 設定開發工具

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="設定開發工具"
>abstract="Adobe Experience Manager(AEM)開發需要在開發人員機器上安裝和設定最少的開發工具集。 這些工具包括Java、Maven、Adobe I/OCLI、開發IDE等。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="開發方針"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="開發基本資訊"

Adobe Experience Manager(AEM)開發需要在開發人員機器上安裝和設定最少的開發工具集。 這些工具可支援專案的開發AEM與建立。

請注意，`~`是用作用戶目錄的速記。 在Windows中，此值相當於`%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK來支援開發，並以AEMCloud ServiceSDK的形式提供。

1. [下載並安裝最新版Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+111%7E+11%7E&amp;orderby=%3E=%40jcr%3E&amp;orderby=%3Econtent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 執行下列命令以確認Java 11 SDK是否已安裝：
   + Windows: `java -version`
   + macOS / Linux:`java --version`

![Java](./assets/development-tools/java.png)

## 安裝Homebrew

_使用Homebrew是選用的，但建議使用。_

Homebrew是適用於macOS、Windows和Linux的開放原始碼封裝管理程式。 所有支援工具都可單獨安裝，Homebrew提供方便的方式來安裝和更新Experience Manager開發所需的各種開發工具。

1. 開啟您的終端機
1. 運行以下命令檢查Homebrew是否已安裝：`brew --version`。
1. 如果未安裝Homebrew，請安裝Homebrew
   + [在macOS上安裝Homebrew](https://brew.sh/)
      + macOS上的Homebrew需要[Xcode](https://apps.apple.com/us/app/xcode/id497799835)或[命令列工具](https://developer.apple.com/download/more/)，可透過以下命令安裝：
         + `xcode-select --install`
   + [在Linux上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [在Windows 10上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 通過運行以下命令來驗證Homebrew是否已安裝：`brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

如果您使用Homebrew，請遵循以下各節中的「使用Homebrew安裝」指示。 ____&#x200B;如果您&#x200B;__不是__&#x200B;使用Homebrew，請使用作業系統特定連結來安裝工具。

## 安裝Git

[指](https://git-scm.com/) 出Adobe雲管理器使用的 [源控制管理系統](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/requirements/source-code-repository.html)，是開發的必要條件。

+ 使用Homebrew安裝Git
   1. 開啟您的終端／命令提示
   1. 執行命令：`brew install git`
   1. 使用以下命令驗證Git是否已安裝：`git --version`
+ 或者，下載並安裝Git（macOS、Linux或Windows）
   1. [下載和安裝Git](https://git-scm.com/downloads)
   1. 開啟您的終端／命令提示
   1. 使用以下命令驗證Git是否已安裝：`git --version`

![Git](./assets/development-tools/git.png)

## 安裝Node.js（和npm）{#node-js}

[Node.](https://nodejs.org) jsis是JavaScript執行時期環境，用來處理專案的AEMui.frontendsub專案的前端 __資__ 產。Node.js與[npm](https://www.npmjs.com/)一起分發，實際上是Node.js包管理器，用於管理JavaScript相依性。

+ 使用Homebrew安裝Node.js
   1. 開啟您的終端／命令提示
   1. 執行命令：`brew install node`
   1. 使用以下命令驗證Node.js是否已安裝：`node -v`
   1. 使用以下命令驗證是否已安裝npm:`npm -v`
+ 或者，下載並安裝Node.js（macOS、Linux或Windows）
   1. [下載並安裝Node.js](https://nodejs.org/en/download/)
   1. 開啟您的終端／命令提示
   1. 使用以下命令驗證Node.js是否已安裝：`node -v`
   1. 使用以下命令驗證是否已安裝npm:`npm -v`

![Node.js 和 npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[Project Archype](https://github.com/adobe/aem-project-archetype)-based AEM Projects會在建立時安裝Node.js的隔離版本。最好讓本機開發系統的版本與Maven專案的Reactor pom.xml中指定的Node.js和npm版本保持同步（或接近）AEM。
>
>請參閱此示例[AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118)，以查找Node.js和npm組建版本的位置。

## 安裝Maven

Apache Maven是開放原始碼Java命令列工具，用於建立從AEMProject Maven Archetype產生AEM的專案。 所有主要IDE（[IntelliJ IDEA](https://www.jetbrains.com/idea/)、[Visual Studio代碼](https://code.visualstudio.com/)、[Eclipse](https://www.eclipse.org/)等） 已整合Maven支援。

+ 使用Homebrew安裝Maven
   1. 開啟您的終端／命令提示
   1. 執行命令：`brew install maven`
   1. 使用以下命令驗證Maven是否已安裝：`mvn -v`
+ 或者，下載並安裝Maven（macOS、Linux或Windows）
   1. [下載Maven](https://maven.apache.org/download.cgi)
   1. [安裝Maven](https://maven.apache.org/install.html)
   1. 開啟您的終端／命令提示
   1. 使用以下命令驗證Maven是否已安裝：`mvn -v`

![馬文](./assets/development-tools/maven.png)

## 設定Adobe I/OCLI{#aio-cli}

[Adobe I/OCLI](https://github.com/adobe/aio-cli)或`aio`提供對多種Adobe服務的命令行訪問，包括[雲管理器](https://github.com/adobe/aio-cli-plugin-cloudmanager)和[Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute)。 Adobe I/OCLI在開發中扮演了不可或缺的角AEM色，因為它為開發人員提供了以下能力：

+ 尾隨日誌AEM作為Cloud Services服務
+ 從CLI管理Cloud Manager管線

### 安裝Adobe I/OCLI

1. 確保[Node.js已安裝](#node-js)，因為Adobe I/OCLI是npm模組
   + 運行`node --version`以確認
1. 執行`npm install -g @adobe/aio-cli`以全局安裝`aio` npm模組

### 設定Adobe I/OCLI Cloud Manager插件{#aio-cloud-manager}

Adobe I/O雲管理器插件允許aio CLI通過`aio cloudmanager`命令與Adobe雲管理器交互。

1. 執行`aio plugins:install @adobe/aio-cli-plugin-cloudmanager`以安裝[aio Cloud Manager插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

### 設定Adobe I/OCLIAsset compute插件{#aio-asset-compute}

Adobe I/O雲管理器插件允許aio CLI通過`aio asset-compute`命令生成並運行Asset compute工作器。

1. 執行`aio plugins:install @adobe/aio-cli-plugin-asset-compute`以安裝[aioAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)。

### 設定Adobe I/OCLI身份驗證

為了讓Adobe I/OCLI與Cloud Manager通信，必須在Adobe I/O控制台中建立Cloud Manager整合，並且必須獲得憑據才能成功驗證。

>[!VIDEO](https://video.tv.adobe.com/v/35094?quality=12&learn=on)

1. 登入[console.adobe.io](https://console.adobe.io)
1. 確保您的「組織」（包含要連接的Cloud Manager產品）在「Adobe組織」切換器中處於活動狀態
1. 建立新程式或開啟現有的[Adobe I/O程式](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O主控台計畫只是組織整合的群組，根據您管理整合的方式建立或使用現有計畫
   + 如果建立新專案，如果出現提示，請選取「空白專案」（與從範本建立）
   + Adobe I/O控制台程式是與Cloud Manager程式不同的概念
1. 建立與「開發人員-Cloud Service」設定檔整合的新Cloud Manager API
1. 獲取需要填充Adobe I/OCLI的[config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)的服務帳戶(JWT)憑據
1. 將`config.json`檔案載入到Adobe I/OCLI中
   + `$ aio config:set jwt-auth PATH_TO_CONFIG_JSON_FILE --file --json`
1. 將`private.key`檔案載入到Adobe I/OCLI中
   + `$ aio config:set jwt-auth.jwt_private_key PATH_TO_PRIVATE_KEY_FILE --file`

通過Adobe I/OCLI開始執行Cloud Manager的[命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands)。

## 設定開發IDE

開AEM發主要包含Java和前端（JavaScript、CSS等）開發和XML管理。 以下是開發時最常用的AEMIDE。

### IntelliJ IDEA

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEA是功能強大的Java開發IDE。IntelliJ IDEA提供兩種不同的版本：免費社群版和商業版（付費）旗艦版。 免費的社群版本足以AEM開發，但Ultimate [擴充了其功能集](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [下載IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下載回購工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio代碼

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)是免費的開放原始碼工具，適用於前端開發人員。您可以設定Visual Studio程式碼，AEM借由Adobe工具&#x200B;__[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__，將內容同步化。

Visual Studio Code是前端開發人員最理想的選擇，主要是建立前端程式碼；JavaScript、CSS和HTML。 雖然VS程式碼透過[擴充功能](https://code.visualstudio.com/docs/java/java-tutorial)提供Java支援，但可能缺乏更多Java專用的進階功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下載Visual Studio代碼](https://code.visualstudio.com/Download)
+ [下載回購工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下載Aemfed VS Code extension](https://aemfed.io/)
+ [下載AEM同步與程式碼擴充功能](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse ](https://www.eclipse.org/ide/)__ IDE是Java開發中常用的IDE，並支援由Adobe提供的AEMDeveloper   __[](https://eclipse.adobe.com/aem/dev-tools/)__ Tools外掛程式，提供IDE內建GUI以製作JCR內容，並將JCR內容與本機執AEM行個體同步。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下載Eclipse](https://www.eclipse.org/ide/)
+ [下載Eclipse Dev工具](https://eclipse.adobe.com/aem/dev-tools/)
