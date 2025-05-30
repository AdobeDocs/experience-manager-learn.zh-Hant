---
title: SPA編輯器和遠端SPA快速入門 — 概觀
description: 歡迎使用多部分教學課程，開發人員若想使用SPA SPA編輯器透過可編輯的AEM內容來增強現有的遠端AEM。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 5%

---

# 概觀

{{edge-delivery-services}}

開發人員若想使用AEM SPA編輯器將現有的React型（或Next.js）遠端SPA擴充為可編輯的AEM內容，歡迎使用多部分教學課程。

此教學課程以[WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=zh-Hant)為基礎，這是一種React應用程式，它會透過AEM的GraphQL API使用AEM內容片段內容，但不會提供任何內容內嵌撰寫SPA內容。

>[!VIDEO](https://video.tv.adobe.com/v/3444858?quality=12&learn=on&captions=chi_hant)

## 關於教學課程

本教學課程旨在說明如何更新遠端SPA或在AEM環境之外執行的SPA，以使用及傳遞在AEM中撰寫的內容。

本教學課程中的大部分活動都著重於JavaScript開發，但也涵蓋圍繞AEM的關鍵層面。 這些方面包括定義內容在AEM中的製作及儲存位置，以及將SPA路由對應至AEM頁面。

此教學課程在設計上與&#x200B;**AEM as a Cloud Service**&#x200B;搭配使用，由兩個專案組成：

1. __AEM專案__&#x200B;包含必須部署至AEM的組態和內容。
1. __WKND App__&#x200B;專案是要與AEM的SPA編輯器整合的SPA

## 最新程式碼

+ 您可以在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial)的`remote-spa-tutorial`資料夾中找到此教學課程程式碼的起點。

## 先決條件

本教學課程需要下列內容：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hant)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip或更新版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始程式碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

本教學課程假設：

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/)作為IDE
+ `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`的工作目錄
+ 在`http://localhost:4502`上以Author服務的形式執行AEM SDK
+ 使用密碼為`admin`的本機`admin`帳戶執行AEM SDK
+ 正在`http://localhost:3000`上執行SPA

>[!NOTE]
>
> **需要協助設定您的本機開發環境嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hant)。

## 1.設定AEM for SPA編輯器

需要AEM設定才能將SPA與AEM SPA編輯器整合。 這些設定是透過AEM專案管理和部署的。 在本章中，瞭解需要哪些設定以及如何定義它們。

+ [瞭解如何為SPA編輯器設定AEM](./aem-configure.md)

## 2.BootstrapSPA

若要讓AEM SPA Editor將SPA整合至其編寫內容，必須對SPA新增一些內容。

+ [瞭解如何為AEM SPA編輯器啟動SPA](./spa-bootstrap.md)

## 3.可編輯的固定元件

首先，探索新增可編輯的「固定元件」至SPA。 這說明了開發人員如何在SPA中放置特定的可編輯元件。 雖然作者可以變更元件的內容，但他們無法移除元件或變更其位置、位置或大小。

+ [瞭解可編輯的固定元件](./spa-fixed-component.md)

## 4.可編輯的容器元件

接下來，探索將可編輯的「容器元件」新增至SPA。 這說明了開發人員如何在SPA中放置容器元件。 容器元件可讓作者將允許的元件放置在其中，並調整元件的版面。

+ [瞭解可編輯的容器元件](./spa-container-component.md)

## 5.動態路由及可編輯元件

最後，使用前幾章中說明的概念來動態路由；根據路由的引數顯示不同內容的路由。 以下說明如何使用AEM SPA Editor來編寫程式化驅動和衍生的路由內容。

+ [瞭解動態路由和可編輯的元件](./spa-dynamic-routes.md)

## 其他資源

+ [AEM SPA React可編輯元件](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
