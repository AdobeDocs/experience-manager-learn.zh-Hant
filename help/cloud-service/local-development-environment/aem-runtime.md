---
title: 設定本機執AEM行時期AEM做為Cloud Service開發
description: 使用作為Cloud ServiceAEM SDK的Quickstart Jar的AEM本機執行階段來設定。
feature: 開發人員工具
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
topic: 開發
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d49ae402b332ba972a78cdbd8f5bf962b91c83b1
workflow-type: tm+mt
source-wordcount: '1734'
ht-degree: 2%

---


# 設定本機執AEM行階段

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="本機執AEM行時期"
>abstract="Adobe Experience Manager(AEM)可以使用本機AEM作為Cloud ServiceSDK的快速入門Jar執行。 這可讓開發人員在將自訂程式碼、組態和內容提交至來源控制項，並將它部署至Cloud Service環境之前，先進行部署AEM和測試。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="下載AEM為Cloud ServiceSDK"

Adobe Experience Manager(AEM)可以使用作為AEMCloud ServiceSDK快速入門Jar的本機執行。 這可讓開發人員在將自訂程式碼、組態和內容提交至來源控制項，並將它部署至Cloud Service環境之前，先進行部署AEM和測試。

請注意，`~`是用作用戶目錄的速記。 在Windows中，此值相當於`%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK來支援開發工具。

1. [下載並安裝最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+111%7E+11%7E&amp;orderby=%3E=%40jcr%3E&amp;orderby=%3Econtent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 執行下列命令以確認Java 11 SDK是否已安裝：
   + Windows:`java -version`
   + macOS / Linux:`java --version`

![Java](./assets/aem-runtime/java.png)

## 下載AEM為Cloud ServiceSDK

作AEM為Cloud ServiceSDK(AEM或SDK)，包含用於在本端執行AEM Author和Publish以進行開發的Quickstart Jar，以及相容版本的Dispatcher Tools。

1. 使用您的Adobe ID登入[https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads)
   + 請注意，您的Adobe組織&#x200B;__必須__&#x200B;布建AEM為Cloud Service，才能以AEMCloud ServiceSDK下載。
1. 導覽至&#x200B;__作為AEMCloud Service__&#x200B;標籤
1. 按&#x200B;____&#x200B;降序&#x200B;__順序的發佈日期__&#x200B;排序
1. 按一下最新的&#x200B;__AEM SDK__&#x200B;結果列
1. 檢閱並接受EULA，然後點選&#x200B;__Download__&#x200B;按鈕

## 從SDK AEM zip解壓縮Quickstart Jar

1. 解壓縮下載的`aem-sdk-XXX.zip`檔案

## 設定本機AEM Author服務{#set-up-local-aem-author-service}

本機AEM作者服務為開發人員提供數位行銷人員／內容作者將分享的本機體驗，以建立和管理內容。  AEM Author Service設計為製作和預覽環境，可針對它執行大部分功能開發驗證，使它成為本端開發程式的重要元素。

1. 建立資料夾`~/aem-sdk/author`
1. 將&#x200B;__快速入門JAR__&#x200B;檔案複製到`~/aem-sdk/author` ，並將其更名為`aem-author-p4502.jar`
1. 從命令列執行下列動作，以啟動本機AEM作者服務：
   + `java -jar aem-author-p4502.jar`
      + 提供管理員密碼為`admin`。 任何管理員密碼都可接受，但建議使用本端開發的預設密碼，以減少重新設定的需要。

   您&#x200B;*cannot*&#x200B;不能通過按兩下](#troubleshooting-double-click)AEM以Cloud Service快速入門Jar [啟動。
1. 在Web瀏覽器中存取位於[http://localhost:4502](http://localhost:4502)的本機AEM作者服務

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## 設定本機AEM Publish服務

本機AEM Publish Service為開發人員提供本機使用者將擁有的AEM體驗，例如瀏覽所在的網站AEM。 本機AEM Publish Service與AEMSDK的[Dispatcher工具](./dispatcher-tools.md)整合，讓開發人員可對最終的使用者對應體驗進行煙霧測試和微調，因此十分重要。

1. 建立資料夾`~/aem-sdk/publish`
1. 將&#x200B;__快速入門JAR__&#x200B;檔案複製到`~/aem-sdk/publish` ，並將其更名為`aem-publish-p4503.jar`
1. 從命令列執行下列動作，以啟動本機AEM Publish Service:
   + `java -jar aem-publish-p4503.jar`
      + 提供管理員密碼為`admin`。 任何管理員密碼都可接受，但建議使用本端開發的預設密碼，以減少重新設定的需要。

   您&#x200B;*cannot*&#x200B;不能通過按兩下](#troubleshooting-double-click)AEM以Cloud Service快速入門Jar [啟動。
1. 在網頁瀏覽器中存取位於[http://localhost:4503](http://localhost:4503)的本機AEM Publish Service

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## 模擬內容分發{#content-distribution}

在真正的Cloud Service環境中，使用[Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html)和Adobe管線，將內容從作者服務散發至發佈服務。 [Adobe管線](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution)是一種隔離的微服務，僅適用於雲環境。

在開發期間，可能需要使用本機「作者」和「發佈」服務來模擬內容的發佈。 這可以通過啟用舊式複製代理來實現。

>[!NOTE]
>
> 複製代理僅可用於本地快速啟動JAR中，並僅提供內容分發的模擬。

1. 登入&#x200B;**Author**&#x200B;服務並導覽至[http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html)。
1. 按一下&#x200B;**預設代理（發佈）**&#x200B;開啟預設的複製代理。
1. 按一下&#x200B;**編輯**&#x200B;以開啟代理的配置。
1. 在&#x200B;**Settings**&#x200B;標籤下，更新下列欄位：

   + **啟用** -檢查是否為真
   + **代理用戶Id**  —— 將此欄位留空

   ![複製代理配置——設定](assets/aem-runtime/settings-config.png)

1. 在&#x200B;**Transport**&#x200B;標籤下，更新以下欄位：

   + **URI** -  `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **用戶** -  `admin`
   + **密碼** -  `admin`

   ![複製代理配置——傳輸](assets/aem-runtime/transport-config.png)

1. 按一下&#x200B;**確定**&#x200B;保存配置並啟用&#x200B;**預設**&#x200B;複製代理。
1. 您現在可以變更「作者」服務的內容，並將它們發佈至「發佈」服務。

![發佈頁面](assets/aem-runtime/publish-page-changes.png)

## 快速啟動Jar啟動模式

Quickstart Jar的命名`aem-<tier>_<environment>-p<port number>.jar`指定如何啟動。 一旦AEM在特定層、作者或發佈中開始，就無法變更為替代層。 要執行此操作，必須刪除在首次運行期間生成的`crx-Quickstart`資料夾，並且必須重新運行Quickstart Jar。 環境和埠可以更改，但需要停止／啟動本地實AEM例。

變更環境`dev`、`stage`和`prod`對於開發人員來說非常有用，以確保環境特定配置的定義和解決方式正確AEM。 建議您主要針對預設的`dev`環境執行模式進行本機開發。

可用的排列如下：

+ `aem-author-p4502.jar`
   + 在埠4502的「開發」運行模式下作為作者
+ `aem-author_dev-p4502.jar`
   + As Author in Dev run mode on port 4502（與`aem-author-p4502.jar`相同）
+ `aem-author_stage-p4502.jar`
   + 在埠4502的「As Author in Staging run mode（As Author in Staging運行模式）」
+ `aem-author_prod-p4502.jar`
   + 作為埠4502上的「在生產中運行」模式的作者
+ `aem-publish-p4503.jar`
   + 在埠4503的「開發」運行模式下作為作者
+ `aem-publish_dev-p4503.jar`
   + As Author in Dev run mode on port 4503(as as `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + 在埠4503的「As Author in Staging run mode（As Author in Staging運行模式）」
+ `aem-publish_prod-p4503.jar`
   + 作為埠4503上的「在生產中運行」模式的作者

請注意，埠號可以是本地開發機器上的任何可用埠，但是按慣例：

+ 埠&#x200B;__4502__&#x200B;用於&#x200B;__本機AEM作者服務__
+ 埠&#x200B;__4503__&#x200B;用於&#x200B;__本機AEM Publish服務__

若要變更這些設定，可能需要對AEMSDK組態進行調整

## 停止本機執行AEM時期

若要停止本機執行AEM階段（AEM Author或Publish服務），請開啟用來啟動執行階段的命AEM令列視窗，然後點選`Ctrl-C`。 等AEM待關閉。 當關閉過程完成時，命令行提示將可用。

## 可選本AEM機執行時期設定工作

+ __OSGi配置環境變數和__ 密碼變 [量是專門為本AEM地運行時設定的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development)，而不是使用aio CLI管理它們。

## 何時更新快速啟動Jar

至少每月AEM更新SDK一次，或在每月最後一個星期四之後不久更新SDK，此為Cloud Service「功能發行」的AEM發行順序。

>[!WARNING]
>
> 將Quickstart Jar更新為新版本需要替換整個本地開發環境，從而導致丟失本地儲存庫中的所有代碼、配置和AEM內容。 請確定任何不應銷毀的程式碼、設定或內容都安全提交至Git，或從本機例項匯出AEM為AEMPackages。

### 如何避免在升級SDK時遺失內AEM容

升級AEMSDK有效地建立了全新的執AEM行階段，包括新的儲存庫，這表示對舊AEMSDK儲存庫所做的任何變更都會遺失。 以下是協助在SDK升級之間持續使用內容的AEM可行策略，可以單獨使用或搭配使用：

1. 建立專用於包含「範例」內容的內容套件，以協助開發，並以Git維護。 任何應透過SDK升級持AEM續存在的內容，都會保存在此套件中，並在升級SDK後重新部AEM署。
1. 使用[oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)搭配`includepaths`指令，將內容從舊版AEMSDK存放庫複製至新的AEMSDK存放庫。
1. 使用舊版SDK上AEM的Package Manager和內容套件來備份任何內AEM容，並在新的AEMSDK上重新安裝。

請記住，使用上述方法在SDK升級之AEM間維護程式碼，表示開發反模式。 非一次性代碼應源自您的開發IDE，並透過部署流AEM入SDK。

## 疑難排解

## 按兩下快速啟動Jar檔案會導致錯誤{#troubleshooting-double-click}

按兩下要啟動的快速啟動Jar時，將顯示錯誤模式，阻止在本AEM地啟動。

![疑難排解——按兩下Quickstart Jar檔案](./assets/aem-runtime/troubleshooting__double-click.png)

這是因AEM為作為Cloud Service快速啟動Jar不支援按兩下快速啟動Jar以在本地AEM啟動。 而必須從該命令行運行Jar檔案。

要啟動AEM Author服務，請將`cd`放入包含Quickstart Jar的目錄中，然後執行命令：

`$ java -jar aem-author-p4502.jar`

或者，若要啟動AEM Publish服務，請`cd`進入包含Quickstart Jar的目錄並執行命令：

`$ java -jar aem-publish-p4503.jar`

## 從命令行啟動快速啟動Jar會立即中止{#troubleshooting-java-8}

從命令行啟動Quickstart Jar時，進程會立即中止，而服AEM務不會啟動，並出現以下錯誤：

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

這是因AEM為Cloud Service需要Java SDK 11，而且您執行的版本很可能是Java 8。 若要解決此問題，請下載並安裝[OracleJava SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+111%7E+11%7E&amp;orderby=%3E=%40jcr%3E&amp;orderby=%3Econtent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)。
在安裝Java SDK 11後，從命令列執行下列動作，以確認其為作用中版本。

在安裝Java 11 SDK後，從命令列執行命令以確認其為作用中版本：

+ Windows: `java -version`
+ macOS / Linux:`java --version`

## 其他資源

+ [下載AEMSDK](https://experience.adobe.com/#/downloads)
+ [Adobe雲管理員](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [Experience Manager調度程式文檔](https://docs.adobe.com/content/help/zh-Hant/experience-manager-dispatcher/using/dispatcher.html)
