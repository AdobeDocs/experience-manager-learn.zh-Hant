---
title: 更新全堆疊AEM專案以使用前端管道
description: 了解如何更新完整堆疊的AEM專案，以針對前端管道啟用，因此只會建置和部署前端成品。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: 7c246c4f1af9dfe599485f68508c66fe29d2f0b6
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# 更新全堆疊AEM專案以使用前端管道 {#update-project-enable-frontend-pipeline}

在本章中，我們會對 __WKND Sites專案__ 使用前端管道來部署JavaScript和CSS，而不需要執行完整的完整堆疊管道。 這可以解耦前端和後端成品的開發和部署生命週期，從而實現更快速、更迭代的整體開發過程。

## 目標 {#objectives}

* 更新全堆棧項目以使用前端管道

## 完整堆疊AEM專案中組態變更概觀

>[!VIDEO](https://video.tv.adobe.com/v/3409419/)

## 必備條件 {#prerequisites}

此為多部分教學課程，假設您已檢閱 [&#39;ui.frontend&#39;模組](./review-uifrontend-module.md).


## 完整堆疊AEM專案的變更

為測試運行部署有三項與項目相關的配置更改和樣式更改，因此WKND項目中總共有四項特定更改，以便用於前端管道合同。

1. 移除 `ui.frontend` 來自完整堆棧構建週期的模組

   * 在中， WKND Sites專案的根目錄 `pom.xml` 註解 `<module>ui.frontend</module>` 子模組項目。

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * 和注釋相關依賴來自 `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. 準備 `ui.frontend` 前端管道合約的模組，新增兩個新的webpack設定檔案。

   * 複製現有 `webpack.common.js` as `webpack.theme.common.js`，和變更 `output` 屬性與 `MiniCssExtractPlugin`, `CopyWebpackPlugin` 外掛程式設定參數如下：

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * 複製現有 `webpack.prod.js` as `webpack.theme.prod.js`，並變更 `common` 變數在上述檔案中的位置為

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >上述兩項「webpack」配置更改將具有不同的輸出檔案和資料夾名稱，因此我們可以輕鬆區分clientlib（完整堆棧）和主題生成（前端）管道前端對象。
   >
   >如您所猜，您也可以略過上述變更，以使用現有的Webpack設定，但需要進行下列變更。
   >
   >這取決於您想如何命名或組織它們。


   * 在 `package.json` 檔案，確定，  `name` 屬性值與 `/conf` 節點。 在 `scripts` 屬性， a `build` 指示如何從此模組建立前端檔案的指令碼。

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. 準備 `ui.content` 模組，借由新增兩個Sling設定。

   * 在建立檔案 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`  — 這包括 `ui.frontend` 模組在 `dist` 資料夾。

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    請參閱完成 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 在 __AEM WKND Sites專案__.


   * 第二 `com.adobe.aem.wcm.site.manager.config.SiteConfig` 和 `themePackageName` 值與 `package.json` 和 `name` 屬性值和 `siteTemplatePath` 指向 `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` 存根路徑值。

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    請參閱，完成 [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 在 __AEM WKND Sites專案__.

1. 主題或樣式會隨之變更，而透過前端管道進行部署以進行測試執行時，我們會變更 `text-color` Adobe為紅色（或您可以自行選取），方法是更新 `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

最後，將這些變更推送至您方案的AdobeGit存放庫。


>[!AVAILABILITY]
>
> 這些變更可在內部的GitHub上取得 [__前端管道__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 分支 __AEM WKND Sites專案__.


## 注意 —  _啟用前端管道_ 按鈕

此 [邊欄選取器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) &#39;s [網站](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 選項顯示 **啟用前端管道** 按鈕。 按一下 **啟用前端管道** 按鈕將覆寫上述內容 **Sling設定**，確定 **您未按** 透過Cloud Manager管道執行部署上述變更後，此按鈕會顯示。

![啟用前端管道按鈕](assets/enable-front-end-Pipeline-button.png)

如果誤點了管道，則必須重新運行管道，以確保前端管道合同和更改被恢復。

## 恭喜！ {#congratulations}

恭喜，您已更新WKND Sites專案，以啟用前端管道合約。

## 後續步驟 {#next-steps}

在下一章中， [使用前端管道進行部署](create-frontend-pipeline.md)，您將建立並運行前端管道，並驗證我們 __搬走__ 從「/etc.clientlibs」型前端資源傳送。
