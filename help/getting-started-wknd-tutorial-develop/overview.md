---
title: AEM Sites - WKND 教學課程快速入門
description: AEM Sites - WKND 教學課程快速入門. WKND教學課程是多部份教學課程，專為Adobe Experience Manager新手開發人員所設計。 本教學課程將逐步介紹虛擬AEM生活品牌WKND的網站實作。 本教學課程涵蓋基本主題，例如專案設定、主原型、核心元件、可編輯範本、用戶端程式庫和元件開發。
sub-product: Sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '893'
ht-degree: 25%

---


# AEM Sites - WKND 教學課程快速入門 {#introduction}

歡迎使用專為Adobe Experience Manager()新手開發人員所設計的多部AEM份教學課程。 本教學課程將逐步介紹WKND這AEM個虛擬生活品牌網站的實作。 本教學課程涵蓋基本主題，例如專案設定、核心元件、可編輯範本、用戶端程式庫，以及與Adobe Experience Manager Sites共同開發元件。

## 概覽 {#wknd-tutorial-overview}

本多部分教學課程的目標，是教導開發人員如何使用Adobe Experience Manager()的最新標準和技術建置網AEM站。 完成本教學課程後，開發人員應瞭解平台的基本基礎，並瞭解中的常見設計模式AEM。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

此教學課程在設計上將使用 **AEM as a Cloud Service**，並向下相容於 **AEM 6.5.5.0+** 和 **AEM 6.4.8.1+**。網站的實作方式為：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling 模型
* [可編輯的範本](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [樣式系統](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計約需 1-2 小時完成教學課程的每個部份。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程所不可或缺的條件。螢幕擷圖和影片都是使用在 Mac OS 環境上執行的 AEM as a Cloud Service SDK 擷取，並將 [Visual Studio Code](https://code.visualstudio.com/) 當做 IDE 使用。除非另有註明，否則命令和程式碼應獨立於本機作業系統。

### 所需軟體

本機應安裝下列程式碼：

* 本AEM機&#x200B;**Author**&#x200B;例項(Cloud ServiceSDK、6.5.5+或6.4.8.1+)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) （LTS —— 長期支援）
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)
* [Visual Studio代](https://code.visualstudio.com/) 碼或相當的IDE
   * [VSCode Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)  —— 在整個教學課程中使用的工具

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 關於教學課程{#about-tutorial}

WKND是虛構的線上雜誌和部落格，主要針對數個國際城市的夜生活、活動和活動。

### Adobe XDUI Kit

為了讓本教學課程更接近實際案例場景，Adobe有才幹的UX設計人員使用[Adobe XD](https://www.adobe.com/products/xd.html)為網站建立模型。 在教學課程中，各種設計會建置在完全可編寫的網AEM站中。 特別感謝為世界開發組織網站設計精美設計的&#x200B;**洛倫佐·布奧西**&#x200B;和&#x200B;**基利安·阿門多拉**。

下載XDUI套件：

* [AEM核心元件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

WKND名稱正合適，因為我們預期開發人員會參與&#x200B;***weekend***&#x200B;的大部分工作，以完成教學課程。

### Github {#github}

項目的所有代碼都可在Github上找到，位於AEMGuide repo:

**[GitHub:WKND Sites Project](https://github.com/adobe/aem-guides-wknd)**

此外，教學課程的每個部分都有其專屬的GitHub分支。 用戶只需檢出與上一個部件對應的分支，即可隨時開始教程。

>[!NOTE]
>
> 如果您使用本教學課程的舊版，您仍可在GitHub上找到[解決方案套件](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1)和[程式碼](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1)。

## 引用網站 {#reference-site}

WKND網站的完成版本也可供參考：[https://wknd.site/](https://wknd.site/)

本教學課程涵蓋開發人員所需AEM的主要開發技巧，但&#x200B;*not*&#x200B;將建立整個網站的端對端。 完成的參考網站是探索和檢視更多現成功AEM能的絕佳資源。

若要在跳至教學課程之前測試最新程式碼，請從GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**下載並安裝**[&#x200B;最新版本。

### 由Adobe Stock提供

WKND參考網站中的許多影像都來自[Adobe Stock](https://stock.adobe.com/)，並且是示範資產附加條款中定義的第三方內容，網址為[https://www.adobe.com/legal/terms.html](https://www.adobe.com/tw/legal/terms.html)。 如果您想要將Adobe Stock影像用於其他目的，而不是檢視此示範網站，例如在網站上加以展示，或在行銷資料中，則可以在Adobe Stock購買授權。

透過Adobe Stock，您可以取用超過1億4千萬張高品質且免版稅的影像，包括像片、圖形、視訊和範本，以快速開始您的創意專案。

## 後續步驟{#next-steps}

你在等什麼！導覽至[Project Setup](project-setup.md)章節以開始教學課程，並瞭解如何使用Project Archetype產生新的Adobe Experience Manager專AEM案。
