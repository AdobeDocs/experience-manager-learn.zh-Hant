---
title: 設定AEMas a Cloud Service開發的開發工具
description: 設定本機開發機器，其中包含針對AEM進行本機開發所需的所有基準線工具。
feature: Developer Tools
version: Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1483'
ht-degree: 8%

---

# 設定開發工具 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="設定開發工具"
>abstract="展開 Adobe Experience Manager (AEM) 的開發作業前，開發人員的電腦上需先安裝與設定一套簡單的開發工具。這些工具包括 Java、Maven、Adobe I/O CLI、開發 IDE 等。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=zh-Hant" text="開發準則"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=zh-Hant" text="開發基本概念"

展開 Adobe Experience Manager (AEM) 的開發作業前，開發人員的電腦上需先安裝與設定一套簡單的開發工具。這些工具可支援AEM專案的開發與建置。

請注意 `~` 會用作使用者目錄的速記。 在Windows中，這相當於 `%HOMEPATH%`.

## 安裝Java

Experience Manager是一種Java應用程式，因此需要Java SDK來支援開發和AEMas a Cloud ServiceSDK。

1. [下載並安裝最新版的Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14444)
1. 執行下列命令，確認已安裝Oracle Java 11 SDK：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/development-tools/java.png)

## 安裝Homebrew

_使用Homebrew為選填，但建議使用。_

Homebrew是適用於macOS、Windows和Linux的開放原始碼套件管理程式。 所有的支援工具都可以單獨安裝，Homebrew提供了便捷的方式來安裝和更新Experience Manager開發所需的各種開發工具。

1. 開啟您的終端機
1. 執行命令以檢查是否已安裝Homebrew： `brew --version`.
1. 如果未安裝Homebrew，請安裝Homebrew

>[!BEGINTABS]

>[!TAB macOS]

[macOS上的Homebrew](https://brew.sh/) 需要 [Xcode](https://apps.apple.com/us/app/xcode/id497799835) 或 [命令列工具](https://developer.apple.com/download/more/)，可透過命令安裝：

```shell
$ xcode-select --install
```

>[!TAB Windows]

[在Windows 10上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!TAB Linux]

[在Linux上安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. 執行下列命令，確認是否已安裝Homebrew： `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

如果您使用Homebrew，請遵循 __使用Homebrew安裝__ 以下各節中的指示。 如果您是 __非__ 使用Homebrew，使用作業系統特定的連結來安裝工具。

## 安裝Git

[Git](https://git-scm.com/) 原始檔控制管理系統是由 [AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html)因此，是開發時的必要專案。

>[!BEGINTABS]

>[!TAB 使用Homebrew安裝Git]

1. 開啟您的終端機/命令提示字元
1. 執行命令： `$ brew install git`
1. 使用命令確認已安裝Git： `$ git --version`

>[!TAB 下載並安裝Git]

1. [下載並安裝Git](https://git-scm.com/downloads)
1. 開啟您的終端機/命令提示字元
1. 使用命令確認已安裝Git： `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## 安裝Node.js （和npm）{#node-js}

[Node.js](https://nodejs.org) 是一個用於處理AEM專案的前端資產的JavaScript執行階段環境 __ui.frontend__ 子專案。 Node.js的發佈方式 [npm](https://www.npmjs.com/)，是實際的Node.js套件管理員，用來管理JavaScript相依性。

>[!BEGINTABS]

>[!TAB 使用Homebrew安裝Node.js]

1. 開啟您的終端機/命令提示字元
1. 執行命令： `$ brew install node`
1. 使用下列命令確認已安裝Node.js： `$ node -v`
1. 使用下列命令驗證是否已安裝npm： `$ npm -v`

>[!TAB 下載並安裝Node.js]

1. [下載並安裝Node.js](https://nodejs.org/en/download/)
2. 開啟您的終端機/命令提示字元
3. 使用下列命令確認已安裝Node.js： `$ node -v`
4. 使用下列命令驗證是否已安裝npm： `$ npm -v`

>[!ENDTABS]

![Node.js 和 npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM專案原型](https://github.com/adobe/aem-project-archetype)-based AEM專案會在建置時安裝隔離版本的Node.js。 最好讓本機開發系統的版本與在AEM Maven專案的Reactor pom.xml中指定的Node.js和npm版本保持同步（或接近）。
>
>請參閱此範例 [AEM專案反應器pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118) 用於找到Node.js和npm組建版本的位置。

## 安裝Maven

Apache Maven是開放原始碼Java命令列工具，用於建置從AEM專案Maven原型產生的AEM專案。 所有主要IDE ([IntelliJ IDEA](https://www.jetbrains.com/idea/)， [Visual Studio Code](https://code.visualstudio.com/)， [Eclipse](https://www.eclipse.org/)、等) 已整合Maven支援。


>[!BEGINTABS]

>[!TAB 使用Homebrew安裝Maven]

1. 開啟您的終端機/命令提示字元
1. 執行命令： `$ brew install maven`
1. 使用命令確認已安裝Maven： `$ mvn -v`

>[!TAB 下載並安裝Maven]

1. [下載Maven](https://maven.apache.org/download.cgi)
1. [安裝Maven](https://maven.apache.org/install.html)
1. 開啟您的終端機/命令提示字元
1. 使用命令確認已安裝Maven： `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## 設定Adobe I/OCLI{#aio-cli}

此 [ADOBE I/OCLI](https://github.com/adobe/aio-cli)，或 `aio`，提供各種Adobe服務的命令列存取權，包括 [Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager) 和 [asset compute](https://github.com/adobe/aio-cli-plugin-asset-compute). Adobe I/OCLI在AEMas a Cloud Service的開發中起著不可或缺的作用，因為它讓開發人員能夠：

+ AEM as aCloud Service服務的尾部記錄
+ 從CLI管理Cloud Manager管道
+ 部署至 [AEM快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html)

### 安裝Adobe I/OCLI

1. 確定 [Node.js已安裝](#node-js) 因為Adobe I/OCLI是npm模組
   + 執行 `node --version` 確認
1. 執行 `npm install -g @adobe/aio-cli` 安裝 `aio` npm模組全域

### 設定Adobe I/OCLI Cloud Manager外掛程式{#aio-cloud-manager}

Adobe I/OAdobe Cloud Manager外掛程式允許aio CLI透過 `aio cloudmanager` 命令。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-cloudmanager` 安裝 [aio Cloud Manager外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager).

#### 設定Adobe I/OCLI驗證

為了讓Adobe I/OCLI與Cloud Manager通訊， [必須在Adobe I/O控制檯中建立Cloud Manager整合](https://github.com/adobe/aio-cli-plugin-cloudmanager)，且必須取得認證才能成功驗證。

1. 登入 [console.adobe.io](https://console.adobe.io)
1. 確保包含要連線的Cloud Manager產品的組織在Adobe組織切換器中處於活動狀態
1. 建立新的或開啟現有的 [Adobe I/O計畫](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O控制檯程式只是根據您想要管理整合的方式，由整合、建立或使用以及現有程式組成的組織群組
   + 如果建立新專案，則在出現提示時選取「空白專案」（與「從範本建立」的比較）
   + Adobe I/O控制檯程式與Cloud Manager程式的概念不同
1. 使用「開發人員 — Cloud Service」設定檔建立新的Cloud Manager API整合
1. 取得服務帳戶(JWT)憑證需要填入Adobe I/OCLI [config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)
1. 載入 `config.json` 檔案放入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager PATH_TO_CONFIG_JSON_FILE --file --json`
1. 載入 `private.key` 檔案放入Adobe I/OCLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager.private_key PATH_TO_PRIVATE_KEY_FILE --file`

開始 [正在執行命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands) 適用於Cloud Manager，透過Adobe I/OCLI。

### 設定AEM快速開發環境外掛程式{#rde}

AEM快速開發環境外掛程式可讓aio CLI與AEMas a Cloud Service互動 [快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) 透過 `aio aem:rde` 命令。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-aem-rde` 安裝 [AEM快速開發環境外掛程式](https://github.com/adobe/aio-cli-plugin-aem-rde).

### 設定Adobe I/OCLIAsset compute外掛程式{#aio-asset-compute}

Adobe I/OCloud Manager外掛程式可讓aio CLI透過產生並執行Asset compute背景工作 `aio asset-compute` 命令。

1. 執行 `aio plugins:install @adobe/aio-cli-plugin-asset-compute` 安裝 [aioAsset compute外掛程式](https://github.com/adobe/aio-cli-plugin-asset-compute).

## 設定開發IDE

AEM開發主要包括了Java和前端（JavaScript、CSS等）開發以及XML管理。 以下是AEM開發中最常用的IDE。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__ 是用於Java開發的強大IDE。 IntelliJ IDEA提供兩種口味：免費社群版和商業（付費） Ultimate版。 免費的社群版本已足以進行AEM開發，但旗艦版仍可使用 [擴充其功能集](https://www.jetbrains.com/idea/download).

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [下載IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)是適用於前端開發人員的免費開放原始碼工具。 Visual Studio Code可設定為在Adobe工具的協助下與AEM整合內容同步。 __[存放庫](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__.

Visual Studio Code是前端開發人員建立前端計畫碼、JavaScript、CSS和HTML的理想選擇。 VS Code透過支援Java [擴充功能](https://code.visualstudio.com/docs/java/java-tutorial)，可能缺乏更具Java特定性的部分進階功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下載Visual Studio Code](https://code.visualstudio.com/Download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下載預設的VS程式碼擴充功能](https://aemfed.io/)
+ [下載AEM Sync VS程式碼擴充功能](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__ 是Java開發的熱門IDE，並支援  __[AEM Developer Tools](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)__ Adobe此外掛程式，提供IDE內部的GUI，以供撰寫及將JCR內容與本機AEM執行個體同步。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下載Eclipse](https://www.eclipse.org/ide/)
+ [下載Eclipse開發工具](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)
