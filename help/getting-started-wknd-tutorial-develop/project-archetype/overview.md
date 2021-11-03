---
title: AEM Sites快速入門 — 專案原型
description: AEM Sites快速入門 — 專案原型。 WKND教學課程是多部分教學課程，專為剛接觸Adobe Experience Manager的開發人員所設計。 本教學課程會逐步說明虛擬生活風格品牌WKND的AEM網站實作。 本教學課程涵蓋基本主題，例如專案設定、主要原型、核心元件、可編輯範本、用戶端程式庫和元件開發。
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: 08146f57235f3de7fd5ab73754166cc85e1f7dda
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 35%

---

# AEM Sites快速入門 — 專案原型 {#project-archetype}

歡迎使用多部分教學課程，專為新進入Adobe Experience Manager(AEM)的開發人員所設計。 本教學課程會逐步說明WKND虛構生活風格品牌AEM網站的實作。

本教學課程的開頭為： [AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 來產生新專案。

此教學課程在設計上將使用 **AEM as a Cloud Service**，並向下相容於 **AEM 6.5.5.0+** 和 **AEM 6.4.8.1+**。網站的實作方式為：

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hant)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)
* Sling 模型
* [可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計約需 1-2 小時完成教學課程的每個部份。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程不可或缺的條件。螢幕截圖和影片都是使用在 Mac OS 環境上執行的 AEM as a Cloud Service SDK 擷取，並將 [Visual Studio Code](https://code.visualstudio.com/) 當做 IDE 使用。除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

### 所需軟體

應在本機安裝下列項目：

* 本機AEM **作者** 例項(Cloud ServiceSDK、6.5.5+或6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) （LTS — 長期支援）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio代碼](https://code.visualstudio.com/) 或等效IDE
   * [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 在整個教學課程中使用的工具

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## Github {#github}

您可在AEM指南存放庫的Github上找到專案的所有程式碼：

**[GitHub:WKND Sites專案](https://github.com/adobe/aem-guides-wknd)**

此外，教學課程的每個部分在GitHub中都有各自的分支。 使用者只要勾選與上一部分對應的分支，就能隨時開始教學課程。

## 後續步驟 {#next-steps}

你在等什麼?!導覽至 [專案設定](project-setup.md) 章節，並了解如何使用AEM專案原型產生新的Adobe Experience Manager專案。
