---
title: 主題工作流程
seo-title: AEM Sites快速入門 — 主題工作流程
description: 了解如何更新Adobe Experience Manager網站的主題來源，以套用品牌專屬的樣式。 了解如何使用Proxy伺服器檢視CSS和Javascript更新的即時預覽。 本教學課程也說明如何使用GitHub動作將主題更新部署至AEM Site。
sub-product: Sites
version: Cloud Service
type: Tutorial
feature: 核心元件
topic: 內容管理、開發
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# 主題工作流程 {#theming}

>[!CAUTION]
>
> 此處的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽。

在本章中，我們將更新Adobe Experience Manager網站的主題來源，以套用品牌特定樣式。 我們將學習如何使用代理伺服器，在針對即時網站編碼時，檢視CSS和Javascript更新的預覽。 本教學課程也說明如何使用GitHub動作將主題更新部署至AEM Site。

最後，我們的網站會更新為加入與WKND品牌相符的樣式。

## 必備條件 {#prerequisites}

這是多部分教學課程，假設已完成[頁面範本](./page-templates.md)章節中概述的步驟。

## 目標

1. 了解如何下載和修改網站的主題來源。
1. 了解針對即時網站的程式碼，以便即時預覽。
1. 了解使用GitHub動作，將編譯的CSS和JavaScript更新隨主題一併傳送的端對端工作流程。

## 更新主題 {#theme-update}

接下來，對主題源進行更改，使網站與WKND品牌的外觀和風格相匹配。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

影片的高階步驟：

1. 在AEM中建立本機使用者，以與Proxy開發伺服器搭配使用。
1. 從AEM下載主題來源，並使用本機IDE（如VSCode）開啟。
1. 修改主題來源，並使用Proxy開發伺服器來即時預覽CSS和JavaScript變更。
1. 更新主題來源，使雜誌文章與WKND品牌風格和模型相符。

### 解決方案檔案

下載[WKND主題](assets/theming/WKND-THEME-src.zip)的已完成樣式

## 部署主題 {#deploy-theme}

使用GitHub動作將主題更新部署至AEM環境。

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

影片的高階步驟：

1. 將主題來源專案新增至[GitHub，作為新存放庫](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)。
1. 在GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)中建立[個人存取權杖，並儲存至安全位置。
1. 在您的AEM環境中配置GitHub設定，以指向您的GitHub存放庫並加入您的個人存取權杖。
1. 在您的GitHub存放庫中建立下列[加密機密](https://docs.github.com/en/actions/reference/encrypted-secrets):
   * **AEM_SITE**  - AEM網站的根(即 `wknd`)
   * **AEM_URL**  — 您AEM製作環境的url
   * **GIT_TOKEN**  — 步驟2中的個人存取Token。
1. 執行GitHub動作：**建立並部署Github成品**。 這會產生執行動作的下游效果：**使用工件id**&#x200B;更新AEM上的主題配置，該工件id將按`AEM_URL`和`AEM_SITE`的指定將主題源部署到AEM環境。

### 儲存範例

以下是幾個可作為參考的GitHub存放區範例：

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e)  — 用作「實際」專案的範例。
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme)  — 以下教學課程的範例使用。

## 恭喜！ {#congratulations}

恭喜，您剛剛剛剛建立並部署主題至AEM!

### 後續步驟 {#next-steps}

使用[AEM專案原型](../project-archetype/overview.md)建立網站，深入探討AEM開發，深入了解基礎技術。
