---
title: 如何利用快速開發環境
description: 了解如何使用快速開發環境，從本機電腦部署程式碼和內容。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 65d54f0137786c7e8ac9ac962c424dd20bf5f3dd
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---


# 如何利用快速開發環境

學習 **如何使用** AEMas a Cloud Service的快速開發環境(RDE)。 部署代碼和內容，以便從您喜愛的整合開發環境(IDE)，將接近最終的代碼的開發週期更快地部署到RDE。

使用 [AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 您可以透過執行AEM-RDE，了解如何將各種AEM成品部署至RDE `install` 命令。

- AEM程式碼與內容套件（所有， ui.apps）部署
- OSGi捆綁和配置檔案部署
- Apache和Dispatcher將部署設定為zip檔案
- 個別檔案，例如HTL、 `.content.xml` （對話XML）部署
- 查看其他RDE命令，如 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## 必備條件

複製 [WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 將項目在您喜愛的IDE中開啟，以將AEM對象部署到RDE上。

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

然後，執行下列maven命令，建立並部署至本機AEM-SDK。

```
$ cd aem-guides-wknd/
$ mvn clean install -PautoInstallSinglePackage
```

## 使用AEM-RDE外掛程式部署AEM成品

使用 `aem:rde:install` 命令，部署各種AEM成品。

### 部署 `all` 和 `dispatcher` 套件

常見的起點是先部署 `all` 和 `dispatcher` 程式包，方法是執行下列命令。

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

成功部署後，請驗證製作和發佈服務上的WKND網站。 您應該可以新增、編輯WKND網站頁面上的內容並發佈。

### 增強和部署元件

我們加強 `Hello World Component` 並部署到RDE。

1. 開啟對話框XML(`.content.xml`)檔案 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` 資料夾
1. 新增 `Description` 文字欄位 `Text` 對話欄位

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. 開啟 `helloworld.html` 檔案 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` 資料夾
1. 呈現 `Description` 屬性 `<div>` 元素 `Text` 屬性。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. 執行Maven組建或同步個別檔案，以驗證本機AEM-SDK上的變更。

1. 透過將變更部署至RDE `ui.apps` 封裝，或部署個別Dialog和HTL檔案。

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

1. 通過添加或編輯 `Hello World Component` 在WKND網站頁面上。

### 檢閱 `install` 命令選項

在上述個別檔案部署命令範例中， `-t` 和 `-p` 標幟用於分別指示JCR路徑的類型和目的地。 讓我們檢閱可用 `install` 命令選項。

```shell
$ aio aem:rde:install --help
```

旗子不言自明， `-s` 標幟只將部署鎖定在製作或發佈服務上很實用。 使用 `-t` 部署時的標幟 **content-file或content-xml** 檔案 `-p` 此旗標可在AEM RDE環境中指定目標JCR路徑。

### 部署OSGi捆綁包

若要了解如何部署OSGi套件組合，請改善 `HelloWorldModel` Java™類，並將其部署到RDE。

1. 開啟 `HelloWorldModel.java` 檔案 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 資料夾
1. 更新 `init()` 方法，如下所示：

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 部署以在本機AEM-SDK上驗證變更 `core` 通過maven命令捆綁
1. 通過運行以下命令將更改部署到RDE

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 通過添加或編輯 `Hello World Component` 在WKND網站頁面上。

### 部署OSGi配置

您可以部署個別設定檔案或完成設定套件，例如：

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
>若只要在製作或發佈執行個體上安裝OSGi設定，請使用 `-s` 標籤。


### 部署Apache或Dispatcher設定

Apache或Dispatcher設定檔案 **無法個別部署**，但整個Dispatcher資料夾結構必須以ZIP檔案的形式部署。

1. 在 `dispatcher` 模組，為了示範用途，請更新 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` 快取 `html` 檔案時間僅為60秒。

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

1. 在本機驗證變更，請參閱 [在本機執行Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) 以取得更多詳細資訊。
1. 通過運行以下命令將更改部署到RDE:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. 驗證RDE上的更改

## 其他AEM RDE外掛程式命令

讓我們檢閱其他AEM RDE外掛程式命令，從本機電腦管理RDE並與之互動。

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

使用上述命令，可從您喜愛的IDE中管理您的RDE，以加快開發/部署生命週期。

## 下一步

了解 [使用RDE的開發/部署生命週期](./development-life-cycle.md) 以速度提供功能。


## 其他資源

[RDE命令文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Adobe I/O Runtime CLI外掛程式，用於與AEM Rapid Development Environments互動](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM專案設定](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
