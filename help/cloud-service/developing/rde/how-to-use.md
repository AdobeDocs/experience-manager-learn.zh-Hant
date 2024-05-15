---
title: 如何使用快速開發環境
description: 瞭解如何使用快速開發環境，從本機電腦部署程式碼和內容。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 792
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 0%

---

# 如何使用快速開發環境

瞭解 **使用方式** AEMas a Cloud Service中的快速開發環境(RDE)。 從您最愛的整合式開發環境(IDE)，將程式碼和內容部署到RDE，以加快接近最終版本的程式碼開發週期。

使用 [AEM WKND網站專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 您將瞭解如何透過執行AEM-RDE將各種AEM成品部署到RDE `install` 命令。

- AEM程式碼和內容套件(all， ui.apps)部署
- OSGi套件組合和設定檔案部署
- Apache和Dispatcher會將部署設定為zip檔案
- 個別檔案，例如HTL、 `.content.xml` （對話方塊XML）部署
- 檢閱其他RDE命令，例如 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## 必備條件

原地複製 [WKND網站](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 專案並在您最愛的IDE中開啟，以將AEM成品部署至RDE。

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

接著，執行以下maven命令，將其建置並部署至本機AEM-SDK。

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## 使用AEM-RDE外掛程式部署AEM成品

使用 `aem:rde:install` 命令，讓我們部署各種AEM成品。

### 部署 `all` 和 `dispatcher` 套件

常見的起始點是先部署 `all` 和 `dispatcher` 套裝軟體，方法是執行下列命令。

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

成功部署後，請在作者和發佈服務上驗證WKND網站。 您應該能夠新增和編輯WKND網站頁面上的內容並加以發佈。

### 增強和部署元件

讓我們來增強 `Hello World Component` 並將其部署至RDE。

1. 開啟對話方塊XML (`.content.xml`)檔案來源 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` 資料夾
1. 新增 `Description` 現有欄位之後的文字欄位 `Text` 對話方塊欄位

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. 開啟 `helloworld.html` 檔案來源 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` 資料夾
1. 呈現 `Description` 現有屬性之後的屬性 `<div>` 的元素 `Text` 屬性。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. 執行Maven組建或同步個別檔案，驗證本機AEM-SDK上的變更。

1. 透過將變更部署至RDE `ui.apps` 封裝或部署個別對話方塊和HTL檔案。

   ```shell
   # Using 'ui.apps' package
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 透過新增或編輯 `Hello World Component` 在WKND網站頁面上。

### 檢閱 `install` 命令選項

在上述個別檔案部署命令範例中， `-t` 和 `-p` 旗標分別用來指出JCR路徑的型別和目的地。 讓我們檢閱可用的 `install` 命令選項，方法是執行下列命令。

```shell
$ aio aem:rde:install --help
```

這些旗幟含義明確， `-s` 標幟可用來將部署目標定位為製作或發佈服務。 使用 `-t` 部署時標幟 **content-file或content-xml** 檔案以及 `-p` 此旗標可指定AEM RDE環境中的目的地JCR路徑。

### 部署OSGi套件組合

若要瞭解如何部署OSGi套件，請增強 `HelloWorldModel` Java™類別並將其部署至RDE。

1. 開啟 `HelloWorldModel.java` 檔案來源 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 資料夾
1. 更新 `init()` 方法如下：

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 透過部署以下專案驗證本機AEM-SDK上的變更： `core` 透過maven指令的套件
1. 執行下列命令，將變更部署至RDE

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 透過新增或編輯 `Hello World Component` 在WKND網站頁面上。

### 部署OSGi設定

例如，您可以部署個別的組態檔或完整的組態套件，例如：

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>若只要在作者或發佈執行個體上安裝OSGi設定，請使用 `-s` 標幟。


### 部署Apache或Dispatcher設定

Apache或Dispatcher設定檔案 **無法個別部署**，但整個Dispatcher資料夾結構都必須以ZIP檔案的形式部署。

1. 在的設定檔案中進行所需的變更 `dispatcher` 模組，如需示範，請更新 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` 快取 `html` 檔案僅保留60秒。

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. 在本機驗證變更，請參閱 [在本機執行Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) 以取得更多詳細資料。
1. 執行下列命令，將變更部署至RDE：

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. 驗證RDE上的變更

## 其他AEM RDE外掛程式命令

讓我們檢閱要管理的其他AEM RDE外掛程式命令，並從您的本機電腦與RDE互動。

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

使用上述命令，您就可以從最喜愛的IDE管理RDE，以加快開發/部署生命週期。

## 下一步

瞭解 [使用RDE的開發/部署生命週期](./development-life-cycle.md) 以快速提供功能。


## 其他資源

[RDE命令檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[與AEM快速開發環境互動的Adobe I/O Runtime CLI外掛程式](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM專案設定](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
