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
source-git-commit: d199ff3b9f4d995614c193f52dc90270f2283adf
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 0%

---

# 如何使用快速開發環境

瞭解&#x200B;**如何在AEM as a Cloud Service中使用**&#x200B;快速開發環境(RDE)。 從您最愛的整合式開發環境(IDE)，將程式碼和內容部署到RDE，以加快接近最終版本的程式碼開發週期。

使用[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)，您將瞭解如何從您最喜愛的IDE執行AEM-RDE的`install`命令，將各種AEM成品部署到RDE。

- AEM程式碼和內容套件(all， ui.apps)部署
- OSGi套件組合和設定檔案部署
- 以zip檔案形式部署Apache和Dispatcher
- 個別檔案，例如HTL、`.content.xml` （對話方塊XML）部署
- 檢閱其他RDE命令，例如`status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## 必備條件

複製[WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)專案，並在您最愛的IDE中開啟該專案，以將AEM成品部署至RDE。

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

然後，執行以下maven命令以將其建置並部署至本機AEM-SDK。

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## 使用AEM-RDE外掛程式部署AEM成品

首先，確定您已安裝[最新的`aio` CLI模組](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools#aio-cli)。

然後，使用`aio aem:rde:install`命令部署各種AEM成品。 現在您必須

### 部署`all`和`dispatcher`封裝

常見的起點是先執行下列命令以部署`all`和`dispatcher`封裝。

```shell
# Install the 'all' content package (zip file)
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' deployment artifact (zip file)
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

成功部署後，請在作者和發佈服務上驗證WKND網站。 您應該能夠新增和編輯WKND網站頁面上的內容並加以發佈。

### 增強和部署元件

讓我們增強`Hello World Component`並將其部署至RDE。

1. 從`ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/`資料夾開啟對話方塊XML (`.content.xml`)檔案
1. 在現有`Text`對話方塊欄位之後新增`Description`文字欄位

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. 從`ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld`資料夾開啟`helloworld.html`檔案
1. 在`Text`屬性的現有`<div>`元素之後轉譯`Description`屬性。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. 執行Maven建置或同步個別檔案，驗證本機AEM SDK上的變更。

1. 透過`ui.apps`套件或透過部署個別對話方塊和HTL檔案來部署對RDE的變更：

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

1. 在WKND網站頁面上新增或編輯`Hello World Component`，以驗證RDE上的變更。

### 檢閱`install`命令選項

在上述個別檔案部署命令範例中，`-t`和`-p`旗標分別用來表示JCR路徑的型別和目的地。 請執行以下命令，檢閱可用的`install`命令選項。

```shell
$ aio aem:rde:install --help
```

標幟的含義不言自明，`-s`標幟有助於將部署目標定位到作者或發佈服務。 部署&#x200B;**content-file或content-xml**&#x200B;檔案時，請使用`-t`旗標搭配`-p`旗標來指定AEM RDE環境中的目的地JCR路徑。

### 部署OSGi套件組合

若要瞭解如何部署OSGi套件，請增強`HelloWorldModel` Java™類別並將其部署至RDE。

1. 從`core/src/main/java/com/adobe/aem/guides/wknd/core/models`資料夾開啟`HelloWorldModel.java`檔案
1. 更新`init()`方法，如下所示：

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. 透過maven命令部署`core`套件組合，驗證本機AEM-SDK上的變更
1. 執行下列命令，將變更部署至RDE

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. 在WKND網站頁面上新增或編輯`Hello World Component`，以驗證RDE上的變更。

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
>若只要在作者或發佈執行個體上安裝OSGi設定，請使用`-s`旗標。


### 部署Apache或Dispatcher設定

Apache或Dispatcher設定檔案&#x200B;**無法個別部署**，但整個Dispatcher資料夾結構必須以ZIP檔案的形式部署。

1. 在`dispatcher`模組的設定檔中進行所需的變更，為了示範之用，請更新`dispatcher/src/conf.d/available_vhosts/wknd.vhost`以快取`html`個檔案，僅保留60秒。

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

1. 在本機驗證變更，請參閱[在本機執行Dispatcher](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools)以取得詳細資料。
1. 執行下列命令，將變更部署至RDE：

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. 驗證RDE上的變更。

### 部署組態(YAML)檔案

可以使用`install`命令將CDN、維護工作、記錄檔轉送和AEM API驗證組態檔部署至RDE。 這些設定在AEM專案的`config`資料夾中管理為YAML檔案，如需詳細資訊，請參閱[支援的設定](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/config-pipeline#configurations)。

若要瞭解如何部署組態檔，請增強`cdn`組態檔並將其部署到RDE。

1. 從`config`資料夾開啟`cdn.yaml`檔案
1. 更新所需的設定，例如，將速率限制更新為每秒200個請求

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     trafficFilters:
       rules:
       #  Block client for 5m when it exceeds an average of 100 req/sec to origin on a time window of 10sec
       - name: limit-origin-requests-client-ip
         when:
           reqProperty: tier
           equals: 'publish'
         rateLimit:
           limit: 200 # updated rate limit
           window: 10
           count: fetches
           penalty: 300
           groupBy:
             - reqProperty: clientIp
         action: log
   ...
   ```

1. 執行下列命令，將變更部署至RDE

   ```shell
   $ aio aem:rde:install -t env-config ./config/cdn.yaml
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

瞭解如何使用RDE](./development-life-cycle.md)快速提供功能的[開發/部署生命週期。


## 其他資源

[RDE命令檔案](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments)

用於與AEM快速開發環境互動的[Adobe I/O Runtime CLI外掛程式](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM專案設定](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup)
