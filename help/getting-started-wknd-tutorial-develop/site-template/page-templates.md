---
title: 頁面模板
description: 瞭解如何建立和修改頁面模板。 瞭解頁面模板與頁面之間的關係。 瞭解如何配置頁面模板的策略以為內容提供精細的治理和品牌一致性。  基於Adobe XD的模型，建立了結構精良的雜誌文章模板。
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Editable Templates, Page Editor
role: Developer
level: Beginner
kt: 7498
thumbnail: KT-7498.jpg
exl-id: 261ec68f-36f4-474f-a6e4-7a2f9cea691b
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 0%

---

# 頁面模板 {#page-templates}

在本章中，我們將探討頁面模板和頁面之間的關係。 我們將根據一些來自 [AdobeXD](https://www.adobe.com/products/xd.html)。 在構建模板的過程中，將介紹核心元件和高級策略配置。

## 必備條件 {#prerequisites}

這是一個多部分教程，並假定在 [編寫內容並發佈更改](./author-content-publish.md) 一章已經完成。

## 目標

1. 瞭解頁面模板的詳細資訊以及如何使用策略來實施對頁面內容的精細控制。
1. 瞭解模板和頁面的連結方式。
1. 建立新模板並建立頁面。

## 您將構建的 {#what-you-will-build}

在本教程的這一部分中，您將構建一個新的雜誌文章頁面模板，該模板可用於建立新雜誌文章並與通用結構對齊。 該模板基於AdobeXD中生成的設計和UI工具包。 本章僅側重於構建模板的結構或骨架。 未實現樣式，但模板和頁面功能正常。

## 建立雜誌文章頁面模板

建立頁面時，必須選擇模板，該模板將用作建立新頁面的基礎。 模板定義結果頁面的結構、初始內容和允許的元件。

有3個主要區域 [頁面模板](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/templates.html):

1. **結構**  — 定義作為模板一部分的元件。 這些內容作者不可編輯。
1. **初始內容**  — 定義模板開始的元件，內容作者可編輯和/或刪除這些元件
1. **策略**  — 定義元件行為方式的配置以及作者將提供哪些選項。

接下來，在與模AEM型結構匹配的新模板中。 這將在的本地實例中發AEM生。 按照以下視頻中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332915?quality=12&learn=on)

您可以使用以下縮略圖來標識模板（或上載您自己的模板！）

![文章頁面模板縮略圖](./assets/page-templates/article-page-template-thumbnail.png)


### 解決方案包

已完成 [雜誌模板的解決方案](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.1.zip) 可以通過包管理器下載和安裝。

## 使用經驗片段更新頁眉和頁腳 {#experience-fragments}

建立全局內容（如頁眉或頁腳）時，通常使用 [體驗片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 體驗片段，允許用戶組合多個元件以建立單個可引用的元件。 經驗片段具有支援多站點管理和 [定位](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)。

站點模板生成了頁眉和頁腳。 接下來，更新「體驗片段」以匹配模型。 按照以下視頻中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332916?quality=12&learn=on)

以下視頻的高級步驟：

1. 下載示例內容包 **[WKND-Starter-Assets-Skate-Article-1.2.zip](assets/page-templates/WKND-Starter-Assets-Skate-Article-1.2.zip)**。
1. 使用包管理器上載和安裝內容包。
1. 更新頁眉和頁腳體驗片段以使用WKND徽標

## 建立雜誌文章頁

接下來，使用雜誌文章頁面模板建立新頁面。 編寫頁面內容以匹配網站模型。 按照以下視頻中的步驟操作：

>[!VIDEO](https://video.tv.adobe.com/v/332917?quality=12&learn=on)

使用 [提供的文本](./assets/page-templates/la-skateparks-copy.txt) 填充文章正文。

## 恭喜！ {#congratulations}

恭喜，您剛剛與Adobe Experience Manager Sites建立了新模板和頁面。

### 後續步驟 {#next-steps}

此時，雜誌文章頁面和網站與WKND的品牌風格不匹配。 關注 [天明](theming.md) 教程，瞭解更新用於將全局樣式應用到站點的CSS和Javascript前置代碼的最佳做法。

### 解決方案包

本章的解決方案包可供下載： [WKND-Magazine-Template-SOLUTION-1.0.zip](assets/page-templates/WKND-Magazine-Template-SOLUTION-1.0.zip)。
