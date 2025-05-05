---
title: 檢閱完整棧疊專案的ui.frontend模組
description: 檢閱Maven型全棧疊AEM Sites專案的前端開發、部署和傳送生命週期。
version: Experience Manager as a Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
duration: 364
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# 檢閱完整棧疊AEM專案的「ui.frontend」模組 {#aem-full-stack-ui-frontent}

在本章中，我們將檢閱完整棧疊AEM專案的前端成品開發、部署和傳遞，其重點為&#x200B;__WKND Sites專案__&#x200B;的「ui.frontend」模組。


## 目標 {#objective}

* 瞭解AEM完整棧疊專案中前端成品的建置和部署流程
* 檢閱AEM完整棧疊專案的`ui.frontend`模組的[webpack](https://webpack.js.org/)設定
* AEM client library （亦稱為clientlibs）產生程式

## AEM完整棧疊和快速網站建立專案的前端部署流程

>[!IMPORTANT]
>
>此影片說明並示範&#x200B;**完整棧疊和快速網站建立**&#x200B;專案的前端流程，以概述前端資源建置、部署和傳遞模式中的細微差異。

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 先決條件 {#prerequisites}


* 複製[AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd)
* 建立並部署複製的AEM WKND Sites專案至AEM as a Cloud Service。

如需詳細資訊，請參閱AEM WKND網站專案[README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md)。

## AEM完整棧疊專案前端成品流程 {#flow-of-frontend-artifacts}

下列是完整棧疊AEM專案中前端成品的&#x200B;__開發、部署和傳遞__&#x200B;流程的高階表示。

![開發、部署及傳遞前端成品](assets/Dev-Deploy-Delivery-AEM-Project.png)


在開發階段中，會透過更新`ui.frontend/src/main/webpack`資料夾中的CSS、JS檔案來執行前端變更（例如樣式和重新命名）。 然後在建置期間，[webpack](https://webpack.js.org/)模組套件組合器和Maven外掛程式會將這些檔案轉換成最佳化的AEM clientlibs，位於`ui.apps`模組下。

在AEM as a Cloud Service[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=zh-Hant)中執行&#x200B;__完整棧疊__&#x200B;管道時，會將前端變更部署到Cloud Manager環境。

前端資源會透過以`/etc.clientlibs/`開頭的URI路徑傳送至網頁瀏覽器，通常會在AEM Dispatcher和CDN上快取。


>[!NOTE]
>
> 同樣地，在&#x200B;__AEM快速網站建立歷程__&#x200B;中，透過執行&#x200B;__前端__&#x200B;管道，將[前端變更](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html?lang=zh-Hant)部署到AEM as a Cloud Service環境。請參閱[設定您的管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html?lang=zh-Hant)

### 檢閱WKND Sites專案中的Webpack設定 {#development-frontend-webpack-clientlib}

* 有三個&#x200B;__webpack__&#x200B;設定檔案可用來捆綁WKND Sites前端資源。

   1. `webpack.common` — 這包含指示WKND資源繫結和最佳化的&#x200B;__公用__&#x200B;設定。 __output__&#x200B;屬性會告訴應在何處產生其建立的整合檔案(也稱為JavaScript套裝，但不要與AEM OSGi套裝混淆)。 預設名稱設為`clientlib-site/js/[name].bundle.js`。

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js`包含webpack-dev-serve的&#x200B;__開發__&#x200B;設定，並指向要使用的HTML範本。 它也包含在`localhost:4502`上執行之AEM執行個體的Proxy設定。

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js`包含&#x200B;__production__&#x200B;設定，並使用外掛程式將開發檔案轉換為最佳化的組合。

  ```javascript
      ...
      module.exports = merge(common, {
          mode: 'production',
          optimization: {
              minimize: true,
              minimizer: [
                  new TerserPlugin(),
                  new CssMinimizerPlugin({ ...})
          }
      ...    
  ```


* 使用[aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator)外掛程式，使用`clientlib.config.js`檔案中管理的組態，將套件資源移至`ui.apps`模組。

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* 來自`ui.frontend/pom.xml`的&#x200B;__frontend-maven-plugin__&#x200B;會在AEM專案建置期間協調webpack套件組合和clientlib產生。

`$ mvn clean install -PautoInstallSinglePackage`

### 部署至AEM as a Cloud Service {#deployment-frontend-aemaacs}

[__完整棧疊__&#x200B;管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=zh-Hant&#full-stack-pipeline)將這些變更部署到AEM as a Cloud Service環境。


### 來自AEM as a Cloud Service的傳遞 {#delivery-frontend-aemaacs}

透過完整棧疊管道部署的前端資源會從AEM網站以`/etc.clientlibs`檔案的形式傳送到網頁瀏覽器。 您可以造訪[公開託管的WKND網站](https://wknd.site/content/wknd/us/en.html)並檢視網頁來源來確認此資訊。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 恭喜！ {#congratulations}

恭喜，您已檢閱完整棧疊專案的ui.frontend模組

## 後續步驟 {#next-steps}

在下一章[更新專案以使用前端管道](update-project.md)，您將更新AEM WKND Sites專案以將其啟用為前端管道合約。
