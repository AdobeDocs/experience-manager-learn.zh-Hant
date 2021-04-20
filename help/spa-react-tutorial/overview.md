---
title: AEM SPA Editor and React 快速入門
description: 使用WKND建立您在Adobe Experience Manager可編輯的SPA第一個React Single Page Application(AEM)SPA。 瞭解如何使用React JSSPA架構與編輯器AEM建SPA立。 本多部分教學課程將逐步介紹虛擬生活品牌WKND的React應用程式實作。 教學課程涵蓋 SPA 端對端建立和 AEM 整合作業。
sub-product: Sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 74%

---


# 在&lt;a0/SPA>中建AEM立您的第一個React{#overview}

歡迎使用專為 Adobe Experience Manager (AEM) 中 **SPA Editor** 功能的新手開發人員設計的多部分教學課程。本教學課程將逐步介紹虛擬生活品牌WKND的React應用程式實作。 React應用程式將會開發並設計為與Editor一起部AEM署，SPA Editor會將React元件對應至AEM元件。 部署至 AEM 的完成 SPA 即可使用 AEM 傳統的內嵌編輯工具動態製作。

![實作的最終 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 實作*

## 關於

本多部分教學課程的目的，是教導開發人員如何實作React應用程式，以搭配SPAEditor功能AEM。 在真實世界情況中，開發活動會依人員細分，常涉及&#x200B;**前端開發人員**&#x200B;和&#x200B;**後端開發人員**。我們認為任何將參與 AEM SPA Editor 專案的任一開發人員在完成此教學課程後，將獲益良多。

此教學課程在設計上將使用 **AEM as a Cloud Service**，並向下相容於 **AEM 6.5.4+** 和 **AEM 6.4.8+**。使用以下項目實作 SPA：

* [Maven AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [核心元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-core-components/using/introduction.html)
* [React JS](https://reactjs.org/)
* [建立React App](https://create-react-app.dev/)

*預計約需 1-2 小時完成教學課程的每個部份。*

## 最新的程式碼

您可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa) 上找到教學課程的所有程式碼。

[最新的程式碼基底](https://github.com/adobe/aem-guides-wknd-spa/releases)可以當做可下載的 AEM 套件使用。

## 必備條件

開始進行此教學課程前，您需要具備以下條件：

* HTML、CSS 和 JavaScript 的基本知識
* 基本熟悉[React](https://reactjs.org/tutorial/tutorial.html)
* [AEM as a Cloud Service SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、[AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) 或 [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

*雖然不一定需要，但對於[開發傳統的 AEM Sites 元件](https://docs.adobe.com/content/help/zh-Hant/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)有基本的了解也會有幫助。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程所不可或缺的條件。螢幕擷圖和影片都是使用在 Mac OS 環境上執行的 AEM as a Cloud Service SDK 擷取，並將 [Visual Studio Code](https://code.visualstudio.com/) 當做 IDE 使用。除非另有註明，否則命令和程式碼應獨立於本機作業系統。

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 後續步驟{#next-steps}

您還在等什麼？！請瀏覽至 [SPA Editor 專案](create-project.md)章節，開始進行此教學課程並了解如何使用 AEM Project Archetype 產生啟用 SPA Editor 的專案。

## 向下相容性 {#compatibility}

此教學課程的專案程式碼是針對 AEM as a Cloud Service 所建置。為了使項目代碼向後相容&#x200B;**6.5.4+**&#x200B;和&#x200B;**6.4.8+**，對教程的POM檔案進行了幾項修改。

The [UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** 已當做相依性納入：

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

`classic` 描述檔預設為停用。如果遵循教學課程AEM6.x，請在指示執行Maven組建時新增`classic`描述檔，例如：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

產生 AEM 實作的新專案時，物閉使用最新版本的 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 並更新 `aemVersion`，以將您預期的 AEM 版本做為目標。
