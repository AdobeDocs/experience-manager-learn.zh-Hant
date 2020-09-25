---
title: AEM Sites快速入門- WKND教學課程
description: AEM Sites快速入門- WKND教學課程。 WKND教學課程是多部分教學課程，專為Adobe Experience Manager新手開發人員所設計。 本教學課程將逐步介紹AEM網站的實作，以建立虛構的生活品牌WKND。 本教學課程涵蓋基本主題，例如專案設定、主原型、核心元件、可編輯範本、用戶端程式庫和元件開發。
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
ht-degree: 3%

---


# Getting Started with AEM Sites - WKND Tutorial {#introduction}

歡迎使用專為Adobe Experience Manager(AEM)新手開發人員所設計的多部份教學課程。 本教學課程將逐步介紹AEM網站對WKND這個虛構生活方式品牌的實作。 本教學課程涵蓋基本主題，例如專案設定、核心元件、可編輯範本、用戶端程式庫，以及使用Adobe Experience Manager Sites進行元件開發。

## 概覽 {#wknd-tutorial-overview}

本多部分教學課程的目標，是教導開發人員如何使用Adobe Experience Manager(AEM)中的最新標準和技術來建置網站。 完成本教學課程後，開發人員應瞭解平台的基本基礎，並具備AEM中常見設計模式的相關知識。

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

教學課程是專為搭配 **AEM做為Cloud Service** ，並向後相容於 **AEM 6.5+** 和 **AEM 6.4.2+**。 網站的實作方式為：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling Models
* [可編輯的範本](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [樣式系統](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*預計1-2小時即可完成教學課程的每個部分。*

## 關於教學課程 {#about-tutorial}

WKND是虛構的線上雜誌和部落格，主要針對數個國際城市的夜生活、活動和活動。

### Adobe XD UI套件

為了讓本教學課程更貼近實際案例，Adobe有才華的UX設計人員使用 [Adobe XD為網站建立了模型](https://www.adobe.com/products/xd.html)。 在教學課程中，各種設計會建置在完全可供作者使用的AEM網站中。 特別感謝 **Lorenzo Buosi****和Kilian Amendola** ，他們為WKND網站設計了美麗的設計。

下載XD UI套件：

* [AEM核心元件UI套件](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND UI Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

WKND的名稱很合適，因為我們預期開發人員可在週末的大部分時間 ***內*** ，完成教學課程。

### 吉圖布 {#github}

專案的所有程式碼都可在AEM Guide回購網站的Github上找到：

**[GitHub:WKND Sites Project](https://github.com/adobe/aem-guides-wknd)**

此外，教學課程的每個部分都有其專屬的GitHub分支。 用戶只需檢出與上一個部件對應的分支，即可隨時開始教程。

>[!NOTE]
>
> 如果您使用本教學課程的舊版，您仍可在GitHub上找 [到解決方](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) 案 [套件和程式碼](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) 。

## 當地開發環境 {#local-dev-environment}

完成本教學課程時，必須具備本機開發環境。 螢幕擷取畫面和視訊會使用AEM擷取，當作在Mac OS環境上執行的Cloud Service SDK。 除非另有說明，指令和程式碼應獨立於本機作業系統。

**您是AEM的新手嗎？** 請參閱下 [列指南，以使用AEM做為雲端服務SDK來設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**您是AEM 6.5的新手嗎？** 請參閱以 [下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

### 所需軟體

本機應安裝下列程式碼：

* [AEM as a Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)[或AEM 6.5](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/technical-requirements.html) 或 [AEM 6.4 + SP2](https://helpx.adobe.com/tw/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) （僅限AEM 6.5+）
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.js v10+](https://nodejs.org/en/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

### 整合開發環境(IDE)

本教學課 [程將](https://www.eclipse.org/) Eclipse與 [AEM Developer Tool Plugin](https://eclipse.adobe.com/aem/dev-tools/) （AEM開發人員工具外掛程式）搭配使用，不過可使用支援Java和Maven專案的任何IDE。 本教學課程對特定IDE功能的依賴度很低。

如需使用Eclipse或其他IDE(例如 [Visual Studio Code](https://code.visualstudio.com/) 或 [IntelliJ](https://www.jetbrains.com/idea/))的詳細步 [驟，請參閱以下指南](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 引用網站 {#reference-site}

WKND網站的完成版本也可供參考： [https://wknd.site/](https://wknd.site/)

本教學課程涵蓋AEM開發人員所需的主要開發技 *巧* ，但不會建立整個網站。 完成的參考網站是探索並檢視更多AEM現成可用功能的絕佳資源。

若要在跳至教學課程之前先測試最新程式碼，請從GitHub下載 **[並安裝最新版本](https://github.com/adobe/aem-guides-wknd/releases/latest)**。

### 由Adobe Stock提供支援

WKND參考網站中的許多影像都來自 [Adobe Stock](https://stock.adobe.com/) ，並且是示範資產附加條款(Demo Asset Additional Terms)中定義的第三方 [材料](https://www.adobe.com/legal/terms.html)。 如果您想要將Adobe Stock影像用於檢視本示範網站以外的其他用途，例如在網站上加以展示，或在行銷資料中，則可以購買Adobe Stock授權。

有了Adobe Stock，您就可以存取超過1億4千萬張高品質且免版稅的影像，包括像片、圖形、視訊和範本，快速開始您的創意專案。

## 後續步驟 {#next-steps}

你在等什麼！導覽至「專案設定」 [一章](project-setup.md) ，以開始教學課程，並瞭解如何使用AEM專案原型產生新的Adobe Experience Manager專案。
