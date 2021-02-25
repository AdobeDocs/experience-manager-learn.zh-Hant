---
title: AEM SPA Editor and Angular 快速入門
description: 使用WKND建立您在Adobe Experience Manager可編SPA輯的第一個Angular單頁AEM應用程式()SPA。 瞭解如何搭配編SPA輯器使用AngularJS架AEM構SPA建立。 本多部分教學課程將逐步介紹虛擬生活品牌WKND的Angular應用程式實作。 本教學課程涵蓋端對端的建立SPA與整合AEM。
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
ht-degree: 5%

---


# 在&lt;a0/SPA>中建AEM立您的第一個Angular{#introduction}

歡迎使用多部分教學課程，專為Adobe Experience Manager(SPA)**Editor**&#x200B;功能新手的開發人員所設AEM計。 本教學課程將逐步介紹虛擬生活品牌WKND的Angular應用程式實作。 angular應用程式將會開發並設計為與Editor一起部AEM署，SPA後者會將Angular元件對應至AEM元件。 已完SPA成、部署至AEM的編輯工具可以動態編寫AEM。

![最終實SPA施](assets/wknd-spa-implementation.png)

*WKND實SPA施*

## 關於

本多部分教學課程的目標，是教導開發人員如何建置Angular應用程式，以搭配SPAEditor功能AEM。 在現實場景中，開發活動按角色劃分，通常涉及&#x200B;**前端開發人員**&#x200B;和&#x200B;**後端開發人員**。 我們相信，任何將參與編輯器專案的開發人員，都AEM可以SPA完成本教學課程。

本教學課程旨在與&#x200B;**作AEM為Cloud Service**&#x200B;一起使用，並向後相容&lt;a2/AEM>6.5.4+**和** AEM 6.4.8+**。**&#x200B;實SPA施方式為：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/archetype/overview.html)
* [編AEM輯SPA器](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [Angular](https://angular.io/)

*預計1-2小時即可完成教學課程的每個部分。*

## 最新代碼

所有教學課程程式碼都可在[GitHub](https://github.com/adobe/aem-guides-wknd-spa)上找到。

[最新代碼庫](https://github.com/adobe/aem-guides-wknd-spa/releases)可作為可下載的包AEM提供。

## 必備條件

在開始本教學課程之前，您需要下列項目：

* HTML、CSS和JavaScript的基本知識
* 對[Angular](https://angular.io/)的基本熟悉
* [作為AEMCloud ServiceSDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk), [AEM6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 或 [AEM6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9或更新版本）
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*雖然不需要，但對開發傳統AEM Sites部件有 [基本認識](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## 本地開發環境{#local-dev-environment}

完成本教學課程時，必須具備本機開發環境。 螢幕擷取和視訊會以AEMCloud ServiceSDK的形式擷取，此SDK在Mac OS環境中執行，IDE為[Visual Studio Code](https://code.visualstudio.com/)。 除非另有說明，指令和程式碼應獨立於本機作業系統。

>[!NOTE]
>
> **剛當AEMCloud Service?** 請參閱下 [列指南，以使用作為Cloud ServiceSDK來設AEM定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **6.AEM5新手？** 請參閱以 [下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 後續步驟{#next-steps}

你在等什麼！導覽至[SPA Editor Project](create-project.md)章節，開始教學課程，並瞭解如何使用Project Archetype產生啟SPA用Editor的AEM專案。

## 向後相容性{#compatibility}

本教學課程的專案程式碼是以AEMCloud Service建立。 為使項目代碼向後相容&#x200B;**6.5.4+**&#x200B;和&#x200B;**6.4.8+**，已做了幾項修改。

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

已添加名為`classic`的其他Maven配置檔案，以修改構建到目標AEM6.x環境：

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

`classic`描述檔預設為停用。 如果遵循教學課程AEM6.x，請在指示執行Maven組建時新增`classic`描述檔：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

為實施生成新項AEM目時，請始終使用[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)的最新版本，並更新`aemVersion`以定位您的預定版本AEM。
