---
title: 如何利用快速開發環境
description: 瞭解如何使用快速開發環境從本地電腦部署代碼和內容。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%

---

# 如何利用快速開發環境

學習 **如何使用** 快速開發環境(RDE)在AEMas a Cloud Service。 將代碼和內容部署到RDE(從您最喜愛的整合開發環境(IDE))，以加快最終代碼的開發週期。

使用 [WKNDAEM站點項目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 通過運行AEM-RDE，您將學習如AEM何將各種對象 `install` 命令。

- AEM代碼和內容包(all、ui.apps)部署
- OSGi捆綁和配置檔案部署
- Apache和Dispatcher將部署配置為zip檔案
- 像HTL這樣的單個檔案， `.content.xml` （對話框XML）部署
- 查看其他RDE命令，如 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## 必備條件

克隆 [WKND站點](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 在您最喜愛的IDE中開啟項目，以將AEM對象部署到RDE。

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

然後，運行以下maven命令，將AEM其構建並部署到本地SDK。

```
$ cd aem-guides-wknd/
$ mvn clean install -PautoInstallSinglePackage
```

## 使用AEMAEM-RDE插件部署對象

使用 `aem:rde:install` 命令，我們部署各種AEM對象。

### 部署 `all` 和 `dispatcher` 軟體包

通常的出發點是首先部署 `all` 和 `dispatcher` 程式包。

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

成功部署後，請驗證作者和發佈服務上的WKND站點。 您應該能夠添加、編輯WKND網站頁面上的內容並發佈它。

### 增強和部署元件

讓我們增強 `Hello World Component` 並部署到RDE。

1. 開啟對話框XML(`.content.xml`從 `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` 資料夾
1. 添加 `Description` 文本欄位 `Text` 對話框

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
1. 呈現 `Description` 現有屬性後 `<div>` 元素 `Text` 屬性。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. 通過執行主生成或同AEM步單個檔案，驗證本地SDK上的更改。

1. 將更改部署到RDE `ui.apps` 或部署單個對話框和HTL檔案。

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

1. 通過添加或編輯RDE `Hello World Component` 在WKND網站頁面上。

### 查看 `install` 命令選項

在上述單個檔案部署命令示例中， `-t` 和 `-p` 標籤用於分別指示JCR路徑的類型和目標。 讓我們回顧一下 `install` 命令選項。

```shell
$ aio aem:rde:install --help
```

旗子不言自明， `-s` 標誌僅用於將部署目標定為作者或發佈服務。 使用 `-t` 在部署時標籤 **內容檔案或內容xml** 檔案以及 `-p` 標誌，用於指定RDE環境中的目標JCRAEM路徑。

### 部署OSGi捆綁包

要瞭解如何部署OSGi捆綁包，讓我們增強 `HelloWorldModel` Java™類並將其部署到RDE。

1. 開啟 `HelloWorldModel.java` 檔案 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 資料夾
1. 更新 `init()` 方法：

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 通過部署本AEM地SDK驗證更改 `core` bundle via maven命令
1. 通過運行以下命令將更改部署到RDE

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 通過添加或編輯RDE `Hello World Component` 在WKND網站頁面上。

### 部署OSGi配置

您可以部署單個配置檔案或完整的配置包，例如：

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
>要僅在作者或發佈實例上安裝OSGi配置，請使用 `-s` 。


### 部署Apache或Dispatcher配置

Apache或Dispatcher配置檔案 **不能單獨部署**，但需要以ZIP檔案的形式部署整個Dispatcher資料夾結構。

1. 在的配置檔案中進行所需的更改 `dispatcher` 模組，為演示目的，更新 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` 快取 `html` 檔案只持續60秒。

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

1. 在本地驗證更改，請參見 [在本地運行Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) 的子菜單。
1. 通過運行以下命令將更改部署到RDE:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. 驗證RDE上的更改

## 其他AEMRDE插件命令

讓我們查看其他AEMRDE插件命令，以管理並與本地電腦中的RDE交互。

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

使用上述命令，可以從您最喜愛的IDE管理RDE，以加快開發/部署生命週期。

## 下一步

瞭解 [使用RDE的開發/部署生命週期](./development-life-cycle.md) 以速度提供功能。


## 其他資源

[RDE命令文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Adobe I/O RuntimeCLI插件，用於與快速開發AEM環境交互](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[項AEM目設定](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
