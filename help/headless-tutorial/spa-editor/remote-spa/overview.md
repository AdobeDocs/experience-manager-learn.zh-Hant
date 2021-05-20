---
title: SPA Editor與遠端SPA快速入門 — 概觀
description: 歡迎使用多部分教學課程，協助開發人員使用AEM SPA編輯器，以可編輯的AEM內容來擴大現有的Remote SPA。
topic: 無頭、SPA、開發
feature: SPA編輯器，核心元件， API，開發
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
source-git-commit: cede0c97e0f322fe5d20d5c4f685ed10b90af1d4
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 5%

---


# 概覽

歡迎使用多部分教學課程，協助開發人員運用AEM SPA編輯器擴充現有的React型（或Next.js）Remote SPA內容，同時提供可編輯的AEM內容。

本教學課程以[WKND GraphQL應用程式](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)為基礎， React應用程式會透過AEM GraphQL API使用AEM內容片段內容，但不提供SPA內容的內容內容編寫。

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## 關於教學課程

本教學課程旨在說明如何更新遠端SPA或在AEM內容外執行的SPA，以使用和提供在AEM中製作的內容。

教學課程中的大部分活動都著重於JavaScript開發，但是會涵蓋以AEM為中心的關鍵層面。 這些方面包括定義內容在AEM中的製作和儲存位置，以及將SPA路由對應至AEM頁面。

本教學課程旨在搭配&#x200B;**AEM作為Cloud Service**&#x200B;使用，且由兩個專案組成：

1. __AEM專案__&#x200B;包含必須部署至AEM的設定和內容。
1. __WKND__ Appproject是要與AEM SPA編輯器整合的SPA

## 最新程式碼

+ 您可在`feature/spa-editor`分支的[GitHub](https://github.com/adobe/aem-guides-wknd-graphql)上找到本教學課程的程式碼。

## 必備條件

本教學課程需要下列項目：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0zip或更新版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼](https://github.com/adobe/aem-guides-wknd-graphql)

本教學課程假設：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 碼作為IDE
+ `~/Code/wknd-app`的工作目錄
+ 在`http://localhost:4502`上以作者服務形式執行AEM SDK
+ 使用密碼為`admin`的本機`admin`帳戶執行AEM SDK
+ 在`http://localhost:3000`上執行SPA

>[!NOTE]
>
> **需要設定本機開發環境的協助嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。


## 快速設定

快速設定可在15分鐘內讓您開始並執行WKND應用程式SPA和AEM SPA編輯器。 此加速設定會直接帶您進入教學課程的結束狀態，讓您探索在AEM SPA編輯器中編寫SPA。

+ [了解快速設定](./quick-setup.md)

## 1.設定AEM for SPA Editor

AEM設定是必要項目，才能將SPA與AEM SPA編輯器整合。 這些設定是透過AEM專案管理並部署。 在本章中，了解哪些設定是必要的以及如何定義。

+ [了解如何設定AEM for SPA Editor](./aem-configure.md)

## 2.BootstrapSPA

AEM SPA編輯器若要將SPA整合至其製作內容，必須對SPA新增一些內容。

+ [了解如何引導SPA for AEM SPA編輯器](./spa-bootstrap.md)

## 3.可編輯的固定元件

首先，探索將可編輯的「固定元件」新增至SPA。 這說明開發人員如何將特定的可編輯元件放入SPA中。 雖然作者可以變更元件的內容，但無法移除元件或變更其放置、位置或大小。

+ [了解可編輯的固定元件](./spa-fixed-component.md)

## 4.可編輯的容器元件

接下來，探索將可編輯的「容器元件」新增至SPA。 這說明開發人員如何將容器元件放入SPA。 容器元件可讓作者在其中放置允許的元件，並調整元件的版面配置。

+ [了解可編輯的容器元件](./spa-container-component.md)

## 5.動態路由和可編輯的元件

最後，將前幾章中解釋的概念用於動態路由；根據路由參數顯示不同內容的路由。 這說明如何使用AEM SPA編輯器，在以程式設計方式驅動和衍生的路由上製作內容。

+ [了解動態路由和可編輯的元件](./spa-dynamic-routes.md)

## 其他資源

+ [在AEM檔案中編輯外部SPA](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM元件 — React核心實作](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM元件 — Spa編輯器 — React Core實作](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
