---
title: AEM SPA Editor and Angular 快速入門
description: 建立可在 Adobe Experience Manager (AEM) 中使用 WKND SPA 編輯的第一個 Angular Single Page Application (SPA)。了解如何使用 Angular JS 框架和 AEM 的 SPA Editor 建立 SPA。此多部分教學課程會逐步引導您為虛擬生活風格品牌 WKND 實作 Angular 應用程式。教學課程涵蓋 SPA 端對端建立和 AEM 整合作業。
sub-product: Sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
source-git-commit: bf9ab30f57faa23721d7d27b837d8e0f0e8cf4f1
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 99%

---


# 建立您在 AEM 的第一個 Angular SPA {#introduction}

歡迎使用專為 Adobe Experience Manager (AEM) 中 **SPA Editor** 功能的新手開發人員設計的多部分教學課程。此教學課程會逐步引導您為虛擬生活風格品牌 WKND 實作 Angular 應用程式。AEM 的 SPA Editor 將用以開發和設計部署 Angular 應用程式，將 Angular 元件對應至 AEM 元件。部署至 AEM 的完成 SPA 即可使用 AEM 傳統的內嵌編輯工具動態製作。

![實作的最終 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 實作*

## 關於

此多部份教學課程的目的為教導開發人員如何實作 Angular 應用程式，以使用 AEM 的 SPA Editor 功能。在真實世界情況中，開發活動會依人員細分，通常包含&#x200B;**前端開發人員**&#x200B;和&#x200B;**後端開發人員**。我們認為任何將參與 AEM SPA Editor 專案的開發人員在完成此教學課程後，將獲益良多。

此教學課程在設計上將使用 **AEM as a Cloud Service**，並向下相容於 **AEM 6.5.4+** 和 **AEM 6.4.8+**。使用以下項目實作 SPA：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/zh-Hant/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*預計約需 1-2 小時完成教學課程的每個部份。*

## 最新的程式碼

您可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa) 上找到教學課程的所有程式碼。

[最新的程式碼基底](https://github.com/adobe/aem-guides-wknd-spa/releases)可以當做可下載的 AEM 套件使用。

## 必備條件

開始進行此教學課程前，您需要具備以下條件：

* HTML、CSS 和 JavaScript 的基本知識
* 對於 [Angular](https://angular.io/) 有基本的認識
* [AEM as a Cloud Service SDK](https://docs.adobe.com/content/help/zh-Hant/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、[AEM 6.5.4+](https://helpx.adobe.com/tw/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/tw/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

*雖然不一定需要，但對於[開發傳統的 AEM Sites 元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)有基本的了解也會有幫助。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程不可或缺的條件。螢幕截圖和影片都是使用在 Mac OS 環境上執行的 AEM as a Cloud Service SDK 擷取，並將 [Visual Studio Code](https://code.visualstudio.com/) 當做 IDE 使用。除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://docs.adobe.com/content/help/zh-Hant/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://docs.adobe.com/content/help/zh-Hant/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 後續步驟{#next-steps}

您還在等什麼？！請瀏覽至 [SPA Editor 專案](create-project.md)章節，開始進行此教學課程並了解如何使用 AEM Project Archetype 產生啟用 SPA Editor 的專案。

## 向下相容性 {#compatibility}

此教學課程的專案程式碼是針對 AEM as a Cloud Service 所建置。為了讓專案程式碼向下相容於 **6.5.4+** 和 **6.4.8+**，已進行數次修改。

The [UberJar](https://docs.adobe.com/content/help/zh-Hant/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** 已當做相依性納入：

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

已新增名為 `classic` 的其他 Maven 描述，以修改組建並將 AEM 6.x 環境做為目標：

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

`classic` 描述檔預設為停用。如果使用 AEM 6.x 進行教學課程，每次收到指示要執行 Maven 組建時，請新增 `classic` 描述檔：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

產生 AEM 實作的新專案時，務必使用最新版本的 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 並更新 `aemVersion`，以將您預期的 AEM 版本做為目標。