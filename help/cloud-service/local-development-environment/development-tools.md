---
title: 設定開發工具AEM以as a Cloud Service開發
description: 設定本地開發機器，其中包含針對本地開發所需的所有基AEM準工具。
feature: Developer Tools
version: Cloud Service
kt: 4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: bca51ece7a9b249727b8746cc9654503059116fb
workflow-type: tm+mt
source-wordcount: '1435'
ht-degree: 1%

---

# 設定開發工具 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="設定開發工具"
>abstract="Adobe Experience Manager(AEM)開發需要在開發機上安裝和設定一套最少的開發工具。 這些工具包括Java、Maven、Adobe I/OCLI、開發IDE等。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="發展准則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="開發基礎"

Adobe Experience Manager(AEM)開發需要在開發機上安裝和設定一套最少的開發工具。 這些工具支援項目的開AEM發和建設。

請注意 `~` 用作用戶目錄的簡寫。 在Windows中，這相當於 `%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK支援開發和AEMas a Cloud ServiceSDK。

1. [下載並安裝最新版本的Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;order=%40jcr%3內容%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 通過運行以下命令驗證Java 11 SDK是否已安裝：
   + Windows: `java -version`
   + macOS/Linux: `java --version`

![Java](./assets/development-tools/java.png)

## 安裝Homebrew

_使用Homebrew是可選的，但建議使用。_

Homebrew是macOS,Windows和Linux的開源軟體包管理器。 所有支援工具都可以單獨安裝，Homebrew為安裝和更新Experience Manager開發所需的各種開發工具提供了一種方便的方法。

1. 開啟終端
1. 運行以下命令檢查Homebrew是否已安裝： `brew --version`。
1. 如果未安裝Homebrew，請安裝Homebrew
   + [在macOS安裝Homebrew](https://brew.sh/)
      + macOS的家釀 [Xcode](https://apps.apple.com/us/app/xcode/id497799835) 或 [命令行工具](https://developer.apple.com/download/more/)，可通過以下命令進行安裝：
         + `xcode-select --install`
   + [在Linux上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
   + [在Windows 10上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)
1. 運行以下命令驗證Homebrew是否已安裝： `brew --version`

![荷姆布魯](./assets/development-tools/homebrew.png)

如果使用Homebrew，請 __使用Homebrew安裝__ 說明。 如果 __不__ 使用Homebrew，使用作業系統特定連結安裝工具。

## 安裝Git

[蠢貨](https://git-scm.com/) 是使用的原始碼管理系統 [Adobe雲管理器](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)是發展的需要。

+ 使用Homebrew安裝Git
   1. 開啟終端/命令提示符
   1. 執行命令： `brew install git`
   1. 使用以下命令驗證Git是否已安裝： `git --version`
+ 或者，下載並安裝Git(macOS、Linux或Windows)
   1. [下載並安裝Git](https://git-scm.com/downloads)
   1. 開啟終端/命令提示符
   1. 使用以下命令驗證Git是否已安裝： `git --version`

![蠢貨](./assets/development-tools/git.png)

## 安裝Node.js（和npm）{#node-js}

[節點.js](https://nodejs.org) 是用於處理項目前端資產的JavaScriptAEM運行時環境 __ui.frontend__ 子項目。 Node.js與 [npm](https://www.npmjs.com/)，是事實上的Node.js包管理器，用於管理JavaScript依賴項。

+ 使用Homebrew安裝Node.js
   1. 開啟終端/命令提示符
   1. 執行命令： `brew install node`
   1. 使用以下命令驗證Node.js是否已安裝： `node -v`
   1. 使用以下命令驗證npm是否已安裝： `npm -v`
+ 或者，下載並安裝Node.js(macOS、Linux或Windows)
   1. [下載並安裝Node.js](https://nodejs.org/en/download/)
   1. 開啟終端/命令提示符
   1. 使用以下命令驗證Node.js是否已安裝： `node -v`
   1. 使用以下命令驗證npm是否已安裝： `npm -v`

![Node.js 和 npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[項AEM目原型](https://github.com/adobe/aem-project-archetype)基於AEM的項目在生成時安裝Node.js的隔離版本。 保持本地開發系統版本與AEMMaven項目的Repart pom.xml中指定的Node.js和npm版本同步（或接近）是好事。
>
>請參閱此示例 [項AEM目Repactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) 查找Node.js和npm內部版本的位置。

## 安裝Maven

Apache Maven是用於生成從Project Maven Archetype生成的「項目」的開AEM源Java命令AEM行工具。 所有主IDE的[IntelliJ IDEA](https://www.jetbrains.com/idea/)。 [Visual Studio代碼](https://code.visualstudio.com/)。 [日蝕](https://www.eclipse.org/)等) 已整合了Maven支援。

+ 使用Homebrew安裝Maven
   1. 開啟終端/命令提示符
   1. 執行命令： `brew install maven`
   1. 使用以下命令驗證Maven是否已安裝： `mvn -v`
+ 或者，下載並安裝Maven(macOS、Linux或Windows)
   1. [下載Maven](https://maven.apache.org/download.cgi)
   1. [安裝Maven](https://maven.apache.org/install.html)
   1. 開啟終端/命令提示符
   1. 使用以下命令驗證Maven是否已安裝： `mvn -v`

![馬文](./assets/development-tools/maven.png)

## 設定Adobe I/OCLI{#aio-cli}

的 [Adobe I/OCLI](https://github.com/adobe/aio-cli)或 `aio`，提供對各種Adobe服務(包括 [雲管理器](https://github.com/adobe/aio-cli-plugin-cloudmanager) 和 [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute)。 Adobe I/OCLI在as a Cloud Service開發中起著不可或缺的作AEM用，因為它使開發人員能夠：

+ 尾日誌作為AEMCloud Services服務
+ 通過CLI管理Cloud Manager管道

### 安裝Adobe I/OCLI

1. 確保 [已安裝Node.js](#node-js) 因為Adobe I/OCLI是npm模組
   + 運行 `node --version` 確認
1. 執行 `npm install -g @adobe/aio-cli` 安裝 `aio` npm模組全局

### 設定Adobe I/OCLI雲管理器插件{#aio-cloud-manager}

Adobe I/O雲管理器插件允許aio CLI通過Adobe雲管理器 `aio cloudmanager` 的子菜單。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 安裝 [aio Cloud Manager插件](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

### 設定Adobe I/OCLIAsset compute插件{#aio-asset-compute}

Adobe I/O雲管理器插件允許aio CLI通過 `aio asset-compute` 的子菜單。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-asset-compute` 安裝 [aioAsset compute插件](https://github.com/adobe/aio-cli-plugin-asset-compute)。

### 設定Adobe I/OCLI身份驗證

為使Adobe I/OCLI與Cloud Manager通信， [必須在Adobe I/O控制台中建立Cloud Manager整合](https://github.com/adobe/aio-cli-plugin-cloudmanager)，必須獲得憑據才能成功進行身份驗證。

1. 登錄到 [console.adobe.io](https://console.adobe.io)
1. 確保包含要連接到的Cloud Manager產品的組織在Adobe組織切換器中處於活動狀態
1. 新建或開啟現有 [Adobe I/O計畫](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O控制台程式只是根據您希望如何管理整合而將整合、建立或使用和現有程式組織起來
   + 如果建立新項目，請在出現提示時選擇「空項目」(與&quot;從模板建立&quot;)
   + Adobe I/O控制台程式與Cloud Manager程式是不同的概念
1. 建立與「開發人員 — Cloud Service」配置檔案的新Cloud Manager API整合
1. 獲取需要填充Adobe I/OCLI的服務帳戶(JWT)憑據 [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 載入 `config.json` 檔案到Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. 載入 `private.key` 檔案到Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

開始 [執行命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) 通過Adobe I/OCLI。

## 設定開發IDE

開AEM發主要由Java和Front-end（JavaScript、CSS等）開發和XML管理組成。 以下是最常用的開發IDEAEM。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ 是用於Java開發的功能強大的IDE。 IntelliJ IDEA有兩種版本，一種是免費的社區版，另一種是商業版（付費）最終版。 免費社區版本足以AEM發展，但最終 [擴展其功能集](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

+ [下載IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下載回購工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### MicrosoftVisual Studio代碼

__[Visual Studio代碼](https://code.visualstudio.com/)__ （VS代碼）是面向前端開發人員的免費開源工具。 可以設定Visual Studio代碼，以便借助Adobe工AEM具將內容同步與整合， __[回購](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__。

Visual Studio Code是前端開發人員的理想選擇，主要是建立前端代碼；JavaScript、CSS和HTML。 而VS代碼通過 [擴展](https://code.visualstudio.com/docs/java/java-tutorial)但是，它可能缺少一些由更特定於Java的高級功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下載Visual Studio代碼](https://code.visualstudio.com/Download)
+ [下載回購工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下載Aemed VS代碼擴展](https://aemfed.io/)
+ [下載同AEM步VS代碼擴展](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### 日蝕

__[Eclipse IDE](https://www.eclipse.org/ide/)__ 是用於Java開發的常用IDE，支援  __[開發AEM人員工具](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)__ 通過Adobe提供的插件，提供了用於創作和將JCR內容與本地實例同步的IDEAEM GUI。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下載Eclipse](https://www.eclipse.org/ide/)
+ [下載Eclipse開發工具](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html?lang=en)
