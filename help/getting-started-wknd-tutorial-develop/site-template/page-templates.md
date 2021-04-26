---
title: 頁面範本
seo-title: 開始使用AEM Sites-頁面範本
description: 瞭解如何建立和修改頁面範本。 瞭解頁面範本與頁面之間的關係。 瞭解如何設定頁面範本的原則，為內容提供精細的治理和品牌一致性。  將會根據Adobe XD的模型，建立結構精良的雜誌文章範本。
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: 內容管理
feature: 核心元件、可編輯範本、頁面編輯器
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 0%

---


# 頁面範本{#page-templates}

>[!CAUTION]
>
> 此處展示的快速網站建立功能將於2021年下半年推出。 相關檔案可供預覽使用。

在本章中，我們將探討頁面範本與頁面之間的關係。 我們將根據[AdobeXD](https://www.adobe.com/products/xd.html)中的部分模型，建立未設定樣式的雜誌文章範本。 在建立範本的過程中，會涵蓋核心元件和進階政策組態。

## 必備條件 {#prerequisites}

本教學課程包含多部分，假設[作者內容和發佈變更](./author-content-publish.md)章節中概述的步驟已完成。

## 目標

1. Inspect在Adobe XD建立的頁面設計，並對應至核心元件。
1. 瞭解頁面範本的詳細資訊，以及如何使用原則來強制精細控制頁面內容。
1. 瞭解範本和頁面的連結方式。

## 您將建立的{#what-you-will-build}

在本教學課程中，您將建立新的雜誌文章頁面範本，可用來建立新的雜誌文章，並與一般結構對齊。 範本將以AdobeXD中的設計和UI Kit為基礎。 本章僅針對構建模板的結構或骨架進行介紹。 不會實作任何樣式，但範本和頁面都能運作。

## 使用Adobe XD{#adobexd}進行UI規劃

在大多數情況下，規劃新網站都從模型和靜態設計開始。 [Adobe](https://www.adobe.com/products/xd.html) XD是建立使用體驗的設計工具。接下來，我們將檢查UI套件和模型，以協助規劃「文章頁面範本」的結構。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**下載 [WKND文章設計檔案](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> [核心元AEM件UI套件也提供](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)做為自訂專案的起點。

## 建立雜誌文章頁面範本

建立頁面時，您必須選取範本，此範本將用作建立新頁面的基礎。 範本會定義產生頁面的結構、初始內容和允許的元件。

[頁面範本](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html)有3個主要區域：

1. **結構** -定義屬於模板一部分的元件。內容作者將無法編輯這些內容。
1. **初始內容** -定義範本將開頭的元件，內容作者可編輯和／或刪除這些元件
1. **Policys**  —— 定義元件的行為方式以及作者將提供哪些選項的配置。

接著，在中建立符合AEM模型結構的新範本。 這將發生在的本地實例AEM中。 請依照下列視訊中的步驟進行：

>[!VIDEO](https://video.tv.adobe.com/v/332915/?quality=12&learn=on)

### 解決方案套件

您可以透過Package Manager下載並安裝Magazine Template](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)的完成解決方案。[

## 使用體驗片段{#experience-fragments}更新頁首和頁尾

建立全域內容（例如頁首或頁尾）時的常見做法是使用「體驗片段」[。 ](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)體驗片段，可讓使用者結合多個元件，以建立單一、可參考的元件。 體驗片段的優點是支援多網站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)。

網站範本會產生頁首和頁尾。 接著，更新「體驗片段」以符合模型。 請依照下列視訊中的步驟進行：

>[!VIDEO](https://video.tv.adobe.com/v/332916/?quality=12&learn=on)

以下視訊的高階步驟：

1. 下載範例內容套件&#x200B;**[WKND-Starter-Assets-Skate-Article-1.0.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.0.zip)**。
1. 使用「套件管理員」上傳及安裝內容套件。
1. 更新頁首和頁尾體驗片段以使用WKND標誌

## 建立雜誌文章頁面

接著，使用「雜誌文章頁面」範本建立新頁面。 製作頁面內容以符合網站模型。 請依照下列視訊中的步驟進行：

>[!VIDEO](https://video.tv.adobe.com/v/332917/?quality=12&learn=on)

## 恭喜！{#congratulations}

恭喜，您剛和Adobe Experience Manager Sites建立了新範本和頁面。

### 後續步驟{#next-steps}

目前雜誌文章頁面和網站不符合WKND的品牌樣式。 請依照[Theming](theming.md)教學課程，瞭解更新CSS和Javascript前端程式碼的最佳範例，以套用全域樣式至網站。

### 解決方案套件

本章的解決方案套件可供下載：[WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)。
