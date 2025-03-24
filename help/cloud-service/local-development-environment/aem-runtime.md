---
title: 設定適用於AEM as a Cloud Service開發的本機AEM SDK
description: 使用AEM SDK的Quickstart Jar設定本機AEM as a Cloud Service SDK執行階段。
feature: Developer Tools
version: Experience Manager as a Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 7%

---

# 設定本機 AEM SDK {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本機 AEM 執行階段"
>abstract="Adobe Experience Manager (AEM) 可透過 AEM as a Cloud Service  SDK 的 Quickstart Jar 在本機上執行。這讓開發人員在將自訂程式碼、設定和內容送交來源控制項前，即可先行部署和測試，然後再部署至 AEM as a Cloud Service 環境。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下載 AEM as a Cloud Service SDK"

Adobe Experience Manager (AEM) 可透過 AEM as a Cloud Service  SDK 的 Quickstart Jar 在本機上執行。這讓開發人員在將自訂程式碼、設定和內容送交來源控制項前，即可先行部署和測試，然後再部署至 AEM as a Cloud Service 環境。

請注意，`~`是用作使用者目錄的速記。 在Windows中，這相當於`%HOMEPATH%`。

## 安裝Java™

Experience Manager是Java™應用程式，因此需要Oracle Java™ SDK支援開發工具。

1. [下載並安裝最新的Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14&amp;p.limit=144)
1. 執行命令，確認已安裝Oracle Java™ 11 SDK：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## 下載AEM as a Cloud Service SDK

AEM as a Cloud Service SDK (或AEM SDK)包含用於在本機執行AEM製作和發佈以進行開發的Quickstart Jar，以及相容的Dispatcher工具版本。

1. 使用您的Adobe ID登入[https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads)
   + 請注意，您的Adobe組織&#x200B;__必須__&#x200B;已布建給AEM as a Cloud Service，才能下載AEM as a Cloud Service SDK。
1. 導覽至&#x200B;__AEM as a Cloud Service__&#x200B;標籤
1. 以&#x200B;__遞減__&#x200B;的順序依&#x200B;__發佈日期__&#x200B;排序
1. 按一下最新的&#x200B;__AEM SDK__&#x200B;結果列
1. 檢閱並接受EULA，然後點選&#x200B;__下載__&#x200B;按鈕

## 從AEM SDK zip解壓縮Quickstart Jar

1. 解壓縮下載的`aem-sdk-XXX.zip`檔案

## 設定本機AEM作者服務{#set-up-local-aem-author-service}

本機AEM作者服務為開發人員提供本機體驗，數位行銷人員/內容作者將共用該體驗來建立和管理內容。  AEM Author Service在設計上既是製作環境，又是預覽環境，可讓您針對此環境執行功能開發的大部分驗證作業，使其成為本機開發流程的重要元素。

1. 建立資料夾`~/aem-sdk/author`
1. 將&#x200B;__Quickstart JAR__&#x200B;檔案複製到`~/aem-sdk/author`並將它重新命名為`aem-author-p4502.jar`
1. 從命令列執行以下命令，啟動本機AEM Author Service：
   + `java -jar aem-author-p4502.jar`
      + 提供管理員密碼做為`admin`。 可接受任何管理員密碼，但建議使用本機開發的預設值，以減少重新設定的需求。

   您&#x200B;*無法*&#x200B;按兩下](#troubleshooting-double-click)，以Cloud Service Quickstart Jar [形式啟動AEM。
1. 在網頁瀏覽器中存取本機AEM作者服務： [http://localhost:4502](http://localhost:4502)

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## 設定本機AEM發佈服務

本機AEM發佈服務為開發人員提供AEM的本機體驗使用者將擁有的體驗，例如瀏覽存放在AEM上的網站。 本機AEM Publish Service非常重要，因為它與AEM SDK的[Dispatcher工具](./dispatcher-tools.md)整合，可讓開發人員進行測試並微調面向最終使用者的體驗。

1. 建立資料夾`~/aem-sdk/publish`
1. 將&#x200B;__Quickstart JAR__&#x200B;檔案複製到`~/aem-sdk/publish`並將它重新命名為`aem-publish-p4503.jar`
1. 從命令列執行以下命令，啟動本機AEM Publish Service：
   + `java -jar aem-publish-p4503.jar`
      + 提供管理員密碼做為`admin`。 可接受任何管理員密碼，但建議使用本機開發的預設值，以減少重新設定的需求。

   您&#x200B;*無法*&#x200B;按兩下](#troubleshooting-double-click)，以Cloud Service Quickstart Jar [形式啟動AEM。
1. 在網頁瀏覽器中存取本機AEM發佈服務，網址為[http://localhost:4503](http://localhost:4503)

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## 在發行前模式下設定本機AEM服務

本機AEM執行階段可以在[發行前模式](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=zh-Hant)中啟動，讓開發人員可以針對AEM as a Cloud Service的下一個發行版本功能進行建置。 透過在本機AEM執行階段的第一個開始傳遞`-r prerelease`引數來啟用發行前版本。 這可同時用於本機AEM Author和AEM Publish服務。


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux®]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## 模擬內容發佈 {#content-distribution}

在真正的Cloud Service環境中，內容是使用[Sling內容發佈](https://sling.apache.org/documentation/bundles/content-distribution.html)和Adobe管道從作者服務發佈到發佈服務。 [Adobe管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution)是只能在雲端環境中使用的隔離微服務。

在開發期間，可能最好使用本機Author和Publish服務來模擬內容的分佈。 這可透過啟用舊版復寫代理程式來達成。

>[!NOTE]
>
> 復寫代理程式僅可用於本機Quickstart JAR，且僅提供內容發佈的模擬。

1. 登入&#x200B;**作者**&#x200B;服務並導覽至[http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html)。
1. 按一下&#x200B;**預設代理程式（發佈）**&#x200B;以開啟預設復寫代理程式。
1. 按一下&#x200B;**編輯**&#x200B;以開啟代理程式的設定。
1. 在&#x200B;**設定**&#x200B;標籤下，更新下列欄位：

   + **已啟用** — 檢查true
   + **代理程式使用者ID** — 留空此欄位

   ![復寫代理程式組態 — 設定](assets/aem-runtime/settings-config.png)

1. 在&#x200B;**傳輸**&#x200B;標籤下，更新下列欄位：

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **使用者** - `admin`
   + **密碼** - `admin`

   ![復寫代理程式設定 — 傳輸](assets/aem-runtime/transport-config.png)

1. 按一下&#x200B;**確定**&#x200B;以儲存設定並啟用&#x200B;**預設**&#x200B;復寫代理程式。
1. 您現在可以變更Author服務上的內容，並將其發佈到Publish服務。

![發佈頁面](assets/aem-runtime/publish-page-changes.png)

## 快速入門Jar啟動模式

快速入門Jar `aem-<tier>_<environment>-p<port number>.jar`的命名會指定其啟動方式。 AEM在特定層級、作者或發佈中啟動後，即無法變更為替代層級。 若要這麼做，必須刪除第一次執行期間產生的`crx-Quickstart`資料夾，而且必須重新執行Quickstart Jar。 環境和連線埠可以變更，但是它們需要停止/啟動本機AEM執行個體。

變更環境`dev`、`stage`和`prod`對開發人員來說很有用，以確保環境特定的設定已由AEM正確定義和解析。 建議主要針對預設`dev`環境執行模式執行本機開發。

可用的排列如下：

| 快速入門Jar檔案名稱 | 模式說明 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | 在連線埠4502上以開發執行模式的作者身分 |
| `aem-author_dev-p4502.jar` | 作為作者，在連線埠4502上處於開發執行模式（與`aem-author-p4502.jar`相同） |
| `aem-author_stage-p4502.jar` | 作為作者，在連線埠4502上處於測試執行模式 |
| `aem-author_prod-p4502.jar` | 在連線埠4502上以生產執行模式中的作者身分 |
| `aem-publish-p4503.jar` | 在連線埠4503上以開發執行模式發佈 |
| `aem-publish_dev-p4503.jar` | 在連線埠4503上以開發執行模式發佈（與`aem-publish-p4503.jar`相同） |
| `aem-publish_stage-p4503.jar` | 在連線埠4503上以測試執行模式發佈 |
| `aem-publish_prod-p4503.jar` | 在連線埠4503上以生產執行模式作為發佈 |

請注意，連線埠號碼可以是本機開發電腦上任何可用的連線埠，不過依照慣例而定：

+ 連線埠&#x200B;__4502__&#x200B;用於&#x200B;__本機AEM作者服務__
+ 連線埠&#x200B;__4503__&#x200B;用於&#x200B;__本機AEM發佈服務__

若要變更這些專案，可能需要調整AEM SDK設定

## 停止本機AEM執行階段

若要停止本機AEM執行階段(AEM製作或發佈服務)，請開啟用來啟動AEM執行階段的命令列視窗，然後點選「`Ctrl-C`」。 等待AEM關閉。 當關機程式完成時，命令列提示字元可用。

## 選用的本機AEM執行階段設定工作

+ __OSGi設定環境變數和密碼變數__&#x200B;是為AEM本機執行階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)專門設定的[，而不是使用aio CLI來管理它們。

## 何時更新Quickstart Jar

在每月的最後一個星期四或之後至少每月更新AEM SDK，這是AEM as a Cloud Service「功能發行」的發行步調。

>[!WARNING]
>
> 將快速入門Jar更新為新版本需要取代整個本機開發環境，導致遺失本機AEM存放庫中的所有程式碼、設定和內容。 確保任何不應銷毀的程式碼、設定或內容都會安全地提交至Git，或從本機AEM執行個體匯出為AEM套件。

### 升級AEM SDK時如何避免內容遺失

升級AEM SDK實際上會建立全新的AEM執行階段，包括新的存放庫，這表示對先前AEM SDK存放庫所做的任何變更都會遺失。 以下是協助在AEM SDK升級之間儲存內容的可行策略，可個別或一致使用：

1. 建立專用於包含「範例」內容的內容套件，以輔助開發並在Git中進行維護。 任何應透過AEM SDK升級而儲存的內容將儲存在此套件中，並在升級AEM SDK後重新部署。
1. 使用[oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)搭配`includepaths`指示詞，將內容從先前的AEM SDK存放庫複製到新的AEM SDK存放庫。
1. 使用AEM Package Manager備份任何內容，並將內容套件重新安裝在新的AEM SDKAEM SDK上。

請記住，在AEM SDK升級之間使用上述方法來維護程式碼，表示開發反模式。 非一次性程式碼應源自開發IDE，並透過部署流入AEM SDK。

## 疑難排解

### 連按兩下Quickstart Jar檔案會導致錯誤{#troubleshooting-double-click}

連按兩下Quickstart Jar以啟動時，會顯示錯誤強制回應視窗，導致AEM無法在本機啟動。

![疑難排解 — 連按兩下Quickstart Jar檔案](./assets/aem-runtime/troubleshooting__double-click.png)

這是因為AEM as a Cloud Service Quickstart Jar不支援按兩下Quickstart Jar以在本機啟動AEM。 而是必須從該命令列執行Jar檔案。

若要啟動AEM Author服務，`cd`進入包含Quickstart Jar的目錄並執行命令：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

或者，若要啟動AEM Publish服務，`cd`會進入包含Quickstart Jar的目錄並執行命令：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]

### 從命令列啟動「快速入門Jar」會立即中止{#troubleshooting-java-8}

從命令列啟動「快速入門Jar」時，流程會立即中止，且AEM服務不會啟動，並出現以下錯誤：

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

這是因為AEM as a Cloud Service需要Java™ SDK 11，而您執行的是其他版本，很可能是Java™ 8。 若要解決此問題，請下載並安裝[Oracle Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p=list&amp;p.offset=limit&amp;p.offset=0&amp;p.limit=14&amp;p.limit=144)。

安裝Oracle Java™ 11 SDK後，從命令列執行命令，確認其為作用中版本：

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

## 其他資源

+ [下載AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [Experience Manager Dispatcher檔案](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hant)
