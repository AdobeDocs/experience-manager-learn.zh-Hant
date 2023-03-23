---
title: 設定AEMas a Cloud Service開發的開發工具
description: 設定本機開發機器，其中包含針對本機AEM開發所需的所有基準工具。
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: e82c30e7f1a1fe04fd43ee639d74788f9bf100f6
workflow-type: tm+mt
source-wordcount: '1502'
ht-degree: 7%

---

# 設定開發工具 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="設定開發工具"
>abstract="展開 Adobe Experience Manager (AEM) 的開發作業前，開發人員的電腦上需先安裝與設定一套簡單的開發工具。這些工具包括 Java、Maven、Adobe I/O CLI、開發 IDE 等。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="開發準則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hant" text="開發基本概念"

展開 Adobe Experience Manager (AEM) 的開發作業前，開發人員的電腦上需先安裝與設定一套簡單的開發工具。這些工具可支援AEM專案的開發與建置。

請注意 `~` 用作用戶目錄的簡稱。 在Windows中，這等同於 `%HOMEPATH%`.

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK才能支援開發和AEMas a Cloud ServiceSDK。

1. [下載並安裝最新版Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14)
1. 執行以下命令以確認Java 11 SDK是否已安裝：
   + Windows: `java -version`
   + macOS / Linux : `java --version`

![Java](./assets/development-tools/java.png)

## 安裝Homebrew

_使用Homebrew是可選的，但建議使用。_

Homebrew是適用於macOS、Windows和Linux的開放原始碼套件管理器。 所有支援工具都可單獨安裝，Homebrew提供了安裝和更新Experience Manager開發所需的各種開發工具的方便方式。

1. 開啟您的終端機
1. 通過運行以下命令來檢查是否已安裝Homebrew: `brew --version`.
1. 如果未安裝Homebrew，請安裝Homebrew
   + [在macOS上安裝Homebrew](https://brew.sh/)
      + macOS的家釀 [Xcode](https://apps.apple.com/us/app/xcode/id497799835) 或 [命令行工具](https://developer.apple.com/download/more/)，可透過命令安裝：
         + `xcode-select --install`
   + [在Linux上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [在Windows 10上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 通過運行以下命令來驗證是否安裝了Homebrew: `brew --version`

![荷姆布魯](./assets/development-tools/homebrew.png)

如果您使用Homebrew，請遵循 __使用Homebrew安裝__ 以下各節中的說明。 如果您 __not__ 使用Homebrew，使用作業系統特定連結安裝工具。

## 安裝Git

[Git](https://git-scm.com/) 是使用的原始碼管理系統 [AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)，因此是發展的必要條件。

+ 使用Homebrew安裝Git
   1. 開啟終端機/命令提示字元
   1. 執行命令： `brew install git`
   1. 使用以下命令確認Git是否已安裝： `git --version`
+ 或者，下載並安裝Git(macOS、Linux或Windows)
   1. [下載及安裝Git](https://git-scm.com/downloads)
   1. 開啟終端機/命令提示字元
   1. 使用以下命令確認Git是否已安裝： `git --version`

![Git](./assets/development-tools/git.png)

## 安裝Node.js（和npm）{#node-js}

[Node.js](https://nodejs.org) 是JavaScript執行階段環境，用於搭配AEM專案的前端資產使用 __ui.frontend__ 子專案。 Node.js與 [npm](https://www.npmjs.com/)，是事實上的Node.js套件管理器，用於管理JavaScript相依性。

+ 使用Homebrew安裝Node.js
   1. 開啟終端機/命令提示字元
   1. 執行命令： `brew install node`
   1. 使用命令驗證是否已安裝Node.js : `node -v`
   1. 使用以下命令驗證是否已安裝npm: `npm -v`
+ 或者，下載並安裝Node.js(macOS、Linux或Windows)
   1. [下載並安裝Node.js](https://nodejs.org/en/download/)
   1. 開啟終端機/命令提示字元
   1. 使用命令驗證是否已安裝Node.js : `node -v`
   1. 使用以下命令驗證是否已安裝npm: `npm -v`

![Node.js 和 npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM專案原型](https://github.com/adobe/aem-project-archetype)-based AEM專案會在建置時安裝孤立的Node.js版本。 最好將本機開發系統的版本保持同步（或接近）AEM Maven專案的pom.xml中指定的Node.js和npm版本。
>
>請參閱此範例 [AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) 用於尋找Node.js和npm組建版本的位置。

## 安裝Maven

Apache Maven是開放原始碼的Java命令列工具，用來建置從AEM Project Maven原型產生的AEM專案。 所有主要IDE([IntelliJ IDEA](https://www.jetbrains.com/idea/), [Visual Studio代碼](https://code.visualstudio.com/), [Eclipse](https://www.eclipse.org/)等) 已整合Maven支援。

+ 使用Homebrew安裝Maven
   1. 開啟終端機/命令提示字元
   1. 執行命令： `brew install maven`
   1. 使用命令驗證是否已安裝Maven : `mvn -v`
+ 或者，下載並安裝Maven(macOS、Linux或Windows)
   1. [下載Maven](https://maven.apache.org/download.cgi)
   1. [安裝Maven](https://maven.apache.org/install.html)
   1. 開啟終端機/命令提示字元
   1. 使用命令驗證是否已安裝Maven : `mvn -v`

![馬文](./assets/development-tools/maven.png)

## 設定Adobe I/OCLI{#aio-cli}

此 [Adobe I/OCLI](https://github.com/adobe/aio-cli)，或 `aio`，可提供對各種Adobe服務的命令列存取，包括 [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) 和 [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/OCLI在AEMas a Cloud Service的開發中扮演著不可或缺的角色，因為它可讓開發人員：

+ 從AEM as a Cloud Services服務追蹤記錄
+ 從CLI管理Cloud Manager管道
+ 部署至 [AEM快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### 安裝Adobe I/OCLI

1. 確保 [已安裝Node.js](#node-js) 因為Adobe I/OCLI是npm模組
   + 執行 `node --version` 確認
1. 執行 `npm install -g @adobe/aio-cli` 安裝 `aio` 全域npm模組

### 設定Adobe I/OCLI Cloud Manager插件{#aio-cloud-manager}

Adobe I/OCloud Manager增效模組可讓aio CLI透過 `aio cloudmanager` 命令。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 安裝 [aio Cloud Manager外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### 設定Adobe I/OCLI身份驗證

為了讓Adobe I/OCLI與Cloud Manager通訊， [必須在Cloud Manager主控台中建立Cloud Manager整合Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager)必須取得和憑證才能成功驗證。

1. 登入 [console.adobe.io](https://console.adobe.io)
1. 確認您所在的組織（包括要連線的Cloud Manager產品）在Adobe組織切換器中處於作用中狀態
1. 建立新或開啟現有 [Adobe I/O方案](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O主控台程式只是整合的組織群組，根據您管理整合的方式建立或使用現有程式
   + 如果建立新專案，如果出現提示，請選取「空白專案」(與&quot;從模板建立&quot;)
   + Adobe I/O主控台程式是與Cloud Manager程式不同的概念
1. 使用「開發人員 — Cloud Service」設定檔建立新的Cloud Manager API整合
1. 取得需要填入Adobe I/OCLI的服務帳戶(JWT)憑證 [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 載入 `config.json` 檔案放入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. 載入 `private.key` 檔案放入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

開始 [執行命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) (透過Adobe I/OCLI)取得Cloud Manager的說明。

### 設定AEM Rapid Development Environment外掛程式{#rde}

AEM Rapid Development Environment外掛程式可讓aio CLI與AEMas a Cloud Service互動 [快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) 透過 `aio aem:rde` 命令。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-aem-rde` 安裝 [AEM Rapid Development Environments外掛程式](https://github.com/adobe/aio-cli-plugin-aem-rde).

### 設定Adobe I/OCLIAsset compute插件{#aio-asset-compute}

Adobe I/OCloud Manager增效模組可讓aio CLI透過 `aio asset-compute` 命令。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-asset-compute` 安裝 [aioAsset compute外掛程式](https://github.com/adobe/aio-cli-plugin-asset-compute).


## 設定開發IDE

AEM開發主要包含Java和前端（JavaScript、CSS等）開發及XML管理。 以下是AEM開發中最常用的IDE。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ 是功能強大的Java開發IDE。 IntelliJ IDEA提供兩種風格，一種是免費的社群版本，另一種是商業版（付費）Ultimate版本。 免費的社群版本就足以供AEM開發使用，不過最終版 [擴展其能力集](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [下載IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio代碼](https://code.visualstudio.com/)__ (VS Code)是前端開發人員專屬的免費開放原始碼工具。 可設定Visual Studio Code ，以便借助Adobe工具將內容同步與AEM整合。 __[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code是主要建立前端代碼的前端開發人員的理想選擇；JavaScript、CSS和HTML。 而VS程式碼則透過 [擴充功能](https://code.visualstudio.com/docs/java/java-tutorial)，則可能缺少更具Java特性的進階功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下載Visual Studio代碼](https://code.visualstudio.com/Download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下載Aemfed VS Code擴充功能](https://aemfed.io/)
+ [下載AEM Sync VS程式碼擴充功能](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ 是Java開發的常用IDE，並支援  __[AEM開發人員工具](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ 由Adobe提供的插件，提供用於創作的IDE內GUI，並將JCR內容與本地AEM實例同步。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下載Eclipse](https://www.eclipse.org/ide/)
+ [下載Eclipse開發工具](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
