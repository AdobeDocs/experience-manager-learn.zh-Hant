---
title: 將AEM的本機AEM執行階段設為雲端服務開發
description: 使用AEM作為雲端服務SDK的Quickstart Jar設定本機AEM執行階段。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: 4cfbf975919eb38413be8446b70b107bbfebb845
workflow-type: tm+mt
source-wordcount: '1406'
ht-degree: 1%

---


# 設定本機AEM Runtime

Adobe Experience Manager(AEM)可以使用AEM作為雲端服務SDK的Quickstart Jar在本機執行。 這可讓開發人員在將自訂程式碼、組態和內容提交至來源控制項，並將它部署至AEM做為雲端服務環境之前，先進行部署和測試。

請注意， `~` 這是用戶目錄的速記。 在Windows中，這相當於 `%HOMEPATH%`。

## 安裝Java

Experience Manager是Java應用程式，因此需要Java SDK來支援開發工具。

1. [下載並安裝最新的Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+111%7E&amp;orderby=%40jcry%3E&amp;orderby=%40jcr%3Econtent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 執行下列命令以確認Java 11 SDK是否已安裝：
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## 下載AEM作為雲端服務SDK

AEM(Cloud Service SDK)或AEM SDK包含用來在本機執行AEM Author和Publish以進行開發的Quickstart Jar，以及相容版本的Dispatcher Tools。

1. 使用您的 [Adobe ID登入](https://experience.adobe.com/#/downloads) https://experience.adobe.com/#/downloads
   + 請注意，您的Adobe __組織__ 必須布建AEM做為雲端服務，才能將AEM下載為雲端服務SDK。
1. 導覽至「 __AEM做為雲端服務」標籤__
1. 依發佈日 __期__ ，依 __遞減__
1. 按一下最新 __的AEM SDK__ 結果列
1. 檢閱並接受EULA，然後點選「下 __載__ 」按鈕

## 從AEM SDK zip解壓縮快速入門(Quickstart Jar)

1. 解壓縮下載的檔 `aem-sdk-XXX.zip` 案

## 設定本機AEM Author服務{#set-up-local-aem-author-service}

本機AEM作者服務為開發人員提供數位行銷人員／內容作者將分享的本機體驗，以建立和管理內容。  AEM Author Service設計為製作和預覽環境，可針對它執行大部分功能開發驗證，使它成為本端開發程式的重要元素。

1. 建立資料夾 `~/aem-sdk/author`
1. 將快速 __啟動JAR檔案複製到__ ，並 `~/aem-sdk/author` 將其更名為 `aem-author-p4502.jar`
1. 從命令列執行下列動作，以啟動本機AEM作者服務：
   + `java -jar aem-author-p4502.jar`
      + 將管理員密碼提供為 `admin`。 任何管理員密碼都可接受，但建議使用本端開發的預設密碼，以減少重新設定的需要。

   您 *無法按兩下* ，將AEM啟動為Cloud Service Quickstart Jar [。](#troubleshooting-double-click)
1. 在網頁瀏覽器中存取本機AEM [作者](http://localhost:4502) (http://localhost:4502)服務

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

本機AEM Publish Service為開發人員提供AEM的本機使用者體驗，例如瀏覽AEM上的網站。 本機AEM Publish Service與AEM SDK的 [](./dispatcher-tools.md) Dispatcher工具整合，讓開發人員可進行煙霧測試並微調最終的使用者對應體驗，因此十分重要。

1. 建立資料夾 `~/aem-sdk/publish`
1. 將快速 __啟動JAR檔案複製到__ ，並 `~/aem-sdk/publish` 將其更名為 `aem-publish-p4503.jar`
1. 從命令列執行下列動作，以啟動本機AEM Publish Service:
   + `java -jar aem-publish-p4503.jar`
      + 將管理員密碼提供為 `admin`。 任何管理員密碼都可接受，但建議使用本端開發的預設密碼，以減少重新設定的需要。

   您 *無法按兩下* ，將AEM啟動為Cloud Service Quickstart Jar [。](#troubleshooting-double-click)
1. 在網頁瀏覽器中存取本機AEM Publish [服務](http://localhost:4503) (http://localhost:4503)

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

## 快速啟動Jar啟動模式

快速啟動Jar的命 `aem-<tier>_<environment>-p<port number>.jar` 名，指定啟動方式。 一旦AEM在特定層、作者或發佈中開始，就無法變更為替代層。 要執行此操作，必 `crx-Quickstart` 須刪除在首次運行期間生成的資料夾，並且必須重新運行Quickstart Jar。 環境和埠可以變更，但需要停止／啟動本機AEM例項。

變更環境 `dev`和 `stage` 變 `prod`更對開發人員有用，以確保AEM正確定義並解決環境特定組態。 建議您主要針對預設環境執行模式進行本 `dev` 機開發。

可用的排列如下：

+ `aem-author-p4502.jar`
   + 在埠4502的「開發」運行模式下作為作者
+ `aem-author_dev-p4502.jar`
   + 在埠4502上以Dev運行模式作為作者(與相同 `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + 在埠4502的「As Author in Staging run mode（As Author in Staging運行模式）」
+ `aem-author_prod-p4502.jar`
   + 作為埠4502上的「在生產中運行」模式的作者
+ `aem-publish-p4503.jar`
   + 在埠4503的「開發」運行模式下作為作者
+ `aem-publish_dev-p4503.jar`
   + 在埠4503上以Dev運行模式作為作者(與相同 `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + 在埠4503的「As Author in Staging run mode（As Author in Staging運行模式）」
+ `aem-publish_prod-p4503.jar`
   + 作為埠4503上的「在生產中運行」模式的作者

請注意，埠號可以是本地開發機器上的任何可用埠，但是按慣例：

+ Port __4502__ is used for __the local AEM Author service__
+ Port __4503__ is used for __the local AEM Publish service__

若要變更這些設定，可能需要對AEM SDK組態進行調整

## 停止本機AEM執行階段

若要停止本機AEM執行階段（AEM Author或Publish服務），請開啟用來啟動AEM Runtime的命令列視窗，然後點選 `Ctrl-C`。 等待AEM關閉。 當關閉過程完成時，命令行提示將可用。

## 何時更新快速啟動Jar

至少每月或不久之後，於每月的最後一個星期四（即AEM雲端服務「功能發行」的發行日期）更新AEM SDK。

>[!WARNING]
>
> 將Quickstart Jar更新為新版本需要取代整個本機開發環境，導致本機AEM儲存庫中的所有程式碼、設定和內容遺失。 請確定任何不應銷毀的程式碼、設定或內容都會安全地提交至Git，或從本機AEM例項匯出為AEM套件。

### 如何避免在升級AEM SDK時遺失內容

升級AEM SDK實際上是在建立全新的AEM執行階段，包括新的儲存庫，這表示對舊版AEM SDK儲存庫所做的任何變更都會遺失。 以下是協助在AEM SDK升級之間持續使用內容的可行策略，可單獨使用或搭配使用：

1. 建立專用於包含「範例」內容的內容套件，以協助開發，並以Git維護。 任何應透過AEM SDK升級持續保存的內容，都會保存在此套件中，並在升級AEM SDK後重新部署。
1. 搭配 [指令使用oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)`includepaths` ，將內容從先前的AEM SDK儲存庫複製到新的AEM SDK儲存庫。
1. 使用AEM Package Manager備份任何內容，並在舊版AEM SDK上重新安裝內容至新的AEM SDK。

請記住，使用上述方法在AEM SDK升級之間維護程式碼，表示開發反模式。 非一次性代碼應源自您的開發IDE，並透過部署流入AEM SDK。

## 疑難排解

## 按兩下快速啟動Jar檔案會導致錯誤{#troubleshooting-double-click}

當連按兩下「快速啟動Jar」開始時，會顯示錯誤模式，使AEM無法在本機啟動。

![疑難排解——按兩下Quickstart Jar檔案](./assets/aem-runtime/troubleshooting__double-click.png)

這是因為AEM作為雲端服務快速入門Jar不支援連按兩下快速入門Jar以在本機啟動AEM。 而必須從該命令行運行Jar檔案。

若要啟動AEM Author服務，請 `cd` 進入包含Quickstart Jar的目錄並執行命令：

`$ java -jar aem-author-p4502.jar`

或者，若要啟動AEM Publish服務，請 `cd` 進入包含Quickstart Jar的目錄並執行命令：

`$ java -jar aem-author-p4503.jar`

## 從命令行啟動快速啟動Jar會立即中止{#troubleshooting-java-8}

從命令行啟動Quickstart Jar時，進程會立即中止，而AEM服務不會啟動，並出現以下錯誤：

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

這是因為AEM做為雲端服務需要Java SDK 11，而且您所執行的版本可能是不同的Java 8。 要解決此問題，請下載並安裝 [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+111%7E&amp;orderby=%40jcry%3E&amp;orderby=%40jcr%3Econtent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)。
在安裝Java SDK 11後，從命令列執行下列動作，以確認其為作用中版本。

在安裝Java 11 SDK後，從命令列執行命令以確認其為作用中版本：

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## 其他資源

+ [下載AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [下載Docker](https://www.docker.com/)
+ [Experience Manager Dispatcher檔案](https://docs.adobe.com/content/help/zh-Hant/experience-manager-dispatcher/using/dispatcher.html)
