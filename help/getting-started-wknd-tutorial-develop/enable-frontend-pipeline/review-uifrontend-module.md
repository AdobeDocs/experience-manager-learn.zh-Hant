---
title: 檢閱全堆疊專案的 ui.frontend 模組
description: 檢閱基於 maven 的全堆疊 AEM Sites 專案的前端開發、部署和傳遞生命週期。
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
workflow-type: ht
source-wordcount: '560'
ht-degree: 100%

---

# 檢閱全堆疊 AEM 專案的「ui.frontend」模組 {#aem-full-stack-ui-frontent}

在本章中，我們將檢閱全堆疊 AEM 專案前端成品的開發、部署和傳遞，並著重於 __WKND Sites 專案__&#x200B;的「ui.frontend」模組。


## 目標 {#objective}

* 了解 AEM 全堆疊專案中前端成品的建置與部署流程
* 檢視 AEM 全堆疊專案中 `ui.frontend` 模組的 [webpack](https://webpack.js.org/) 設定檔
* AEM 用戶端程式庫 (也稱為 clientlibs) 的產生過程

## AEM 全堆疊和 Quick Site Creation 專案的前端部署流程

>[!IMPORTANT]
>
>此影片解釋並示範&#x200B;**全堆疊和 Quick Site Creation** 專案的前端流程，以便概述前端資源建置、部署和傳遞模型中的細微差別。

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 先決條件 {#prerequisites}


* 原地複製 [AEM WKND 網站專案](https://github.com/adobe/aem-guides-wknd)
* 已經在 AEM as a Cloud Service 中建置和部署原地複製的 AEM WKND 網站專案。

如需更多詳細資訊，請參閱 AEM WKND 網站專案的 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md)。

## AEM 全堆疊專案前端成品流程 {#flow-of-frontend-artifacts}

以下是全堆疊 AEM 專案中前端成品&#x200B;__開發、部署與傳遞__&#x200B;流程的簡要圖示。

![前端成品的開發、部署與傳遞](assets/Dev-Deploy-Delivery-AEM-Project.png)


在開發階段，更新 `ui.frontend/src/main/webpack` 資料夾中的 CSS、JS 檔案，即可進行樣式設定一類前端變更以及品牌重塑。然後，在建置期間，這些檔案經過 [webpack](https://webpack.js.org/) 模組套件工具和 maven 外掛程式最佳化後，會變成 AEM clientlibs 並放在 `ui.apps` 模組之下。

在 Cloud Manager [__中執行__&#x200B;全堆疊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html)管道時，前端變更便會部署到 AEM as a Cloud Service 環境。

前端資源會經由以 `/etc.clientlibs/` 開頭的 URI 路徑傳送到 Web 瀏覽器，並且通常會在 AEM Dispatcher 和內容傳遞網路上儲存快取。


>[!NOTE]
>
> 同樣地，在 __AEM Quick Site Creation 歷程__&#x200B;中，執行&#x200B;__前端__&#x200B;管道即可把[前端變更](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html)部署到 AEM as a Cloud Service 環境中，請參閱[設定您的管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### 檢閱 WKND 網站專案中的 webpack 設定檔 {#development-frontend-webpack-clientlib}

* 用來打包 WKND 網站前端資源的 __webpack__ 設定檔有三個。

   1. `webpack.common` - 這個設定檔包含&#x200B;__通用__&#x200B;設定，可指示 WKND 資源打包和最佳化。「__output__」屬性指示將其建立的合併檔案 (也稱為 JavaScript 套件，但不要與 AEM OSGi 套件混淆) 傳送到何處。預設名稱設為 `clientlib-site/js/[name].bundle.js`。

  ```javascript
      ...
      output: {
              filename: 'clientlib-site/js/[name].bundle.js',
              path: path.resolve(__dirname, 'dist')
          }
      ...    
  ```

   1. `webpack.dev.js` 包含 webpack-dev-serve 的&#x200B;__開發__&#x200B;設定並指向要使用的 HTML 範本。此設定檔還包含在 `localhost:4502`上執行的 AEM 實例的 Proxy 設定。

  ```javascript
      ...
      devServer: {
          proxy: [{
              context: ['/content', '/etc.clientlibs', '/libs'],
              target: 'http://localhost:4502',
          }],
      ...    
  ```

   1. `webpack.prod.js` 包含&#x200B;__生產__&#x200B;設定，並使用外掛程式將開發檔案轉換為最佳化的套件。

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


* 我們會使用 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 外掛程式將所打包的資源移動到 `ui.apps` 模組，而該外掛程式則是使用 `clientlib.config.js` 檔案中管理的設定。

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

* 來自 `ui.frontend/pom.xml` 的 __frontend-maven-plugin__ 在 AEM 專案建置期間，負責協調 webpack 打包與產生 clientlib 的過程。

`$ mvn clean install -PautoInstallSinglePackage`

### 部署至 AEM as a Cloud Service  {#deployment-frontend-aemaacs}

[__全堆疊__&#x200B;管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline)將這些變更部署到 AEM as a Cloud Service 環境。


### 從 AEM as a Cloud Service 傳遞 {#delivery-frontend-aemaacs}

透過全堆疊管道部署的前端資源會以 `/etc.clientlibs` 檔案的形式，從 AEM 網站傳遞到網頁瀏覽器。您可以瀏覽[公開託管的 WKND 網站](https://wknd.site/content/wknd/us/en.html)以及檢視網頁上的原始碼來驗證這件事。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 恭喜！ {#congratulations}

恭喜，您已檢閱全堆疊專案的 ui.frontend 模組

## 後續步驟 {#next-steps}

在下一章「[更新專案以使用前端管道](update-project.md)」，您將更新 AEM WKND 網站專案，使其能夠符合前端管道合約的要求。
