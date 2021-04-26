---
title: 主題工作流程
seo-title: 開始使用AEM Sites- Theming工作流程
description: 瞭解如何更新Adobe Experience Manager網站的主題來源以套用品牌特定的樣式。 瞭解如何使用Proxy伺服器來檢視CSS和Javascript更新的即時預覽。 本教學課程也將涵蓋如何使用GitHub動作將主題AEM更新部署至網站。
sub-product: Sites
version: Cloud Service
type: Tutorial
feature: 核心元件
topic: 內容管理、開發
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---


# 主題工作流程{#theming}

>[!CAUTION]
>
> 此處展示的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽使用。

在本章中，我們將更新Adobe Experience Manager網站的主題來源，以套用品牌特定樣式。 我們將學習如何使用Proxy伺服器來檢視CSS和Javascript更新的預覽，以針對即時網站編碼。 本教學課程也將涵蓋如何使用GitHub動作將主題AEM更新部署至網站。

最後，我們的網站將會更新，加入符合WKND品牌的樣式。

## 必備條件 {#prerequisites}

本教學課程包含多部分，假設[頁面範本](./page-templates.md)章節中概述的步驟已完成。

## 目標

1. 瞭解如何下載和修改網站的主題來源。
1. 瞭解如何針對即時網站編寫程式碼，以便即時預覽。
1. 瞭解使用GitHub動作，將編譯的CSS和JavaScript更新傳送至主題的端對端工作流程。

## 更新主題{#theme-update}

接著，變更主題來源，讓網站符合WKND品牌的外觀和感覺。

>[!VIDEO](https://video.tv.adobe.com/v/332918/?quality=12&learn=on)

影片的高階步驟：

1. 建立本機使用AEM者，以便與Proxy開發伺服器搭配使用。
1. 從本機IDE（如VSCode）下AEM載主題來源並開啟。
1. 修改主題來源並使用proxy開發伺服器即時預覽CSS和JavaScript變更。
1. 更新主題來源，讓雜誌文章符合WKND品牌樣式和模型。

### 解決方案檔案

下載[WKND Theme](assets/theming/WKND-THEME-src.zip)的完成樣式

## 部署主題{#deploy-theme}

使用GitHub動作將主題的更AEM新部署至環境。

>[!VIDEO](https://video.tv.adobe.com/v/332919/?quality=12&learn=on)

影片的高階步驟：

1. 將主題源項目作為新儲存庫](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)添加到[GitHub。
1. 在GitHub](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)中建立[個人存取Token並儲存至安全位置。
1. 在您的環境中配置GitHub設AEM定，以指向您的GitHub儲存庫並包含您的個人存取Token。
1. 在GitHub資料庫中建立下列[加密機密](https://docs.github.com/en/actions/reference/encrypted-secrets):
   * **AEM_SITE** -您網站AEM的根目錄(亦即 `wknd`)
   * **AEM_URL** - AEM Author環境的url
   * **GIT_TOKEN**  —— 步驟2中的個人存取Token。
1. 執行GitHub動作：**建立和部署Github工件**。 這將產生執行動作的下游效果：**使用工件ID**&#x200B;更新主題配置AEM，該工具將主題源部署到AEM`AEM_URL`和`AEM_SITE`指定的環境。

### 重複範例

以下是幾個GitHub重複回覆範例，可做為參考：

* [aem-site-template-basic-theme-e2e](https://github.com/adobe/aem-site-template-basic-theme-e2e) -用作「實際」專案的範例。
* [https://github.com/godanny86/wknd-theme](https://github.com/godanny86/wknd-theme) -用作教學課程後的範例。

## 恭喜！{#congratulations}

恭喜您，您剛剛建立並部署主題至AEM!

### 後續步驟{#next-steps}

使用[Project Archetype&lt;a1/AEM>建立網站，深入瞭解開發並進一步瞭解基礎技術。](../project-archetype/overview.md)
