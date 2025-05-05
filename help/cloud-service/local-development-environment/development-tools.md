---
title: 設定AEM as a Cloud Service開發的開發工具
description: 設定本機開發機器，並具備在本機針對AEM進行開發所需的所有基準線工具。
feature: Developer Tools
version: Experience Manager as a Cloud Service
jira: KT-4267
thumbnail: 25907.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
exl-id: 6fb3199a-02c9-48bc-a6fa-1f767cfd2f2a
duration: 3508
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '1302'
ht-degree: 6%

---

# 設定開發工具 {#set-up-development-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_devtools"
>title="設定開發工具"
>abstract="展開 Adobe Experience Manager (AEM) 的開發作業前，開發人員的電腦上需先安裝與設定一套簡單的開發工具。這些工具包括 Java、Maven、Adobe I/O CLI、開發 IDE 等。"
>additional-url="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines" text="開發準則"
>additional-url="https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk" text="開發基本概念"

展開 Adobe Experience Manager (AEM) 的開發作業前，開發人員的電腦上需先安裝與設定一套簡單的開發工具。這些工具可支援AEM專案的開發與建置。

請注意，`~`是用作使用者目錄的速記。 在Windows中，這相當於`%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK來支援開發和AEM as a Cloud Service SDK。

1. [下載並安裝最新版的Java 11 SDK](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14&amp;p.limit=144)
1. 透過執行命令，確認已安裝Oracle Java 11 SDK：

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

_使用Homebrew是選用的，但建議使用。_

Homebrew是適用於macOS、Windows和Linux的開放原始碼套件管理程式。 所有支援工具均可單獨安裝，Homebrew提供便利的方式來安裝和更新Experience Manager開發所需的各種開發工具。

1. 開啟您的終端機
1. 執行命令，檢查是否已安裝Homebrew： `brew --version`。
1. 如果未安裝Homebrew，請安裝Homebrew

>[!BEGINTABS]

>[!TAB macOS]

macOS[&#128279;](https://brew.sh/)上的Homebrew需要[Xcode](https://apps.apple.com/us/app/xcode/id497799835)或[命令列工具](https://developer.apple.com/download/more/)，可透過命令安裝：

```shell
$ xcode-select --install
```

>[!TAB Windows]

[在Windows 10](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)上安裝Homebrew

>[!TAB Linux]

在Linux上[安裝Homebrew](https://docs.brew.sh/Installation#linux-or-windows-10-subsystem-for-linux)

>[!ENDTABS]

1. 執行命令以驗證Homebrew是否已安裝： `brew --version`

![Homebrew](./assets/development-tools/homebrew.png)

如果您使用Homebrew，請遵循以下章節中的&#x200B;__使用Homebrew安裝__&#x200B;指示。 如果您&#x200B;__不是__&#x200B;使用Homebrew，請使用作業系統特定的連結來安裝工具。

## 安裝Git

[Git](https://git-scm.com/)是[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/source-code-repository.html?lang=zh-Hant)使用的原始檔控制管理系統，因此是開發所需。

>[!BEGINTABS]

>[!TAB 使用Homebrew安裝Git]

1. 開啟您的終端機/命令提示字元
1. 執行命令： `$ brew install git`
1. 使用下列命令驗證Git是否已安裝： `$ git --version`

>[!TAB 下載並安裝Git]

1. [下載並安裝Git](https://git-scm.com/downloads)
1. 開啟您的終端機/命令提示字元
1. 使用下列命令驗證Git是否已安裝： `$ git --version`

>[!ENDTABS]

![Git](./assets/development-tools/git.png)

## 安裝Node.js （和npm）{#node-js}

[Node.js](https://nodejs.org)是JavaScript執行階段環境，用來處理AEM專案的&#x200B;__ui.frontend__&#x200B;子專案的前端資產。 Node.js與[npm](https://www.npmjs.com/)一同發佈，是實際的Node.js封裝管理員，用於管理JavaScript相依性。

>[!BEGINTABS]

>[!TAB 使用Homebrew安裝Node.js]

1. 開啟您的終端機/命令提示字元
1. 執行命令： `$ brew install node`
1. 使用下列命令，驗證是否已安裝Node.js： `$ node -v`
1. 使用下列命令驗證是否已安裝npm： `$ npm -v`

>[!TAB 下載並安裝Node.js]

1. [下載並安裝Node.js](https://nodejs.org/en/download/)
2. 開啟您的終端機/命令提示字元
3. 使用下列命令，驗證是否已安裝Node.js： `$ node -v`
4. 使用下列命令驗證是否已安裝npm： `$ npm -v`

>[!ENDTABS]

![Node.js和npm](./assets/development-tools/nodejs-and-npm.png)

>[!TIP]
>
>[AEM專案原型](https://github.com/adobe/aem-project-archetype)型AEM專案會在建置時安裝隔離版本的Node.js。 最好讓本機開發系統的版本與在AEM Maven專案的Reactor pom.xml中指定的Node.js和npm版本保持同步（或接近）。
>
>請參閱此範例[AEM Project Reactor pom.xml](https://github.com/adobe/aem-guides-wknd/blob/9ac94f3f40c978a53ec88fae79fbc17dd2db72f2/pom.xml#L117-L118)，瞭解在何處找到Node.js和npm組建版本。

## 安裝Maven

Apache Maven是開放原始碼Java命令列工具，用於建置從AEM專案Maven原型產生的AEM專案。 所有主要IDE （[IntelliJ IDEA](https://www.jetbrains.com/idea/)、[Visual Studio Code](https://code.visualstudio.com/)、[Eclipse](https://www.eclipse.org/)等）均已整合Maven支援。


>[!BEGINTABS]

>[!TAB 使用Homebrew安裝Maven]

1. 開啟您的終端機/命令提示字元
1. 執行命令： `$ brew install maven`
1. 使用下列命令確認是否已安裝Maven： `$ mvn -v`

>[!TAB 下載並安裝Maven]

1. [下載Maven](https://maven.apache.org/download.cgi)
1. [安裝Maven](https://maven.apache.org/install.html)
1. 開啟您的終端機/命令提示字元
1. 使用下列命令確認是否已安裝Maven： `$ mvn -v`

>[!ENDTABS]

![Maven](./assets/development-tools/maven.png)

## 設定Adobe I/O CLI{#aio-cli}

[Adobe I/O CLI](https://github.com/adobe/aio-cli)或`aio`提供各種Adobe服務的命令列存取權，包括[Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)和[Asset Compute](https://github.com/adobe/aio-cli-plugin-asset-compute)。 Adobe I/O CLI在AEM as a Cloud Service的開發中起著不可或缺的作用，因為它讓開發人員能夠：

+ AEM as a Cloud Services的尾部記錄
+ 從CLI管理Cloud Manager管道
+ 部署至[AEM快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=zh-Hant)

### 安裝Adobe I/O CLI

1. 確認已安裝[Node.js](#node-js)，因為Adobe I/O CLI是npm模組
   + 執行`node --version`以確認
1. 執行`npm install -g @adobe/aio-cli`以全域安裝`aio` npm模組

### 設定Adobe I/O CLI Cloud Manager外掛程式{#aio-cloud-manager}

Adobe I/O Cloud Manager外掛程式可讓aio CLI透過`aio cloudmanager`命令與Adobe Cloud Manager互動。

1. 執行`aio plugins:install @adobe/aio-cli-plugin-cloudmanager`以安裝[aio Cloud Manager外掛程式](https://github.com/adobe/aio-cli-plugin-cloudmanager)。

#### 設定Adobe I/O CLI驗證

為了讓Adobe I/O CLI與Cloud Manager通訊，必須在Adobe I/O主控台[&#128279;](https://github.com/adobe/aio-cli-plugin-cloudmanager)中建立Cloud Manager整合，且必須取得認證才能成功驗證。

1. 登入[console.adobe.io](https://console.adobe.io)
1. 確保包含要連線之Cloud Manager產品的組織在Adobe組織切換器中處於活動狀態
1. 建立新的或開啟現有的[Adobe I/O程式](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)
   + Adobe I/O Console專案不過是依您想要管理整合的方式而組成的組織性整合群組、建立或使用以及現有的專案。
   + 如果建立新專案，則在出現提示時選取「空白專案」（與「從範本建立」的比較）
   + Adobe I/O Console程式與Cloud Manager程式有不同的概念
1. 建立新的Cloud Manager API整合
   + 選取「Oauth伺服器對伺服器」認證型別。
   + 選取「部署管理員 — Cloud Service」產品設定檔。
   + 儲存已設定的API
1. 取得認證時，需要開啟新建立的「OAuth伺服器對伺服器」認證，然後從右上角的動作列選取「下載JSON」，以填入Adobe I/O CLI的[config.json](https://github.com/adobe/aio-cli-plugin-cloudmanager#authentication)。
1. 開啟下載的JSON檔案，並將所有金鑰重新命名為小寫。 例如，`CLIENT_ID`會變成`client_id`。
1. 將`config.json`檔案載入Adobe I/O CLI
   + `$ aio config:set ims.contexts.aio-cli-plugin-cloudmanager /path/to/downloaded/json --file --json`

透過Adobe I/O CLI開始[執行Cloud Manager的命令](https://github.com/adobe/aio-cli-plugin-cloudmanager#commands)。

### 設定AEM快速開發環境外掛程式{#rde}

AEM快速開發環境外掛程式可讓aio CLI透過`aio aem:rde`命令與AEM as a Cloud Service [快速開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html?lang=zh-Hant)互動。

1. 執行`aio plugins:install @adobe/aio-cli-plugin-aem-rde`以安裝[AEM快速開發環境外掛程式](https://github.com/adobe/aio-cli-plugin-aem-rde)。

### 設定Adobe I/O CLI Asset Compute外掛程式{#aio-asset-compute}

Adobe I/O Cloud Manager外掛程式可讓aio CLI透過`aio asset-compute`命令產生及執行Asset Compute Worker。

1. 執行`aio plugins:install @adobe/aio-cli-plugin-asset-compute`以安裝[aio Asset Compute外掛程式](https://github.com/adobe/aio-cli-plugin-asset-compute)。

## 設定開發IDE

AEM開發主要包括了Java和前端(JavaScript、CSS等)開發及XML管理。 以下是AEM開發中最常用的IDE。

### IntelliJ IDEA

__[IntelliJ IDEA](https://www.jetbrains.com/idea/)__&#x200B;是適用於Java開發的強大IDE。 IntelliJ IDEA提供兩種風格：免費社群版本和商業（付費）Ultimate版本。 免費社群版本已足夠AEM開發，但Ultimate [已擴充其功能集](https://www.jetbrains.com/idea/download)。

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

+ [下載IntelliJ IDEA](https://www.jetbrains.com/idea/download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

### Microsoft Visual Studio Code

__[Visual Studio Code](https://code.visualstudio.com/)__ (VS Code)是供前端開發人員使用的免費開放原始碼工具。 Visual Studio Code可設定為在Adobe工具&#x200B;__[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)__&#x200B;的協助下，將內容同步處理與AEM整合。

Visual Studio Code是前端開發人員建立前端程式碼、JavaScript、CSS和HTML的理想選擇。 雖然VS Code透過[擴充功能](https://code.visualstudio.com/docs/java/java-tutorial)提供Java支援，但可能缺乏更具Java特定性的部分進階功能。

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

+ [下載Visual Studio程式碼](https://code.visualstudio.com/Download)
+ [下載存放庫工具](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)
+ [下載AEM Sync VS副檔名](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)

### Eclipse

__[Eclipse IDE](https://www.eclipse.org/ide/)__&#x200B;是Java開發的熱門IDE，可支援Adobe提供的&#x200B;__[AEM Developer Tools](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=zh-Hant)__&#x200B;外掛程式，提供IDE中的GUI以供撰寫及同步JCR內容與本機AEM執行個體。

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

+ [下載Eclipse](https://www.eclipse.org/ide/)
+ [下載Eclipse開發工具](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html?lang=zh-Hant)
