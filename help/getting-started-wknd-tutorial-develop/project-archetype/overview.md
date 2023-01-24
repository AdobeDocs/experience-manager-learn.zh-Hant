---
title: AEM Sites快速入門 — 專案原型
description: AEM Sites快速入門 — 專案原型。 WKND教學課程是多部分教學課程，專為剛接觸Adobe Experience Manager的開發人員所設計。 本教學課程會逐步說明虛擬生活風格品牌WKND的AEM網站實作。 本教學課程涵蓋基本主題，例如專案設定、主要原型、核心元件、可編輯範本、用戶端程式庫和元件開發。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 35%

---

# AEM Sites快速入門 — 專案原型 {#project-archetype}

歡迎使用多部分教學課程，專為新進入Adobe Experience Manager(AEM)的開發人員所設計。 此教學課程會逐步引導您實施虛擬生活風格品牌 WKND 的 AEM 網站。

本教學課程的開頭為： [AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant) 來產生新專案。

本教學課程旨在搭配 **AEMas a Cloud Service** 而且回溯相容於 **AEM 6.5.14+**. 網站的實作方式為：

* [Maven AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)
* [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計約需 1-2 小時完成教學課程的每個部份。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程不可或缺的條件。螢幕擷取畫面和影片是使用在macOS環境中執行的AEMas a Cloud ServiceSDK來擷取，並搭配 [Visual Studio代碼](https://code.visualstudio.com/) 作為IDE。 除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

### 所需軟體

應在本機安裝下列項目：

* [本機AEM **作者** 執行個體](https://experience.adobe.com/#/downloads) (Cloud ServiceSDK或6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) （LTS — 長期支援）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio代碼](https://code.visualstudio.com/) 或等效IDE
   * [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 在整個教學課程中使用的工具

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## GitHub {#github}

您可在AEM指南存放庫的GitHub上找到本教學課程的程式碼：

**[GitHub:WKND Sites專案](https://github.com/adobe/aem-guides-wknd)**

此外，教學課程的每個部分在GitHub中都有各自的分支。 使用者只要勾選與上一個部分對應的分支，就能隨時開始教學課程。

## 後續步驟 {#next-steps}

你在等什麼？ 導覽至 [專案設定](project-setup.md) 章節，並了解如何使用AEM專案原型產生新的Adobe Experience Manager專案。
