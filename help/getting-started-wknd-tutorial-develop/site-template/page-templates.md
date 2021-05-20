---
title: 頁面範本
seo-title: AEM Sites快速入門 — 頁面範本
description: 了解如何建立和修改頁面範本。 了解頁面範本和頁面之間的關係。 了解如何設定頁面範本的原則，以提供精細的控管和內容的品牌一致性。  系統會根據Adobe XD的模型，建立結構良好的雜誌文章範本。
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: 內容管理
feature: 核心元件、可編輯的範本、頁面編輯器
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 0%

---


# 頁面模板{#page-templates}

>[!CAUTION]
>
> 此處的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽。

在本章中，我們將探討頁面範本與頁面之間的關係。 我們會根據[AdobeXD](https://www.adobe.com/products/xd.html)中的某些模型，建立未設定樣式的雜誌文章範本。 在建立範本的過程中，會說明核心元件和進階政策設定。

## 必備條件 {#prerequisites}

這是多部分教學課程，假設已完成[製作內容和發佈變更](./author-content-publish.md)章節中概述的步驟。

## 目標

1. Inspect是在Adobe XD中建立的頁面設計，並對應至核心元件。
1. 了解頁面範本的詳細資訊，以及如何使用原則來強制精細控制頁面內容。
1. 了解範本和頁面的連結方式。

## 您要建立的{#what-you-will-build}

在本教學課程中，您將會建立新的雜誌文章頁面範本，以用來建立新雜誌文章，並與通用結構對齊。 範本將以AdobeXD中製作的設計和UI Kit為基礎。 本章僅側重於構建模板的結構或骨架。 不會實作樣式，但範本和頁面將可運作。

## 使用Adobe XD規劃UI {#adobexd}

在大多數情況下，規劃新網站的開始會是模型和靜態設計。 [Adobe](https://www.adobe.com/products/xd.html) XD是建立使用者體驗的設計工具。接下來，我們將檢查UI套件和模型，以協助規劃文章頁面範本的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**下載WKND [文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)也提供通用[AEM核心元件UI套件作為自訂專案的起點。

## 建立雜誌文章頁面範本

建立頁面時，您必須選取範本，以作為建立新頁面的基礎。 範本會定義產生的頁面、初始內容及允許的元件結構。

[頁面範本](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html)有3個主要區域：

1. **結構**  — 定義屬於範本一部分的元件。內容作者將無法編輯這些內容。
1. **初始內容**  — 定義範本要開頭的元件，內容作者可以編輯和/或刪除這些元件
1. **原則**  — 定義元件行為設定，以及作者可使用的選項。

接下來，在AEM中建立符合模型結構的新範本。 這會發生在AEM的本機例項中。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### 解決方案套件

已完成的[雜誌模板解決方案](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)可通過包管理器下載和安裝。

## 使用體驗片段{#experience-fragments}更新頁首和頁尾

建立全域內容（例如頁首或頁尾）時，常見的作法是使用[體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 體驗片段，可讓使用者結合多個元件，以建立單一、可參考的元件。 體驗片段的優點是支援多網站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)。

網站範本會產生「頁首」和「頁尾」。 接下來，更新體驗片段以符合模型。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

以下是影片的高階步驟：

1. 下載範例內容套件&#x200B;**[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**。
1. 使用套件管理器上傳並安裝內容套件。
1. 更新頁首與頁尾體驗片段以使用WKND標誌

## 建立雜誌文章頁面

接下來，使用「雜誌文章頁面」範本建立新頁面。 製作頁面內容以符合網站模型。 請依照以下影片中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## 恭喜！ {#congratulations}

恭喜，您剛使用Adobe Experience Manager Sites建立新範本和頁面。

### 後續步驟{#next-steps}

此時，雜誌文章頁面和網站不符合WKND的品牌樣式。 請依照[Theming](theming.md)教學課程，了解更新用於將全域樣式套用至網站的CSS和Javascript前端程式碼的最佳實務。

### 解決方案套件

可下載本章的解決方案包：[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)。
