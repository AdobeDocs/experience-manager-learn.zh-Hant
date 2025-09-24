---
title: 主題工作流程 | AEM Quick Site Creation
description: 了解如何更新 Adobe Experience Manager Site 的主題來源以套用品牌專屬風格。了解如何使用 Proxy 伺服器檢視 CSS 和 Javascript 更新的即時預覽。本教學課程也將介紹如何使用 Adobe Cloud Manager 的前端管道，將主題更新部署到 AEM 網站。
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-7498
thumbnail: KT-7498.jpg
doc-type: Tutorial
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
duration: 1275
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '458'
ht-degree: 100%

---

# 主題工作流程 {#theming}

在本章中，我們會更新 Adobe Experience Manager Site 的主題來源以套用品牌專屬風格。我們會了解如何使用 Proxy 伺服器在針對即時網站進行編碼時檢視 CSS 和 Javascript 更新的預覽。本教學課程也將介紹如何使用 Adobe Cloud Manager 的前端管道，將主題更新部署到 AEM 網站。

最後，我們更新網站以便包括與 WKND 品牌相符的風格。

## 先決條件 {#prerequisites}

這是包含多個部分的教學課程，並假設您已經完成「[頁面範本](./page-templates.md)」章節所述的步驟。

## 目標

1. 了解如何下載和修改網站的主題來源。
1. 了解如何針對即時網站編寫程式碼以進行即時預覽。
1. 了解使用 Adobe Cloud Manager 的前端管道，將編譯的 CSS 和 JavaScript 更新作為主題的一部分傳遞出去的端到端工作流程。

## 更新主題 {#theme-update}

接下來，對主題來源進行變更，以便網站與 WKND 品牌的外觀相符。

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

影片的概括性步驟：

1. 在 AEM 中建立本機使用者以便搭配 Proxy 開發伺服器使用。
1. 從 AEM 下載主題來源，並使用本機 IDE (如 VSCode) 開啟。
1. 修改主題來源並使用 Proxy 開發伺服器來即時預覽 CSS 和 JavaScript 的變更。
1. 更新主題來源，以便雜誌文章與 WKND 品牌風格和模型相符。

### 解決方案檔案

下載 [WKND 範例主題](assets/theming/WKND-THEME-src-1.1.zip)的完成版樣式

## 使用前端管道部署主題 {#deploy-theme}

使用 Cloud Manager 前端管道將主題更新部署到 AEM 環境。

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

影片的概括性步驟：

1. 在 Cloud Manager 中建立一個新的 git [存放庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html?lang=zh-Hant)
1. 將您的主題來源專案新增至 Cloud Manager git 存放庫：

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 在 Cloud Manager 中設定[前端管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?lang=zh-Hant)，以便部署前端程式碼。
1. 執行前端管道以便將更新部署到目標 AEM 環境。

### 範例存放庫

有幾個範例 GitHub 存放庫可以用作參考：

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) - 用作「現實世界」專案的範例。

## 恭喜！ {#congratulations}

恭喜，您剛剛已更新並部署一個主題到 AEM！

### 後續步驟 {#next-steps}

使用 [AEM 專案原型](../project-archetype/overview.md)來建立一個網站，藉此深入探討 AEM 開發，並加深對其基礎技術的了解。
