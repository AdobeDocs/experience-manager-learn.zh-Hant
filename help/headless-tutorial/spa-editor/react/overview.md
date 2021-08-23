---
title: AEM SPA Editor and React 快速入門
description: 建立可在Adobe Experience Manager AEM中使用WKND SPA編輯的第一個React Single Page Application(SPA)。 了解如何使用React JS架構搭配AEM SPA編輯器建立SPA。 本多部分教學課程將逐步說明虛擬生活風格品牌WKND的React應用程式實作。 教學課程涵蓋 SPA 端對端建立和 AEM 整合作業。
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
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 60%

---


# 在AEM中建立第一個React SPA {#overview}

歡迎使用專為 Adobe Experience Manager (AEM) 中 **SPA Editor** 功能的新手開發人員設計的多部分教學課程。本教學課程將逐步說明虛擬生活風格品牌WKND的React應用程式實作。 React應用程式將開發並設計為搭配AEM SPA Editor進行部署，此編輯器會將React元件對應至AEM元件。 部署至 AEM 的完成 SPA 即可使用 AEM 傳統的內嵌編輯工具動態製作。

![實作的最終 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 實作*

## 關於

此教學課程在設計上將使用 **AEM as a Cloud Service**，並向下相容於 **AEM 6.5.4+** 和 **AEM 6.4.8+**。

## 最新的程式碼

您可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa) 上找到教學課程的所有程式碼。

[最新的程式碼基底](https://github.com/adobe/aem-guides-wknd-spa/releases)可以當做可下載的 AEM 套件使用。

## 必備條件

開始進行此教學課程前，您需要具備以下條件：

* HTML、CSS 和 JavaScript 的基本知識
* 基本熟悉[React](https://reactjs.org/tutorial/tutorial.html)

*雖然不一定需要，但對於[開發傳統的 AEM Sites 元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)有基本的了解也會有幫助。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程不可或缺的條件。螢幕截圖和影片都是使用在 Mac OS 環境上執行的 AEM as a Cloud Service SDK 擷取，並將 [Visual Studio Code](https://code.visualstudio.com/) 當做 IDE 使用。除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

### 所需軟體

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)、[AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) 或 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 後續步驟 {#next-steps}

你在等什麼?!導覽至[建立專案](create-project.md)章節，了解如何使用AEM專案原型產生已啟用SPA編輯器的專案，以啟動教學課程。
