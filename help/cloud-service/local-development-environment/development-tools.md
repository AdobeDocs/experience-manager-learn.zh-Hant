---
title: 設定AEM的開發工具作為Cloud Service開發
description: 設定本機開發機器，其中包含針對本機AEM開發所需的所有基準工具。
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 0737cd2410b48dbaa9b6dfaaa27b854d44536f15
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 1%

---


# 設定開發工具

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="設定開發工具"
>abstract="Adobe Experience Manager(AEM)開發作業需要在開發人員電腦上安裝及設定一套簡單的開發工具。 這些工具包括Java、Maven、Adobe I/OCLI、開發IDE等。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="開發准則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="開發基本知識"

Adobe Experience Manager(AEM)開發作業需要在開發人員電腦上安裝及設定一套簡單的開發工具。 這些工具可支援AEM專案的開發與建置。

請注意， `~`是用戶目錄的簡稱。 在Windows中，這等同於`%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK才能支援開發，而AEM則是Cloud ServiceSDK。

1. [下載並安裝最新版Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14)
1. 執行以下命令以確認Java 11 SDK是否已安裝：
   + Windows: `java -version`
   + macOS / Linux:`java --version`

![Java](./assets/development-tools/java.png)

## 安裝Homebrew

_使用Homebrew是可選的，但建議使用。_

Homebrew是適用於macOS、Windows和Linux的開放原始碼套件管理器。 所有支援工具都可單獨安裝，Homebrew提供了安裝和更新Experience Manager開發所需的各種開發工具的方便方式。

1. 開啟您的終端機
1. 通過運行以下命令來檢查是否已安裝Homebrew:`brew --version`。
1. 如果未安裝Homebrew，請安裝Homebrew
   + [在macOS上安裝Homebrew](https://brew.sh/)
      + macOS上的Homebrew需要[Xcode](https://apps.apple.com/us/app/xcode/id497799835)或[命令行工具](https://developer.apple.com/download/more/)，可通過以下命令進行安裝：
         + `xcode-select --install`
   + [在Linux上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [在Windows 10上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 通過運行以下命令來驗證是否安裝了Homebrew:`brew --version`

![荷姆布魯](./assets/development-tools/homebrew.png)

如果您使用Homebrew，請遵循以下各節中的&#x200B;__使用Homebrew__&#x200B;安裝指示。 如果您是使用Homebrew的&#x200B;__not__，請使用作業系統特定連結安裝工具。

## 安裝Git

[](https://git-scm.com/) 提供Experience Cloud Manager所使用的原始 [碼控制管理系統](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)，因此是開發所需的。

+ 使用Homebrew安裝Git
   1. 開啟終端機/命令提示字元
   1. 執行命令：`brew install git`
   1. 使用以下命令確認Git是否已安裝：`git --version`
+ 或者，下載並安裝Git（macOS、Linux或Windows）
   1. [下載及安裝Git](https://git-scm.com/downloads)
   1. 開啟終端機/命令提示字元
   1. 使用以下命令確認Git是否已安裝：`git --version`

![Git](./assets/development-tools/git.png)

## 安裝Node.js（和npm）{#node-js}

[Node.](https://nodejs.org) jsis是JavaScript執行階段環境，用於搭配AEM專案的ui.frontendsub專案的 __前端資__ 產使用。Node.js與[npm](https://www.npmjs.com/)一起發佈，是事實上的Node.js套件管理器，用於管理JavaScript相依性。

+ 使用Homebrew安裝Node.js
   1. 開啟終端機/命令提示字元
   1. 執行命令：`brew install node`
   1. 使用命令驗證是否已安裝Node.js :`node -v`
   1. 使用以下命令驗證是否已安裝npm:`npm -v`
+ 或者，下載並安裝Node.js（macOS、Linux或Windows）
   1. [下載並安裝Node.js](https://nodejs.org/en/download/)
   1. 開啟終端機/命令提示字元
   1. 使用命令驗證是否已安裝Node.js :`node -v`
   1. 使用以下命令驗證是否已安裝npm:`npm -v`

![Node.js 和 npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM專案原型](https://github.com/adobe/aem-project-archetype)型的AEM專案會在建置時安裝孤立的Node.js版本。最好將本機開發系統的版本同步（或接近）AEM Maven專案的pom.xml中指定的Node.js和npm版本。
>
>請參閱此範例[AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118)，以了解如何找到Node.js和npm組建版本。

## 安裝Maven

Apache Maven是開放原始碼的Java命令列工具，用來建置從AEM Project Maven原型產生的AEM專案。 所有主要IDE（[IntelliJ IDEA](https://www.jetbrains.com/idea/)、[Visual Studio代碼](https://code.visualstudio.com/)、[Eclipse](https://www.eclipse.org/)等） 已整合Maven支援。

+ 使用Homebrew安裝Maven
   1. 開啟終端機/命令提示字元
   1. 執行命令：`brew install maven`
   1. 使用命令驗證是否已安裝Maven :`mvn -v`
+ 或者，下載並安裝Maven（macOS、Linux或Windows）
   1. [下載Maven](https://maven.apache.org/download.cgi)
   1. [安裝Maven](https://maven.apache.org/install.html)
   1. 開啟終端機/命令提示字元
   1. 使用命令驗證是否已安裝Maven :`mvn -v`

![馬文](./assets/development-tools/maven.png)

## 設定Adobe I/OCLI{#aio-cli}

[Adobe I/OCLI](https://github.com/adobe/aio-cli)或`aio`提供對多種Adobe服務(包括[Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)和[Asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute))的命令行訪問。 Adobe I/OCLI在AEM作為Cloud Service的開發中扮演著不可或缺的角色，因為它可讓開發人員：

+ 從AEM as a Cloud Services服務追蹤記錄
+ 從CLI管理Cloud Manager管道

### 安裝Adobe I/OCLI

1. 確保[Node.js已安裝](#node-js)，因為Adobe I/OCLI是npm模組
   + 運行`node --version`以確認
1. 執行`npm install -g @adobe/aio-cli`以全局安裝`aio` npm模組

### 設定Adobe I/OCLI Cloud Manager插件{#aio-cloud-manager}

Adobe I/OCloud Manager增效模組可讓aio CLI透過`aio cloudmanager`命令與AdobeCloud Manager互動。

1. 執行`aio plugins:install @adobe/aio-cli-plugin-cloudmanager`以安裝[aio Cloud Manager增效模組](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

### 設定Adobe I/OCLIAsset compute插件{#aio-asset-compute}

Adobe I/OCloud Manager增效模組可讓aio CLI透過`aio asset-compute`命令產生及執行Asset compute背景工作。

1. 執行`aio plugins:install @adobe/aio-cli-plugin-asset-compute`以安裝[aioAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)。

### 設定Adobe I/OCLI身份驗證

為了讓Adobe I/OCLI與Cloud Manager通訊，必須在Adobe I/O控制台](https://github.com/adobe/aio-cli-plugin-cloudmanager)中建立[ Cloud Manager整合，並取得憑證以成功驗證。

1. 登入[console.adobe.io](https://console.adobe.io)
1. 確認您所在的組織（包括要連線的Cloud Manager產品）在Adobe組織切換器中處於作用中狀態
1. 建立新程式或開啟現有的[Adobe I/O程式](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O主控台程式只是整合的組織群組，根據您管理整合的方式建立或使用現有程式
   + 如果建立新專案，如果出現提示，請選取「空白專案」(與&quot;從模板建立&quot;)
   + Adobe I/O主控台程式是與Cloud Manager程式不同的概念
1. 使用「開發人員 — Cloud Service」設定檔建立新的Cloud Manager API整合
1. 取得服務帳戶(JWT)憑證，需要填入Adobe I/OCLI的[config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 將`config.json`檔案載入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. 將`private.key`檔案載入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

通過Adobe I/OCLI開始[執行Cloud Manager的命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands)。

## 設定開發IDE

AEM開發主要包含Java和前端（JavaScript、CSS等）開發及XML管理。 以下是AEM開發中最常用的IDE。

### IntelliJ IDEA

__[IntelliJ ](https://www.jetbrains.com/idea/)__ IDEA是功能強大的Java開發IDE。IntelliJ IDEA提供兩種風格，一種是免費的社群版本，另一種是商業版（付費）Ultimate版本。 免費的Community版本足以用於AEM開發，但Ultimate [會擴充其功能集](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [下載IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio代碼

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)是前端開發人員的免費開放原始碼工具。可設定Visual Studio Code ，以便借助Adobe工具&#x200B;__[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__&#x200B;將內容同步與AEM整合。

Visual Studio Code是主要建立前端代碼的前端開發人員的理想選擇；JavaScript、CSS和HTML。 雖然VS程式碼透過[擴充功能](https://code.visualstudio.com/docs/java/java-tutorial)支援Java，但可能缺少更具Java特定性的部分進階功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下載Visual Studio代碼](https://code.visualstudio.com/Download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下載Aemfed VS Code擴充功能](https://aemfed.io/)
+ [下載AEM Sync VS程式碼擴充功能](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ 是Java開發的常用IDE，支援由Adobe提  __[供的AEM](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ 開發工具外掛程式，提供IDE內部GUI，用於編寫JCR內容，並將JCR內容與本機AEM執行個體同步。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下載Eclipse](https://www.eclipse.org/ide/)
+ [下載Eclipse開發工具](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
