---
title: AEM Sites - WKND 教學課程快速入門
description: AEM Sites - WKND 教學課程快速入門. WKND教學課程是多部分教學課程，專為Adobe Experience Manager新手開發人員所設計。 本教學課程將逐步介紹AEM網站的實作，以建立虛構的生活品牌WKND。 本教學課程涵蓋基本主題，例如專案設定、主原型、核心元件、可編輯範本、用戶端程式庫和元件開發。
sub-product: sites
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
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 5%

---


# AEM Sites - WKND 教學課程快速入門 {#introduction}

歡迎使用專為Adobe Experience Manager(AEM)新手開發人員所設計的多部份教學課程。 本教學課程將逐步介紹AEM網站對WKND這個虛構生活方式品牌的實作。 本教學課程涵蓋基本主題，例如專案設定、核心元件、可編輯範本、用戶端程式庫，以及使用Adobe Experience Manager Sites進行元件開發。

## 概覽 {#wknd-tutorial-overview}

本多部分教學課程的目標，是教導開發人員如何使用Adobe Experience Manager(AEM)中的最新標準和技術來建置網站。 完成本教學課程後，開發人員應瞭解平台的基本基礎，並具備AEM中常見設計模式的相關知識。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

教學課程旨在搭配&#x200B;**AEM做為Cloud Service**&#x200B;使用，並向後相容於&#x200B;**AEM 6.5+**&#x200B;和&#x200B;**AEM 6.4.2+**。 網站的實作方式為：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling Models
* [可編輯的範本](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [樣式系統](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計1-2小時即可完成教學課程的每個部分。*

## 關於教學課程{#about-tutorial}

WKND是虛構的線上雜誌和部落格，主要針對數個國際城市的夜生活、活動和活動。

### Adobe XD UI套件

為了讓本教學課程更接近實際案例，Adobe有才華的UX設計人員使用[Adobe XD](https://www.adobe.com/products/xd.html)為網站建立了模型。 在教學課程中，各種設計會建置在完全可供作者使用的AEM網站中。 特別感謝為世界開發組織網站設計精美設計的&#x200B;**洛倫佐·布奧西**&#x200B;和&#x200B;**基利安·阿門多拉**。

下載XD UI套件：

* [AEM核心元件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

WKND名稱正合適，因為我們預期開發人員會參與&#x200B;***weekend***&#x200B;的大部分工作，以完成教學課程。

### Github {#github}

專案的所有程式碼都可在AEM Guide回購網站的Github上找到：

**[GitHub:WKND Sites Project](https://github.com/adobe/aem-guides-wknd)**

此外，教學課程的每個部分都有其專屬的GitHub分支。 用戶只需檢出與上一個部件對應的分支，即可隨時開始教程。

>[!NOTE]
>
> 如果您使用本教學課程的舊版，您仍可在GitHub上找到[解決方案套件](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1)和[程式碼](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1)。

## 本地開發環境{#local-dev-environment}

完成本教學課程時，必須具備本機開發環境。 螢幕擷取畫面和視訊會使用AEM擷取，當作在Mac OS環境上執行的Cloud Service SDK。 除非另有說明，指令和程式碼應獨立於本機作業系統。

**您是AEM的新手嗎？** 請參閱下 [列指南，以使用AEM做為雲端服務SDK來設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**您是AEM 6.5的新手嗎？** 請參閱以 [下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

### 所需軟體

本機應安裝下列程式碼：

* [AEM as a Cloud Service ](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) SDK [或AEM 6.5](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/technical-requirements.html) 或 [AEM 6.4 + SP2](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) （僅限AEM 6.5+）
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

### 整合開發環境(IDE)

本教學課程將[Eclipse](https://www.eclipse.org/)與[AEM Developer Tool Plugin](https://eclipse.adobe.com/aem/dev-tools/)搭配使用，做為IDE，但是可使用支援Java和Maven專案的任何IDE。 本教學課程對特定IDE功能的依賴度很低。

有關使用Eclipse或其他IDE（如[Visual Studio Code](https://code.visualstudio.com/)或[IntelliJ](https://www.jetbrains.com/idea/)）的詳細步驟，請參閱以下指南](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。[

## 引用網站 {#reference-site}

WKND網站的完成版本也可供參考：[https://wknd.site/](https://wknd.site/)

本教學課程涵蓋AEM開發人員所需的主要開發技巧，但&#x200B;*not*&#x200B;將建立整個網站的端對端。 完成的參考網站是探索並檢視更多AEM現成可用功能的絕佳資源。

若要在跳至教學課程之前測試最新程式碼，請從GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)**下載並安裝**[&#x200B;最新版本。

### 由Adobe Stock提供支援

WKND參考網站中的許多影像都來自[Adobe Stock](https://stock.adobe.com/)，並且如示範資產附加條款(位於[https://www.adobe.com/legal/terms.html](https://www.adobe.com/legal/terms.html))中所定義，為協力廠商內容。 如果您想要將Adobe Stock影像用於檢視本示範網站以外的其他用途，例如在網站上加以展示，或在行銷資料中，則可以購買Adobe Stock授權。

有了Adobe Stock，您就可以存取超過1億4千萬張高品質且免版稅的影像，包括像片、圖形、視訊和範本，快速開始您的創意專案。

## 後續步驟{#next-steps}

你在等什麼！導覽至「[專案設定](project-setup.md)」章節，開始教學課程，並瞭解如何使用AEM專案原型產生新的Adobe Experience Manager專案。
