---
title: AEM Sites 快速入門 - 專案原型
description: AEM Sites 快速入門 - 專案原型。WKND 教學課程是專為初次接觸 Adobe Experience Manager 的開發人員設計且包含多個部分的教學課程。此教學課程會逐步解說為虛構生活風格品牌 WKND 實施 AEM 網站的情形。此教學課程涵蓋基礎主題，例如專案設定、Maven 原型、核心元件、可編輯的範本、用戶端程式庫以及元件開發。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: display, noCatalog
duration: 74
source-git-commit: b612e2e36af8f56661a07577e979959c650564ee
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 100%

---

# AEM Sites 快速入門 - 專案原型 {#project-archetype}

{{traditional-aem}}

歡迎使用專為初次接觸 Adobe Experience Manager (AEM) 的開發人員設計且包含多個部分的教學課程。此教學課程會逐步解說為虛擬生活風格品牌 WKND 實施 AEM 網站的情形。

本教學課程開始時會使用 [AEM 專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)產生一個新專案。

此教學課程能與 **AEM as a Cloud Service** 搭配使用，並向下相容於 **AEM 6.5.14+**。使用以下項目實作網站：

* [Maven AEM 專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)
* [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html?lang=zh-Hant)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=zh-Hant)
* [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=zh-Hant)

*預計需要 1 至 2 小時完成教學課程的每個部份。*

## 本機開發環境 {#local-dev-environment}

您必須具備本機開發環境才能完成此教學課程。利用 [Visual Studio Code](https://code.visualstudio.com/) 作為 IDE，並使用在 macOS 環境執行的 AEM as a Cloud Service SDK 來擷取螢幕擷圖和影片。除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

### 必要的軟體

必須在本機安裝以下工具：

* [本機 AEM **作者** 實例](https://experience.adobe.com/#/downloads) (Cloud Service SDK 或 6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或以上版本)
* [Node.js](https://nodejs.org/en/)&#x200B;( LTS - 長期支援版)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) 或同等的 IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) - 教學課程中使用的工具

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hant)。

## GitHub {#github}

本教學課程中使用的程式碼可在 GitHub 的 AEM 指南存放庫中找到：

**[GitHub：WKND 網站專案](https://github.com/adobe/aem-guides-wknd)**

此外，本教學課程的每個部分在 GitHub 上都有各自的分支。使用者只要查看與前一部分相對應的分支，便可以從任何地方開始教學課程。

## 後續步驟 {#next-steps}

您還在等什麼？導覽至「[專案設定](project-setup.md)」章節並開始教學課程，了解如何使用 AEM 專案原型產生新的 Adobe Experience Manager 專案。
