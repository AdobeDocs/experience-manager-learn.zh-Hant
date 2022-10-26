---
title: 檢閱完整堆疊專案的ui.frontend模組
description: 檢閱基於Maven的完整堆疊AEM Sites專案的前端開發、部署和傳送生命週期。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---


# 檢閱完整堆疊AEM專案的「ui.frontend」模組 {#aem-full-stack-ui-frontent}

在中，本章中我們將重點放在的「ui.frontend」模組上，審視完整堆疊AEM專案的前端成品的開發、部署和傳送 __WKND Sites專案__.


## 目標 {#objective}

* 了解AEM完整堆疊專案中前端成品的建立和部署流程
* 檢閱AEM完整堆疊專案的 `ui.frontend` 模組 [webpack](https://webpack.js.org/) 設定
* AEM用戶端程式庫（也稱為clientlibs）產生程式

## AEM完整堆疊和快速網站建立專案的前端部署流程

>[!IMPORTANT]
>
>此影片說明並示範兩者的前端流程 **完整堆疊和快速建立網站** 專案，概述前端資源建置、部署和傳送模型的細微差異。

>[!VIDEO](https://video.tv.adobe.com/v/3409344/)

## 必備條件 {#prerequisites}


* 複製 [AEM WKND Sites專案](https://github.com/adobe/aem-guides-wknd)
* 已建立並部署將複製的AEM WKND Sites專案至AEMas a Cloud Service。

請參閱AEM WKND Site專案 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) 以取得更多詳細資訊。

## AEM全堆棧項目前端對象流 {#flow-of-frontend-artifacts}

以下是 __開發、部署和傳遞__ 完整堆疊AEM專案中前端成品的流量。

![開發、部署和傳遞前端對象](assets/Dev-Deploy-Delivery-AEM-Project.png)


在開發階段期間，前端變更（例如樣式和品牌重塑）會透過從 `ui.frontend/src/main/webpack` 檔案夾。 在建置期間， [webpack](https://webpack.js.org/) module-bundler和maven plugin會將這些檔案轉換為下方的最佳化AEM clientlib `ui.apps` 模組。

執行 [__完整堆疊__ Cloud Manager中的管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

前端資源會透過URI路徑(以 `/etc.clientlibs/`，通常會在AEM Dispatcher和CDN上快取。


>[!NOTE]
>
> 同樣地，在 __AEM快速網站建立歷程__, [前端更改](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) 會透過執行 __前端__ 管道，請參見 [設定管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### 檢閱WKND Sites專案中的Webpack設定 {#development-frontend-webpack-clientlib}

* 有三個 __webpack__ 用於捆綁WKND站點前端資源的配置檔案。

   1. `webpack.common`  — 這包含 __公用__ 配置，用於指導WKND資源捆綁和優化。 此 __輸出__ 屬性會指出要發出其建立之合併檔案(也稱為JavaScript套件組合，但不要與AEM OSGi套件組合混淆)的位置。 預設名稱設為 `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` 包含 __開發__ webpack-dev-serve的設定，並指向要使用的HTML範本。 它也包含執行於的AEM例項的Proxy設定 `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` 包含 __生產__ 設定，並使用外掛程式將開發檔案轉換為最佳化的套件組合。

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


* 套件資源會移至 `ui.apps` 模組使用 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 外掛程式，使用 `clientlib.config.js` 檔案。

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

* 此 __frontend-maven-plugin__ 從 `ui.frontend/pom.xml` 在AEM專案建置期間協調webpack整合和clientlib產生。

`$ mvn clean install -PautoInstallSinglePackage`

### 部署至AEMas a Cloud Service {#deployment-frontend-aemaacs}

此 [__完整堆疊__ 管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) 部署這些變更至AEMas a Cloud Service環境。


### 從AEMas a Cloud Service傳送 {#delivery-frontend-aemaacs}

透過完整堆疊管道部署的前端資源，會以 `/etc.clientlibs` 檔案。 您可以造訪 [公開托管的WKND站點](https://wknd.site/content/wknd/us/en.html) 以及網頁的瀏覽來源。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 恭喜！ {#congratulations}

恭喜，您已檢閱完整堆疊專案的ui.frontend模組

## 後續步驟 {#next-steps}

在下一章中， [更新專案以使用前端管道](update-project.md)，您將會更新AEM WKND Sites專案，以啟用前端管道合約。
