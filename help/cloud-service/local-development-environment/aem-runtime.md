---
title: 設定本地運AEM行時以AEM進行as a Cloud Service開發
description: 使用as a Cloud ServiceSDK的AEM快速啟動AEMJar設定本地運行時。
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: 3a9615177acb5475d9b2b4ef22907c11e7da2bf7
workflow-type: tm+mt
source-wordcount: '1801'
ht-degree: 2%

---

# 設定本地運AEM行時

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本地運AEM行時"
>abstract="Adobe Experience Manager(AEM)可以使用as a Cloud ServiceSDK的Quickstart AEMJar在本地運行。 這允許開發人員在將自定義代碼、配置和內容提交到原始碼控制並將其部署到as a Cloud Service環境之前，將其部署到並testAEM自定義代碼、配置和內容。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下載AEMas a Cloud ServiceSDK"

Adobe Experience Manager(AEM)可以使用as a Cloud ServiceSDK的Quickstart AEMJar在本地運行。 這允許開發人員在將自定義代碼、配置和內容提交到原始碼控制並將其部署到as a Cloud Service環境之前，將其部署到並testAEM自定義代碼、配置和內容。

請注意 `~` 用作用戶目錄的簡寫。 在Windows中，這相當於 `%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK支援開發工具。

1. [下載並安裝最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;order=%40jcr%3內容%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 通過運行以下命令驗證Java 11 SDK是否已安裝：
   + Windows:`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## 下載AEMas a Cloud ServiceSDK

as a Cloud ServiceAEMSDK(或AEMSDK)包含用於運行AEM Author和本地發佈以供開發的Quickstart Jar以及Dispatcher Tools的相容版本。

1. 登錄到 [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) 你的Adobe ID
   + 請注意，您的Adobe組織 __必須__ 已設定AEMas a Cloud Service以下載AEMas a Cloud ServiceSDK。
1. 導航到 __AEMas a Cloud Service__ 頁籤
1. 排序依據 __發佈日期__ 在 __降序__ 訂單
1. 按一下最新 __SDKAEM__ 結果行
1. 查看並接受EULA，並點擊 __下載__ 按鈕

## 從AEMSDK zip解壓快速啟動Jar

1. 解壓縮下載的 `aem-sdk-XXX.zip` 檔案

## 設定本地AEM作者服務{#set-up-local-aem-author-service}

本地AEM作者服務為開發商提供本地體驗數字營銷者/內容作者將分享以建立和管理內容。  AEM Author Service既是創作環境，也是預覽環境，它允許對功能開發執行大多數驗證，因此它是本地開發流程的一個重要元素。

1. 建立資料夾 `~/aem-sdk/author`
1. 複製 __快速啟動JAR__ 檔案  `~/aem-sdk/author` 將其更名為 `aem-author-p4502.jar`
1. 通過從命令行執行以下命令來啟動本地AEM作者服務：
   + `java -jar aem-author-p4502.jar`
      + 將管理員密碼提供為 `admin`。 任何管理員密碼都是可接受的，但建議使用本地開發的預設密碼來減少重新配置的需要。

   你 *不能* 以「Cloud Service快AEM速啟動Jar」的身份啟動 [按兩下](#troubleshooting-double-click)。
1. 訪問本地AEM作者服務，地址為 [http://localhost:4502](http://localhost:4502) 在Web瀏覽器中

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 設定本地AEM發佈服務

本地AEM發佈服務為開發人員提供本地體驗，最終用戶AEM將擁有本地體驗，例如瀏覽上門的網AEM站。 本地AEM發佈服務與SDK整合AEM時非常重要 [調度程式工具](./dispatcher-tools.md) 並允許開發人員抽煙test並微調最終面向最終用戶的體驗。

1. 建立資料夾 `~/aem-sdk/publish`
1. 複製 __快速啟動JAR__ 檔案  `~/aem-sdk/publish` 將其更名為 `aem-publish-p4503.jar`
1. 通過從命令行執行以下命令來啟動本地AEM發佈服務：
   + `java -jar aem-publish-p4503.jar`
      + 將管理員密碼提供為 `admin`。 任何管理員密碼都是可接受的，但建議使用本地開發的預設密碼來減少重新配置的需要。

   你 *不能* 以「Cloud Service快AEM速啟動Jar」的身份啟動 [按兩下](#troubleshooting-double-click)。
1. 訪問本地AEM發佈服務，地址為 [http://localhost:4503](http://localhost:4503) 在Web瀏覽器中

窗口：

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 在預發行模AEM式下設定本地服務

可以AEM在中啟動本地運行時 [預釋放模式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html) 允許開發人員根據AEMas a Cloud Service的下一版功能進行構建。 通過 `-r prerelease` 本地運行時AEM的第一個啟動的參數。 這可以與本地AEM Author和AEM Publish服務一起使用。

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

## 模擬內容分發 {#content-distribution}

在真正的Cloud Service環境中，使用 [Sling內容分發](https://sling.apache.org/documentation/bundles/content-distribution.html) 和Adobe管道。 的 [Adobe管線](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) 是僅在雲環境中可用的孤立微服務。

在開發過程中，最好使用本地作者和發佈服務來模擬內容的分發。 這可以通過啟用舊式複製代理來實現。

>[!NOTE]
>
> 複製代理僅可在本地Quickstart JAR中使用，並僅提供內容分發的模擬。

1. 登錄到 **作者** 服務並導航 [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html)。
1. 按一下 **預設代理（發佈）** 開啟預設複製代理。
1. 按一下 **編輯** 開啟代理的配置。
1. 在 **設定** 頁籤，更新以下欄位：

   + **已啟用**  — 檢查真
   + **代理用戶ID**  — 將此欄位留空

   ![複製代理配置 — 設定](assets/aem-runtime/settings-config.png)

1. 在 **運輸** 頁籤，更新以下欄位：

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **用戶** - `admin`
   + **密碼** - `admin`

   ![複製代理配置 — 傳輸](assets/aem-runtime/transport-config.png)

1. 按一下 **確定** 保存配置並啟用 **預設** 複製代理。
1. 現在，您可以對「作者」服務上的內容進行更改，並將其發佈到「發佈」服務。

![發佈頁面](assets/aem-runtime/publish-page-changes.png)

## 快速啟動Jar啟動模式

快速啟動罐的命名， `aem-<tier>_<environment>-p<port number>.jar` 指定啟動方式。 一旦AEM在特定層、作者或發佈中啟動，就不能將其更改為備用層。 為此， `crx-Quickstart` 必須刪除在第一次運行期間生成的資料夾，並且必須再次運行Quickstart Jar。 環境和埠可以更改，但需要停止/啟動本地實AEM例。

不斷變化的環境， `dev`。 `stage` 和 `prod`，對於開發人員來說非常有用，可確保由正確定義和解決特定於環境的配AEM置。 建議主要在預設情況下進行本地開發 `dev` 環境運行模式。

可用排列如下：

| 快速啟動Jar檔案名 | 模式說明 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | 作為作者在埠4502上以開發運行模式運行 |
| `aem-author_dev-p4502.jar` | 作為Author在埠4502上以開發運行模式運行(與 `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | 在埠4502的「暫存」運行模式中作為作者 |
| `aem-author_prod-p4502.jar` | 作為作者在埠4502上的生產運行模式 |
| `aem-publish-p4503.jar` | 在埠4503上以開發運行模式發佈 |
| `aem-publish_dev-p4503.jar` | 在埠4503上以開發運行模式發佈(與 `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | 在埠4503上以暫存運行模式發佈 |
| `aem-publish_prod-p4503.jar` | 在埠4503上以生產運行模式發佈 |

請注意，埠號可以是本地開發電腦上的任何可用埠，但是按慣例：

+ 埠 __4502__ 用於 __本地AEM作者服務__
+ 埠 __4503__ 用於 __本地AEM發佈服務__

更改這些設定可能需要對AEMSDK配置進行調整

## 停止本地運AEM行時

為了停止本地運行AEM時（AEM Author或Publish服務），開啟用於啟動運行時的命令行窗AEM口，然後點擊 `Ctrl-C`。 等待關AEM閉。 關閉過程完成後，命令行提示將可用。

## 可選本AEM地運行時設定任務

+ __OSGi配置環境變數和秘密變數__ 是 [為本地運行AEM時特別設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)而不是使用aio CLI管理它們。

## 何時更新快速啟動Jar

至少每AEM月在每月最後一個星期四(即as a Cloud Service的「功能發佈」的發佈節目)上更新SDKAEM，或在之後不久更新SDK。

>[!WARNING]
>
> 將Quickstart Jar更新為新版本需要替換整個本地開發環境，從而導致本地儲存庫中的所有代碼、配置和內AEM容丟失。 確保任何不應銷毀的代碼、配置或內容安全提交到Git，或從本地實例導出為AEM包AEM。

### 如何避免升級SDK時的內AEM容丟失

升級AEMSDK將有效地建立全新的運行AEM時，包括新的儲存庫，這意味著對先前SDK的儲存庫所做的AEM任何更改都將丟失。 以下是可行的策略，可幫助在SDK升級之AEM間保留內容，並可以單獨或協調使用：

1. 建立專用於包含「示例」內容的內容包，以幫助開發，並以Git維護。 應通過SDK升級保AEM留的任何內容將保留到此包中，並在升級SDK後重新AEM部署。
1. 使用 [橡樹升級](https://jackrabbit.apache.org/oak/docs/migration.html) 和 `includepaths` 指令，將內容從以前的AEMSDK儲存庫複製到新AEM的SDK儲存庫。
1. 使用先前AEMSDK上的包管理器和內容包備AEM份任何內容，並在新SDK上重新安AEM裝它們。

請記住，使用以上方法在SDK升級之AEM間維護代碼表示開發反模式。 非一次性代碼應源於開發IDE，並通過部署流AEM入SDK。

## 疑難排解

### 按兩下Quickstart Jar檔案將導致錯誤{#troubleshooting-double-click}

按兩下快速啟動Jar啟動時，將顯示錯誤模式，阻止本AEM地啟動。

![疑難解答 — 按兩下Quickstart Jar檔案](./assets/aem-runtime/troubleshooting__double-click.png)

這是因為AEMas a Cloud Service快速啟動Jar不支援按兩下快速啟動Jar以在本地AEM啟動。 而必須從該命令行運行Jar檔案。

要啟動AEM作者服務， `cd` 進入包含快速啟動Jar的目錄並執行命令：

`$ java -jar aem-author-p4502.jar`

或者，啟動AEM發佈服務， `cd` 進入包含快速啟動Jar的目錄並執行命令：

`$ java -jar aem-publish-p4503.jar`

### 從命令行啟動快速啟動Jar會立即中止{#troubleshooting-java-8}

從命令行啟動Quickstart Jar時，進程會立即中止，AEM服務不會啟動，出現以下錯誤：

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

這是因為AEMas a Cloud Service需要Java SDK 11，而您正在運行的版本很可能是Java 8。 要解決此問題，請下載並安裝 [OracleJava SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3內容%2Fmetadata%2Fdc%3SoftwareType&amp;1_group.propertyvalues.operation=等於&amp;1_group.propertyvalues.0_values=軟體類型%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;order=%40jcr%3內容%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)。
安裝Java SDK 11後，從命令行執行以下命令來驗證它是活動版本。

安裝Java 11 SDK後，通過從命令行運行命令來驗證它是活動版本：

+ Windows: `java -version`
+ macOS/Linux: `java --version`

## 其他資源

+ [下載AEMSDK](https://experience.adobe.com/#/downloads)
+ [Adobe雲管理器](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [Experience Manager調度程式文檔](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)
