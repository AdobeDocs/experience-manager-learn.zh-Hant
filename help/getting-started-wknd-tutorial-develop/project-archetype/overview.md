---
title: AEM Sites入門 — 項目原型
description: AEM Sites入門 — 原型項目。 WKND教程是為Adobe Experience Manager新晉開發人員設計的多部分教程。 本教程將介紹虛擬生AEM活品牌WKND網站的實施。 本教程介紹基本主題，如項目設定、主要原型、核心元件、可編輯模板、客戶端庫和元件開發。
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
ht-degree: 40%

---

# AEM Sites入門 — 項目原型 {#project-archetype}

歡迎學習為Adobe Experience Manager()新開發人員設計的多部分教AEM程。 此教學課程會逐步引導您實施虛擬生活風格品牌 WKND 的 AEM 網站。

本教程首先使用 [項AEM目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant) 生成新項目。

本教程旨在 **AEMas a Cloud Service** 向後相容 **AEM6.5.14**。 該站點使用：

* [Maven AEM 專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)
* [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計約需 1-2 小時完成教學課程的每個部份。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程不可或缺的條件。使用在macOS環境中運行AEM的as a Cloud ServiceSDK捕獲螢幕截圖和視頻 [Visual Studio代碼](https://code.visualstudio.com/) 的下界。 除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

### 所需軟體

應在本地安裝以下內容：

* [本地AEM **作者** 實例](https://experience.adobe.com/#/downloads) (Cloud ServiceSDK或6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [節點.js](https://nodejs.org/en/) （LTS — 長期支援）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio代碼](https://code.visualstudio.com/) 或等效的IDE
   * [VSCode同AEM步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 在整個教程中使用的工具

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## GitHub {#github}

本教程中的代碼可在GitHub上的「指南」回AEM購版中找到：

**[GitHub:WKND站點項目](https://github.com/adobe/aem-guides-wknd)**

此外，本教程的每個部分都在GitHub中有自己的分支。 用戶只需檢出與上一部件對應的分支即可開始教程。

## 後續步驟 {#next-steps}

你在等什麼？ 導航至 [項目設定](project-setup.md) 並學習如何使用項目原型生成新的Adobe Experience ManagerAEM項目。
