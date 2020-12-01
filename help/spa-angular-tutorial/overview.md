---
title: AEM SPA Editor and Angular 快速入門
description: 使用WKND SPA建立您在Adobe Experience Manager, AEM中可編輯的第一個Angular Single Page Application(SPA)。 瞭解如何使用Angular JS架構與AEM的SPA編輯器建立SPA。 本多部分教學課程將逐步介紹虛擬生活品牌WKND的Angular應用程式實作。 本教學課程涵蓋SPA的端對端建立以及與AEM的整合。
sub-product: sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 4%

---


# 在AEM {#introduction}中建立您的第一個Angular SPA

歡迎使用多部分教學課程，專為Adobe Experience Manager(AEM)中&#x200B;**SPA Editor**&#x200B;功能新手的開發人員所設計。 本教學課程將逐步介紹虛擬生活品牌WKND的Angular應用程式實作。 Angular應用程式將會開發並設計為與AEM的SPA編輯器一起部署，後者會將Angular元件對應至AEM元件。 部署至AEM的已完成SPA，可使用AEM的傳統線上編輯工具動態編寫。

![最終實施的SPA](assets/wknd-spa-implementation.png)

*WKND SPA實施*

## 關於

本多部分教學課程的目的，是教導開發人員如何實作Angular應用程式，以搭配AEM的SPA編輯器功能運作。 在現實場景中，開發活動按角色劃分，通常涉及&#x200B;**前端開發人員**&#x200B;和&#x200B;**後端開發人員**。 我們相信，對於任何將參與AEM SPA Editor專案的開發人員來說，完成本教學課程都是有益的。

教學課程旨在搭配&#x200B;**AEM做為雲端服務**&#x200B;使用，並向後相容於&#x200B;**AEM 6.5.4+**&#x200B;和&#x200B;**AEM 6.4.8+**。 SPA的實施方式如下：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA編輯器](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [角度](https://angular.io/)

*預計1-2小時即可完成教學課程的每個部分。*

## 最新代碼

所有教學課程程式碼都可在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到。

[最新的程式碼基底](https://github.com/adobe/aem-guides-wknd-spa/releases)可供下載的AEM套件使用。

## 必備條件

在開始本教學課程之前，您需要下列項目：

* HTML、CSS和JavaScript的基本知識
* 對[Angular](https://angular.io/)的基本熟悉
* [AEM as a Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、 [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*雖然不需要，但對開發傳統AEM Sites元件有基 [本的瞭解是有益的](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 本地開發環境{#local-dev-environment}

完成本教學課程時，必須具備本機開發環境。 螢幕擷取和視訊會使用AEM擷取，當作在Mac OS環境上執行的Cloud Service SDK，並以[Visual Studio Code](https://code.visualstudio.com/)做為IDE。 除非另有說明，指令和程式碼應獨立於本機作業系統。

>[!NOTE]
>
> **您是AEM的新手嗎？** 請參閱下 [列指南，以使用AEM做為雲端服務SDK來設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **您是AEM 6.5的新手嗎？** 請參閱以 [下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 後續步驟{#next-steps}

你在等什麼！導覽至[SPA編輯器專案](create-project.md)章節，開始教學課程，並瞭解如何使用AEM專案原型產生啟用SPA編輯器的專案。

## 向後相容性{#compatibility}

本教學課程的專案程式碼是針對AEM以雲端服務建立的。 為使項目代碼向後相容&#x200B;**6.5.4+**&#x200B;和&#x200B;**6.4.8+**，已做了幾項修改。

[UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4**&#x200B;已包含為從屬關係：

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

已新增另一個名為`classic`的Maven設定檔，以修改建置至目標AEM 6.x環境：

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

`classic`描述檔預設為停用。 如果遵循AEM 6.x的教學課程，請在有指示時新增`classic`描述檔，以執行Maven組建：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

為AEM實作產生新專案時，請一律使用[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)的最新版本，並更新`aemVersion`以定位您的AEM預定版本。
