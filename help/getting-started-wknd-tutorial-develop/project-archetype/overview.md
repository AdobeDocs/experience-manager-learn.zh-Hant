---
title: AEM Sites — 專案原型快速入門
description: AEM Sites — 專案原型快速入門。 WKND教學課程是多部分教學課程，專為Adobe Experience Manager的新手開發人員設計。 此教學課程會逐步引導您為虛擬生活風格品牌WKND實作AEM網站。 此教學課程涵蓋基礎的主題，例如專案設定、maven原型、核心元件、可編輯的範本、使用者端資料庫和元件開發。
version: 6.5, Cloud Service
feature: Core Components, Page Editor, Editable Templates, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
index: y
doc-type: Tutorial
exl-id: 90d14734-f644-4a45-9361-1e47a5b52fff
recommendations: disable
duration: 84
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 28%

---

# AEM Sites — 專案原型快速入門 {#project-archetype}

{{edge-delivery-services-and-page-editor}}

歡迎使用專為Adobe Experience Manager (AEM)的新手開發人員設計的多部分教學課程。 此教學課程會逐步引導您實施虛擬生活風格品牌WKND的AEM網站。

本教學課程從使用開始 [AEM專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html) 以產生新專案。

此教學課程在設計上將使用 **AEMas a Cloud Service** 並且與回溯相容 **AEM 6.5.14+**. 使用以下項目實作網站：

* [Maven AEM 專案原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hant)
* [核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)
* [Sling 模型](https://sling.apache.org/documentation/bundles/models.html)
* [可編輯的範本](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計約需 1-2 小時完成教學課程的每個部份。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程不可或缺的條件。熒幕截圖和影片都是使用在macOS環境中執行的AEMas a Cloud ServiceSDK擷取，並搭配使用 [Visual Studio Code](https://code.visualstudio.com/) 作為IDE。 除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

### 必要的軟體

下列專案應在本機安裝：

* [本機AEM **作者** 例項](https://experience.adobe.com/#/downloads) (Cloud Service SDK或6.5.14+)
* [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) （LTS — 長期支援）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio Code](https://code.visualstudio.com/) 或同等的IDE
   * [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  — 教學課程中使用的工具

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## GitHub {#github}

本教學課程的程式碼可在AEM指南存放庫的GitHub上找到：

**[GitHub： WKND網站專案](https://github.com/adobe/aem-guides-wknd)**

此外，教學課程的每個部分在GitHub中都有各自的分支。 使用者只需出庫與上一個零件對應的分支即可隨時開始教學課程。

## 後續步驟 {#next-steps}

您還在等什麼？ 導覽至「 」以開始本教學課程 [專案設定](project-setup.md) 章節，並瞭解如何使用AEM專案原型產生新的Adobe Experience Manager專案。
