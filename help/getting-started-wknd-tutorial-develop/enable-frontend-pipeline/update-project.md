---
title: 更新完整棧疊AEM專案以使用前端管道
description: 瞭解如何更新完整棧疊AEM專案以將其啟用用於前端管道，因此它只會建置和部署前端成品。
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
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
duration: 307
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---

# 更新完整棧疊AEM專案以使用前端管道 {#update-project-enable-frontend-pipeline}

在本章中，我們會對&#x200B;__WKND Sites專案__&#x200B;進行設定變更，以使用前端管道來部署JavaScript和CSS，而不是要求完全的完整棧疊管道執行。 這會分離前端和後端成品的開發和部署生命週期，讓整體開發流程更快速、反覆無常。

## 目標 {#objectives}

* 更新完整棧疊專案以使用前端管道

## 完整棧疊AEM專案中的設定變更概觀

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## 先決條件 {#prerequisites}

此教學課程包含多個部分，並假設您已檢閱[&#39;ui.frontend&#39;模組](./review-uifrontend-module.md)。


## 完整棧疊AEM專案的變更

有三個與專案相關的設定變更和一個要針對測試回合部署的樣式變更，因此在WKND專案中總共有四個特定變更，以便為前端管道合約啟用它。

1. 從完整棧疊建置週期移除`ui.frontend`模組

   * 在中，WKND Sites專案的根`pom.xml`註解`<module>ui.frontend</module>`子模組專案。

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

   * 和來自`ui.apps/pom.xml`的註解相關相依性

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

1. 新增兩個新的Webpack設定檔，為前端管道合約準備`ui.frontend`模組。

   * 將現有的`webpack.common.js`復製為`webpack.theme.common.js`，並變更`output`屬性和`MiniCssExtractPlugin`、`CopyWebpackPlugin`外掛程式設定引數，如下所示：

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

   * 將現有的`webpack.prod.js`復製為`webpack.theme.prod.js`，並將`common`變數的位置變更為上述檔案為

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >上述兩個「webpack」設定變更是具有不同的輸出檔案和資料夾名稱，因此我們可以輕鬆區分clientlib （完整棧疊）和產生的主題（前端）管道前端成品。
   >
   >如您所知，上述變更也可以略過以使用現有的Webpack設定，但需要以下變更。
   >
   >您可以自行命名或組織這些欄位。


   * 在`package.json`檔案中，確定`name`屬性值與`/conf`節點的網站名稱相同。 在`scripts`屬性下，指示如何從此模組建置前端檔案的`build`指令碼。

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

1. 透過新增兩個Sling設定，為前端管道準備`ui.content`模組。

   * 在`com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`建立檔案 — 這包含`ui.frontend`模組使用webpack建置程式在`dist`資料夾下產生的所有前端檔案。

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
   >    在&#x200B;__AEM WKND Sites專案__&#x200B;中檢視完整的[HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml)。


   * 接著`com.adobe.aem.wcm.site.manager.config.SiteConfig`的`themePackageName`值與`package.json`和`name`屬性值相同，且`siteTemplatePath`指向`/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0`存根路徑值。

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
   >    檢視&#x200B;__AEM WKND Sites專案__&#x200B;中的完整[SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml)。

1. 主題或樣式變更成透過測試回合的前端管道部署，我們正在更新`ui.frontend/src/main/webpack/base/sass/_variables.scss`以將`text-color`變更為Adobe紅（或者您也可以選擇自己的顏色）。

   ```css
       $black:     #a40606;
       ...
   ```

最後，將這些變更推送至您程式的Adobe Git存放庫。


>[!AVAILABILITY]
>
> 這些變更可在&#x200B;__AEM WKND Sites專案__&#x200B;的&#x200B;[__前端管道__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline)分支內的GitHub上取得。


## 警告 — _啟用前端管道_&#x200B;按鈕

[邊欄選取器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html)的[站台](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html)選項會在選取您的站台根或站台頁面時顯示&#x200B;**啟用前端管道**&#x200B;按鈕。 按一下&#x200B;**啟用前端管道**&#x200B;按鈕將會覆寫上述&#x200B;**Sling設定**，請確定&#x200B;**在透過Cloud Manager管道執行部署上述變更後，您沒有按一下**&#x200B;此按鈕。

![啟用前端管道按鈕](assets/enable-front-end-Pipeline-button.png)

如果錯誤地點選它，您必須重新執行管道以確保前端管道合約和變更恢復。

## 恭喜！ {#congratulations}

恭喜，您已更新WKND Sites專案，以便為前端管道合約啟用它。

## 後續步驟 {#next-steps}

在下一章[使用前端管道部署](create-frontend-pipeline.md)中，您將建立和執行前端管道，並驗證我們如何&#x200B;__離開__&#x200B;基於「/etc.clientlibs」的前端資源傳遞。
