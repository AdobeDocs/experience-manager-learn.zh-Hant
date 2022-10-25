---
title: 設定本機AEM執行階段以進行AEMas a Cloud Service開發
description: 使用AEMas a Cloud ServiceSDK的Quickstart Jar設定本機AEM執行階段。
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '1800'
ht-degree: 2%

---

# 設定本機AEM執行階段 {#set-up-local-aem-runtime}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本機AEM執行階段"
>abstract="Adobe Experience Manager(AEM)可透過AEMas a Cloud Service SDK的Quickstart Jar在本機執行。 這可讓開發人員在將自訂程式碼、設定和內容提交至原始碼控制項之前，先進行部署和測試，然後將其部署至AEMas a Cloud Service環境。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEMas a Cloud ServiceSDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下載AEMas a Cloud ServiceSDK"

Adobe Experience Manager(AEM)可透過AEMas a Cloud Service SDK的Quickstart Jar在本機執行。 這可讓開發人員在將自訂程式碼、設定和內容提交至原始碼控制項之前，先進行部署和測試，然後將其部署至AEMas a Cloud Service環境。

請注意 `~` 用作用戶目錄的簡稱。 在Windows中，這等同於 `%HOMEPATH%`.

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK支援開發工具。

1. [下載並安裝最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14)
1. 執行以下命令以確認Java 11 SDK是否已安裝：
   + Windows:`java -version`
   + macOS / Linux : `java --version`

![Java](./assets/aem-runtime/java.png)

## 下載AEMas a Cloud ServiceSDK

AEMas a Cloud ServiceSDK(或AEM SDK)包含用於在本機執行AEM製作和發佈以供開發的Quickstart Jar，以及相容的Dispatcher工具版本。

1. 登入 [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) 與Adobe ID
   + 請注意，您的Adobe組織 __必須__ 布建AEMas a Cloud Service以下載AEMas a Cloud Service SDK。
1. 導覽至 __AEMas a Cloud Service__ 標籤
1. 排序依據 __發佈日期__ in __降序__ 訂購
1. 按一下最新 __AEM SDK__ 結果行
1. 檢閱並接受EULA，然後點選 __下載__ 按鈕

## 從AEM SDK zip解壓縮Quickstart Jar

1. 將下載的郵遞區號解壓縮 `aem-sdk-XXX.zip` 檔案

## 設定本機AEM Author服務{#set-up-local-aem-author-service}

本機AEM作者服務可為開發人員提供本機體驗，供數位行銷人員/內容作者共用，以建立和管理內容。  AEM Author Service的設計目的，是要同時提供製作和預覽環境，讓您能針對其執行大部分的功能開發驗證，成為本機開發程式的重要元素。

1. 建立資料夾 `~/aem-sdk/author`
1. 複製 __快速入門JAR__ 檔案  `~/aem-sdk/author` 將重新命名為 `aem-author-p4502.jar`
1. 從命令列執行下列動作，以啟動本機AEM Author Service:
   + `java -jar aem-author-p4502.jar`
      + 提供管理員密碼作為 `admin`. 任何管理員密碼都是可接受的，但建議使用本機開發的預設密碼，以減少重新設定的需求。

   您 *不能* 將AEM啟動為Cloud Service快速入門Jar [按兩下](#troubleshooting-double-click).
1. 在存取本機AEM Author Service [http://localhost:4502](http://localhost:4502) 在網頁瀏覽器中

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux :

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 設定本機AEM發佈服務

本機AEM發佈服務可為開發人員提供AEM的本機體驗一般使用者將擁有的體驗，例如瀏覽AEM上放置的網站。 本機AEM Publish Service與AEM SDK整合時很重要 [Dispatcher工具](./dispatcher-tools.md) 並可讓開發人員對最終面向使用者的體驗進行煙霧測試和微調。

1. 建立資料夾 `~/aem-sdk/publish`
1. 複製 __快速入門JAR__ 檔案  `~/aem-sdk/publish` 將重新命名為 `aem-publish-p4503.jar`
1. 從命令列執行下列動作，以啟動本機AEM Publish Service:
   + `java -jar aem-publish-p4503.jar`
      + 提供管理員密碼作為 `admin`. 任何管理員密碼都是可接受的，但建議使用本機開發的預設密碼，以減少重新設定的需求。

   您 *不能* 將AEM啟動為Cloud Service快速入門Jar [按兩下](#troubleshooting-double-click).
1. 在存取本機AEM發佈服務 [http://localhost:4503](http://localhost:4503) 在網頁瀏覽器中

窗口：

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux :

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 在發行前模式中設定本機AEM服務

本機AEM執行階段可在 [預發行模式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=zh-Hant) 可讓開發人員根據AEM as a Cloud Service的下一版功能進行建置。 若要啟用發行前版本，請將 `-r prerelease` 本機AEM執行階段首次開始的引數。 這可與本機AEM製作和AEM發佈服務搭配使用。

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

## 模擬內容分發 {#content-distribution}

在真正的Cloud Service環境中，內容會使用 [Sling內容分送](https://sling.apache.org/documentation/bundles/content-distribution.html) 和Adobe管道。 此 [Adobe管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) 是孤立的微服務，僅適用於雲端環境。

在開發期間，可能希望使用本機「作者」和「發佈」服務來模擬內容的分佈。 這可以通過啟用舊版複製代理來實現。

>[!NOTE]
>
> 複製代理僅可在本地Quickstart JAR中使用，並僅提供內容分發的模擬。

1. 登入 **作者** 服務，並導覽至 [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. 按一下 **預設代理（發佈）** 以開啟預設的復寫代理。
1. 按一下 **編輯** 以開啟代理的配置。
1. 在 **設定** 頁簽，更新下列欄位：

   + **已啟用**  — 檢查true
   + **代理用戶Id**  — 將此欄位留空

   ![複製代理配置 — 設定](assets/aem-runtime/settings-config.png)

1. 在 **運輸** 頁簽，更新下列欄位：

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **使用者** - `admin`
   + **密碼** - `admin`

   ![複製代理配置 — 傳輸](assets/aem-runtime/transport-config.png)

1. 按一下 **確定** 儲存設定並啟用 **預設** 復寫代理。
1. 您現在可以變更Author服務上的內容，並發佈至Publish服務。

![發佈頁面](assets/aem-runtime/publish-page-changes.png)

## 快速啟動Jar啟動模式

快速入門罐的命名， `aem-<tier>_<environment>-p<port number>.jar` 指定啟動方式。 AEM在特定層級、製作或發佈中啟動後，就無法變更為替代層級。 若要這麼做， `crx-Quickstart` 必須刪除在首次運行期間生成的資料夾，並且必須重新運行Quickstart Jar。 環境和埠可以變更，但需要停止/啟動本機AEM執行個體。

不斷變化的環境， `dev`, `stage` 和 `prod`，對於開發人員來說非常有用，以確保AEM正確定義並解析環境特定設定。 建議主要針對預設情況進行地方開發 `dev` 環境執行模式。

可用的排列如下：

| 快速啟動Jar檔案名 | 模式說明 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | 在埠4502上以開發運行模式作為作者 |
| `aem-author_dev-p4502.jar` | 在埠4502上以開發執行模式製作(與 `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | 在4502埠的測試執行模式中作為作者 |
| `aem-author_prod-p4502.jar` | 在埠4502的「生產」運行模式中作為作者 |
| `aem-publish-p4503.jar` | 在埠4503上以開發運行模式發佈 |
| `aem-publish_dev-p4503.jar` | 在埠4503上以開發執行模式發佈(與 `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | 在連接埠4503上以測試運行模式發佈 |
| `aem-publish_prod-p4503.jar` | 在埠4503上以生產運行模式發佈 |

請注意，埠號可以是本地開發電腦上的任何可用埠，但按慣例：

+ 埠 __4502__ 用於 __本機AEM作者服務__
+ 埠 __4503__ 用於 __本機AEM發佈服務__

變更這些項目可能需要調整AEM SDK設定

## 停止本機AEM執行階段

若要停止本機AEM執行階段（AEM製作或發佈服務），請開啟用來啟動AEM執行階段的命令列視窗，然後點選 `Ctrl-C`. 等待AEM關閉。 關閉過程完成後，命令行提示可用。

## 可選的本地AEM運行時設定任務

+ __OSGi設定環境變數和機密變數__ 為 [為AEM本機執行階段特別設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)，而不是使用aio CLI管理。

## 何時更新Quickstart Jar

每月至少更新AEM SDK一次，或在每月最後一個星期四(即AEMas a Cloud Service「功能發行」的發行時間)。

>[!WARNING]
>
> 將Quickstart Jar更新到新版本需要替換整個本地開發環境，從而導致本地AEM儲存庫中的所有代碼、配置和內容丟失。 請確定任何不應銷毀的程式碼、設定或內容都能安全提交至Git，或從本機AEM例項匯出為AEM Packages。

### 升級AEM SDK時如何避免內容遺失

升級AEM SDK可有效建立全新AEM執行階段，包括新的存放庫，這表示對先前AEM SDK的存放庫所做的任何變更都會遺失。 以下是可行的策略，以協助在AEM SDK升級之間持續保存內容，且可分散使用或搭配使用：

1. 建立專用於包含「範例」內容的內容套件，以輔助開發，並在Git中維護。 應透過AEM SDK升級而保存的任何內容都會保存在此套件中，並在升級AEM SDK後重新部署。
1. 使用 [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) 和 `includepaths` 指令，將內容從先前的AEM SDK存放庫複製到新的AEM SDK存放庫。
1. 使用AEM Package Manager備份任何內容，並在先前的AEM SDK上使用內容套件，然後在新的AEM SDK上重新安裝。

請記住，使用上述方法在AEM SDK升級之間維護程式碼，表示開發反模式。 非可支配的程式碼應源自您的開發IDE，並透過部署流入AEM SDK。

## 疑難排解

### 按兩下Quickstart Jar檔案會導致錯誤{#troubleshooting-double-click}

按兩下要啟動的Quickstart Jar時，會顯示錯誤強制回應，使AEM無法在本機啟動。

![疑難排解 — 按兩下Quickstart Jar檔案](./assets/aem-runtime/troubleshooting__double-click.png)

這是因為AEMas a Cloud ServiceQuickstart Jar不支援按兩下Quickstart Jar以在本機啟動AEM。 而必須從該命令行運行Jar檔案。

若要啟動AEM Author服務， `cd` 進入包含Quickstart Jar的目錄，然後執行命令：

`$ java -jar aem-author-p4502.jar`

或者，若要啟動AEM發佈服務， `cd` 進入包含Quickstart Jar的目錄，然後執行命令：

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

這是因為AEM as a Cloud Service需要Java SDK 11，而您執行的是不同版本，很可能是Java 8。 若要解決此問題，請下載並安裝 [OracleJava SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;modified by.sort=dest&amp;p.st=dest&amp;p.llep.p.p=14).
安裝Java SDK 11後，從命令列執行下列動作，以確認其為作用中版本。

安裝Java 11 SDK後，從命令列執行命令以確認其為作用中版本：

+ Windows: `java -version`
+ macOS / Linux : `java --version`

## 其他資源

+ [下載AEM SDK](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [Experience ManagerDispatcher檔案](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)
