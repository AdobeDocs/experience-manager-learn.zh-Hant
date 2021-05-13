---
title: 開始使用SPAEditor和Remote SPA —— 概觀
description: 歡迎使用多部分教學課程，讓開發人員可使用編輯器來擴充現SPA有的AEM遠端AEM內容SPA。
topic: 無頭、SPA開發
feature: 編SPA輯器、核心元件、API、開發
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

歡迎使用多部分教學課程，讓開發人員可使用編輯器來擴充現有的React-based（或Next.js）SPA遠端AEM，並加AEM入可編輯SPA內容。

本教學課程以[WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)為基礎，此React應用程式會透過AEMAEMGraphQL API取用內容片段內容，但不提供任何內容的內容內SPA容製作。

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## 關於教學課程

本教學課程旨在說明如何更SPA新遠端(或SPA執行於上下文之外的AEM遠端)，以使用和傳送在中編寫的AEM內容。

教學課程中的大部分活動都著重於JavaScript開發，但主要方面則圍繞其中AEM。 這些方面包括定義內容的創作和儲存位置AEM，以及將路SPA由映射AEM到頁。

本教學課程旨在與&#x200B;**作為AEMCloud Service**&#x200B;一起使用，由兩個項目組成：

1. __ProjectAEM__&#x200B;包含必須部署到的配置和內容AEM。
1. __WKND__ Appproject是SPA要與編輯器整AEM合SPA的

## 最新程式碼

+ 本教學課程的程式碼可在`feature/spa-editor`分支的[GitHub](https://github.com/adobe/aem-guides-wknd-graphql)上找到。

## 必備條件

本教學課程需要：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip或更新版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼](https://github.com/adobe/aem-guides-wknd-graphql)

本教學課程假設：

+ [Microsoft® Visual Studio代](https://visualstudio.microsoft.com/) 碼作為IDE
+ `~/Code/wknd-app`的工作目錄
+ 在&lt;AEMa0/>上以作者服務的身分執行SDK`http://localhost:4502`
+ 使用密碼AEM為`admin`的本機`admin`帳戶執行SDK
+ 在SPA`http://localhost:3000`上運行

>[!NOTE]
>
> **需要協助您設定本機開發環境嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。


## 快速設定

快速設定功能可讓您在15分鐘內使用WKND應用程SPA式和AEM編SPA輯器啟動並執行。 此加速設定會直接帶您進入教學課程的結束狀態，讓您探索在編輯器中SPA制AEM作SPA內容。

+ [瞭解快速設定](./quick-setup.md)

## 1.為編AEM輯器設SPA定

您必AEM須進行設定，才能將SPA與編AEM輯器整SPA合。 這些配置通過項目進行管理和部AEM署。 在本章中，瞭解哪些配置是必需的以及如何定義配置。

+ [瞭解如何為編AEM輯器設SPA定](./aem-configure.md)

## 2.BootstrapSPA

For AEM SPA   Editor to integrate a it SPA&#39;s authoring context, must be additions to theSPA.

+ [瞭解如何引導SPA編AEM輯SPA器](./spa-bootstrap.md)

## 3.可編輯的固定元件

首先，探索將可編輯的「固定元件」新增至SPA。 這說明開發人員如何在中放置特定的可編輯元SPA件。 雖然作者可以變更元件的內容，但無法移除元件或變更其位置、位置或大小。

+ [瞭解可編輯的固定元件](./spa-fixed-component.md)

## 4.可編輯的容器元件

接著，探索將可編輯的「容器元件」新增至SPA。 這說明開發人員如何將容器元件置於SPA。 容器元件可讓作者將允許的元件置入其中，並調整元件的版面配置。

+ [瞭解可編輯的容器元件](./spa-container-component.md)

## 5.動態路由和可編輯的元件

最後，將前一章中闡述的概念用於動態路由；根據路由參數顯示不同內容的路由。 這說明了如AEM何使用SPAEditor在以程式設計方式驅動和衍生的路由上製作內容。

+ [瞭解動態路由和可編輯的元件](./spa-dynamic-routes.md)

## 其他資源

+ [在檔案中編SPA輯外AEM部](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEMWCM元件- React Core實施](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEMWCM元件- Spa編輯器- React Core實作](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
