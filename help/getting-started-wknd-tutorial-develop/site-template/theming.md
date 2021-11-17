---
title: 主題工作流程 | AEM快速網站建立
description: 了解如何更新Adobe Experience Manager網站的主題來源，以套用品牌專屬的樣式。 了解如何使用Proxy伺服器檢視CSS和Javascript更新的即時預覽。 本教學課程也說明如何使用AEM Cloud Manager的前端管道，將主題更新部署至Adobe Site。
sub-product: sites
version: Cloud Service
type: Tutorial
feature: Core Components
topic: Content Management, Development
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 98946462-1536-45f9-94e2-9bc5d41902d4
source-git-commit: 04096fe3c99cdcce2d43b2b29899c2bbe37ac056
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---

# 主題工作流程 {#theming}

>[!CAUTION]
>
> 快速網站建立工具目前是技術預覽。 除非經Adobe支援同意，否則可供測試及評估之用，且非供生產使用。

在本章中，我們將更新Adobe Experience Manager網站的主題來源，以套用品牌特定樣式。 我們將學習如何使用代理伺服器，在針對即時網站編碼時，檢視CSS和Javascript更新的預覽。 本教學課程也說明如何使用AEM Cloud Manager的前端管道，將主題更新部署至Adobe Site。

最後，我們的網站會更新為加入與WKND品牌相符的樣式。

## 必備條件 {#prerequisites}

此為多部分教學課程，假設要執行 [頁面範本](./page-templates.md) 章節已完成。

## 目標

1. 了解如何下載和修改網站的主題來源。
1. 了解針對即時網站的程式碼，以便即時預覽。
1. 了解使用Analytics Cloud Manager的前端管道，將編譯的CSS和JavaScript更新傳送為主題一部分的端對端工作流程。

## 更新主題 {#theme-update}

接下來，對主題源進行更改，使網站與WKND品牌的外觀和風格相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

影片的高階步驟：

1. 在AEM中建立本機使用者，以與Proxy開發伺服器搭配使用。
1. 從AEM下載主題來源，並使用本機IDE（如VSCode）開啟。
1. 修改主題來源，並使用Proxy開發伺服器來即時預覽CSS和JavaScript變更。
1. 更新主題來源，使雜誌文章與WKND品牌風格和模型相符。

### 解決方案檔案

下載的已完成樣式 [WKND範例主題](assets/theming/WKND-THEME-src-1.1.zip)

## 使用前端管道部署主題 {#deploy-theme}

使用Cloud Manager的前端管道將主題更新部署至AEM環境。

>[!VIDEO](https://video.tv.adobe.com/v/338722/?quality=12&learn=on)

影片的高階步驟：

1. 建立新Git [Cloud Manager中的存放庫](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/managing-code/cloud-manager-repositories.html)
1. 將主題來源專案新增至Cloud Manager Git存放庫：

   ```shell
   $ cd <PATH_TO_THEME_SOURCES_FOLDER>
   $ git init -b main
   $ git add .
   $ git commit -m "initial commit"
   $ git remote add origin <CLOUD_MANAGER_GIT_REPOSITORY_URL>
   ```

1. 設定 [前端管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html) 部署前端程式碼。
1. 執行前端管道以將更新部署至目標AEM環境。

### 儲存範例

以下是幾個可作為參考的GitHub存放區範例：

* [aem-site-template-standard](https://github.com/adobe/aem-site-template-standard)
* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 作為「真實世界」專案的範例。

## 恭喜！ {#congratulations}

恭喜，您剛剛剛剛建立並部署主題至AEM!

### 後續步驟 {#next-steps}

深入探討AEM開發，並透過使用 [AEM專案原型](../project-archetype/overview.md).
