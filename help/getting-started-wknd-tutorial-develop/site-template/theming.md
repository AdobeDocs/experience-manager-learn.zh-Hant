---
title: 主題工作流 |快AEM速站點建立
description: 瞭解如何更新Adobe Experience Manager網站的主題源以應用品牌特定樣式。 瞭解如何使用代理伺服器查看CSS和Javascript更新的即時預覽。 本教程還將介紹如何使用Adobe雲管理AEM器的前端管道將主題更新部署到站點。
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 0%

---

# 主題工作流 {#theming}

在本章中，我們更新Adobe Experience Manager網站的主題來源以應用特定品牌的樣式。 我們將學習如何使用代理伺服器在對活動站點編碼時查看CSS和Javascript更新的預覽。 本教程還將介紹如何使用Adobe雲管理AEM器的前端管道將主題更新部署到站點。

最後，我們的網站將更新為包含與WKND品牌相匹配的樣式。

## 必備條件 {#prerequisites}

這是一個多部分教程，並假定在 [頁面模板](./page-templates.md) 一章已經完成。

## 目標

1. 瞭解如何下載和修改網站的主題源。
1. 瞭解如何針對即時預覽的即時站點編碼。
1. 瞭解使用AdobeCloud Manager的前端管道將編譯的CSS和JavaScript更新作為主題的一部分提供的端到端工作流。

## 更新主題 {#theme-update}

接下來，對主題源進行更改，以便網站與WKND品牌的外觀和感覺相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/332918?quality=12&learn=on)

視頻的高級步驟：

1. 建立本地用戶AEM以與代理開發伺服器一起使用。
1. 從本地IDE（如VSCode）AEM下載主題源並使用其開啟。
1. 修改主題源並使用代理開發伺服器即時預覽CSS和JavaScript更改。
1. 更新主題源，以便雜誌文章與WKND品牌樣式和模型相匹配。

### 解決方案檔案

下載已完成的樣式 [WKND示例主題](assets/theming/WKND-THEME-src-1.1.zip)

## 使用前端管道部署主題 {#deploy-theme}

使用Cloud Manager的前端管AEM道將主題更新部署到環境。

>[!VIDEO](https://video.tv.adobe.com/v/338722?quality=12&learn=on)

視頻的高級步驟：

1. 建立新Git [Cloud Manager中的儲存庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. 將主題源項目添加到Cloud Manager Git儲存庫：

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 配置 [前端管線](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) 部署前端代碼。
1. 運行前端管道以將更新部署到目標環AEM境。

### 回零示例

有幾個示例GitHub回購可以用作引用：

* [aem站點模板標準](https://github.com/adobe/aem-site-template-standard)
* [aem站點模板基本主題e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 用作&quot;真實世界&quot;項目的實例。

## 恭喜！ {#congratulations}

祝賀您，您剛剛更新並部署了主題AEM!

### 後續步驟 {#next-steps}

通過建立一個站點，AEM深入瞭解開發和瞭解底層技術 [項AEM目原型](../project-archetype/overview.md)。
