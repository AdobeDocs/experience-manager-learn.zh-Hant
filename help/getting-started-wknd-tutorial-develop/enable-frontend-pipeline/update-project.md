---
title: 更新全堆棧AEM項目以使用前端管線
description: 瞭解如何更新全堆棧項AEM目，以便為前端管道啟用它，以便它只生成和部署前端對象。
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
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# 更新全堆棧AEM項目以使用前端管線 {#update-project-enable-frontend-pipeline}

在本章中，我們將對 __WKND站點項目__ 使用前端管線來部署JavaScript和CSS，而不是要求執行完整的完整堆棧管線。 這可以使前端和後端偽像的開發和部署生命週期解耦，從而實現更快速、更迭的整體開發過程。

## 目標 {#objectives}

* 更新全堆棧項目以使用前端管線

## 完整堆棧項目中配置更改概AEM述

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## 必備條件 {#prerequisites}

這是一個多部分教程，假定您已查看 [「ui.frontend」模組](./review-uifrontend-module.md)。


## 對全堆棧項目的更AEM改

為test運行部署有三個與項目相關的配置更改和樣式更改，因此WKND項目中總共有四個特定更改，以使其適用於前端管道合同。

1. 刪除 `ui.frontend` 從全堆棧生成週期生成的模組

   * 在中，WKND站點項目的根 `pom.xml` 注釋 `<module>ui.frontend</module>` 子模組條目。

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

   * 和來自的注釋相關依賴項 `ui.apps/pom.xml`

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

1. 準備 `ui.frontend` 用於前端管道合同的模組，方法是添加兩個新的webpack配置檔案。

   * 複製現有 `webpack.common.js` 如 `webpack.theme.common.js`，更改 `output` 物業及 `MiniCssExtractPlugin`。 `CopyWebpackPlugin` 插件配置參數如下：

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

   * 複製現有 `webpack.prod.js` 如 `webpack.theme.prod.js`，並更改 `common` 變數到上述檔案的位置

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >以上兩個「webpack」配置更改的輸出檔案和資料夾名稱不同，因此我們可以輕鬆地區分客戶端庫（完整堆棧）和主題生成（前端）管道前端對象。
   >
   >如您所猜測，也可以跳過上述更改以使用現有Webpack配置，但需要進行以下更改。
   >
   >你要如何命名或組織它們，取決於你。


   * 在 `package.json` 檔案，確保  `name` 屬性值與 `/conf` 的下界。 在 `scripts` 屬性a `build` 指令碼指示如何從此模組構建前端檔案。

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

1. 準備 `ui.content` 通過添加兩個Sling配置實現前端管線模組。

   * 在以下位置建立檔案 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`  — 這包括所有 `ui.frontend` 模組在 `dist` 資料夾。

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
   >    查看完整 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 的 __WKNDAEM站點項目__。


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
   >    請參閱，完整 [站點配置](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 的 __WKNDAEM站點項目__。

1. 通過前端管道進行部署以進行test運行時，主題或樣式會發生更改，我們正在更改 `text-color` Adobe為紅色（或者您可以自行選擇） `ui.frontend/src/main/webpack/base/sass/_variables.scss`。

   ```css
       $black:     #a40606;
       ...
   ```

最後，將這些更改推送到程式的AdobeGit儲存庫。


>[!AVAILABILITY]
>
> 這些更改可在GitHub中 [__前端管道__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 分支 __WKNDAEM站點項目__。


## 警告 —  _啟用前端管線_ 按鈕

的 [軌道選擇器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) `s [站點](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 選項 **啟用前端管線** 按鈕。 按一下 **啟用前端管線** 按鈕將覆蓋上面的內容 **吊具配置**，確保 **不按一下** 通過Cloud Manager管道執行部署上述更改後，此按鈕。

![「啟用前端管道」按鈕](assets/enable-front-end-Pipeline-button.png)

如果錯誤地按一下了它，則必須重新運行管道以確保恢復前端管道合同和更改。

## 恭喜！ {#congratulations}

祝賀您，您已更新WKND站點項目，以啟用該項目，以滿足前端管道合同。

## 後續步驟 {#next-steps}

在下一章， [使用前端管道部署](create-frontend-pipeline.md)，您將建立並運行前端管線，並驗證我們 __搬走__ 基於「/etc.clientlibs」的前端資源交付。
