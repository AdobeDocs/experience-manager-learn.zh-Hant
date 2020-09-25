---
title: AEM SPA編輯器快速入門與回應
description: 使用WKND SPA建立您在Adobe Experience Manager AEM中可編輯的第一個React Single Page Application(SPA)。 瞭解如何使用React JS架構與AEM的SPA編輯器建立SPA。 本多部分教學課程將逐步介紹虛擬生活品牌WKND的React應用程式實作。 本教學課程涵蓋SPA的端對端建立以及與AEM的整合。
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 2%

---


# 在AEM中建立您的第一個React SPA {#overview}

歡迎使用多部分教學課程，專為Adobe Experience Manager(AEM)中 **SPA Editor** （SPA編輯器）功能新手設計。 本教學課程將逐步介紹虛擬生活品牌WKND的React應用程式實作。 React應用程式將會開發並設計為與AEM的SPA編輯器一起部署，後者會將React元件對應至AEM元件。 部署至AEM的已完成SPA，可使用AEM的傳統線上編輯工具動態編寫。

![最終實施的SPA](assets/wknd-spa-implementation.png)

*WKND SPA實施*

## 關於

本多部分教學課程的目標，是教導開發人員如何實作React應用程式，以搭配AEM的SPA編輯器功能運作。 在現實場景中，開發活動會依角色細分，通常涉及前端開發人 **員****和後端開發人員**。 我們相信，對於任何將參與AEM SPA Editor專案的開發人員來說，完成本教學課程都是有益的。

教學課程是專為搭配 **AEM做為雲端服務而設計** ，並向後相容於 **AEM 6.5.4+** 和 **AEM 6.4.8+**。 SPA的實施方式如下：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA編輯器](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [React JS](https://reactjs.org/)
* [建立React App](https://create-react-app.dev/)

*預計1-2小時即可完成教學課程的每個部分。*

## 最新代碼

所有教學課程程式碼都可在 [GitHub上找到](https://github.com/adobe/aem-guides-wknd-spa)。

最新 [的程式碼庫](https://github.com/adobe/aem-guides-wknd-spa/releases) ，可供下載的AEM套件使用。

## 必備條件

在開始本教學課程之前，您需要下列項目：

* HTML、CSS和JavaScript的基本知識
* 基本熟悉 [React](https://reactjs.org/tutorial/tutorial.html)
* [AEM as a Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、 [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

*雖然不需要，但對開發傳統AEM Sites元件有基[本的瞭解是有益的](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 當地開發環境 {#local-dev-environment}

完成本教學課程時，必須具備本機開發環境。 螢幕擷取和視訊會使用AEM擷取，當作在Mac OS環境上執行的Cloud Service SDK, [Visual Studio Code為IDE](https://code.visualstudio.com/) 。 除非另有說明，指令和程式碼應獨立於本機作業系統。

>[!NOTE]
>
> **您是AEM的新手嗎？** 請參閱下 [列指南，以使用AEM做為雲端服務SDK來設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **您是AEM 6.5的新手嗎？** 請參閱以 [下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 後續步驟 {#next-steps}

你在等什麼！導覽至「 [SPA編輯器專案」章節，開始教學課程](create-project.md) ，並瞭解如何使用AEM Project Archetype產生啟用SPA編輯器的專案。

## 向後相容性 {#compatibility}

本教學課程的專案程式碼是針對AEM以雲端服務建立的。 為了使項目代碼向後相容 **6.5.4+和****6.4.8+** ，對教程的POM檔案進行了幾項修改。

Uber [Jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** 已包含為依賴項：

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

已新增其他Maven設定檔( `classic` 名稱為)，以修改建置至目標AEM 6.x環境：

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

預 `classic` 設會停用描述檔。 如果遵循AEM 6.x的教學課程，請在有指示 `classic` 執行Maven組建時新增描述檔，例如：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

為AEM實作產生新專案時，請一律使用最新版 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) ，並 `aemVersion` 更新以定位您的AEM目標版本。
