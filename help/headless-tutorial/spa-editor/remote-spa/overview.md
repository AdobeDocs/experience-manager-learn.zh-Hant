---
title: SPA 編輯器和遠端 SPA 快速入門 - 概觀
description: 歡迎瀏覽此多部分教學課程，適合想要透過 AEM SPA 編輯器為現有的遠端 SPA 新增可編輯 AEM 內容的開發人員。
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
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: ht
source-wordcount: '571'
ht-degree: 100%

---

# 概觀

{{edge-delivery-services}}

歡迎瀏覽此多部分教學課程，適合想要透過 AEM SPA 編輯器為現有基於 React (或 Next.js) 的遠端 SPA 新增可編輯 AEM 內容的開發人員。

本教學課程以 [WKND GraphQL 應用程式](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)為基礎，這是透過 AEM 的 GraphQL API 使用 AEM 內容片段內容的一個 React 應用程式，但不提供任何 SPA 內容情境式製作功能。

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## 關於本教學課程

本教學課程旨在說明如何更新遠端 SPA，或者不在 AEM 環境內執行的 SPA，以便能夠使用及傳遞在 AEM 中製作的內容。

本教學課程中的大部分活動將焦點放在 JavaScript 開發，但也涵蓋關於 AEM 的重要層面。這些方面包括定義 AEM 中製作和儲存內容的位置，以及將 SPA 路由對應至 AEM 頁面。

本教學課程旨在與 **AEM as a Cloud Service** 搭配使用，並由兩個專案組成：

1. __AEM 專案__&#x200B;包含必須部署至 AEM 的設定和內容。
1. __WKND 應用程式__&#x200B;專案是要與 AEM 的 SPA 編輯器整合的 SPA

## 最新的程式碼

+ 本教學課程之程式碼的起點可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) 的 `remote-spa-tutorial` 資料夾中找到。

## 先決條件

本教學課程需要以下項目：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=zh-Hant)
+ [Node.js v18](https://nodejs.org/en/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip 或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql 原始程式碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

本教學課程假設：

+ 以 [Microsoft® Visual Studio 程式碼](https://visualstudio.microsoft.com/)作為 IDE
+ 工作目錄是 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ 執行 AEM SDK，作為 `http://localhost:4502` 上的製作服務
+ 使用具備 `admin` 密碼的本機 `admin` 帳戶執行 AEM SDK
+ 在 `http://localhost:3000` 上執行 SPA

>[!NOTE]
>
> **需要協助設定您的本機開發環境嗎？若要使用 AEM as a Cloud Service SDK 設定本機開發環境，**&#x200B;請參閱[以下指南](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

## &#x200B;1. 設定 AEM 以便支援 SPA 編輯器

需要進行 AEM 設定才能將 SPA 與 AEM SPA 編輯器整合。這些設定會透過 AEM 專案進行管理和部署。在此章節中，了解哪些是必要設定以及如何定義那些設定。

+ [了解如何設定 AEM 以支援 SPA 編輯器](./aem-configure.md)

## &#x200B;2. 啟動 SPA

若要讓 AEM SPA 編輯器能夠將 SPA 整合到其製作環境中，必須在 SPA 中添加一些內容。

+ [了解如何為 AEM SPA 編輯器啟動 SPA。](./spa-bootstrap.md)

## &#x200B;3. 可編輯的固定元件

首先，探索在 SPA 中新增可編輯的「固定元件」。這樣會說明開發人員如何在 SPA 中放置特定的可編輯元件。雖然作者可以變更元件的內容，但他們不能刪除元件或變更其放置環境、定位或大小。

+ [了解可編輯的固定元件](./spa-fixed-component.md)

## &#x200B;4. 可編輯的容器元件

接下來，探索在 SPA 中新增可編輯的「容器元件」。這會說明開發人員如何在 SPA 中放置容器元件。作者能夠在容器元件中放置經允許的元件，並調整元件的版面。

+ [了解可編輯的容器元件](./spa-container-component.md)

## &#x200B;5. 動態路由和可編輯的元件

最後，使用前幾個章節所解釋的概念來實現動態路由；會根據路由的參數而顯示不同內容的路由。這會說明如何使用 AEM SPA 編輯器，在以程式設計方式驅動及推導出來的路由上製作內容。

+ [了解動態路由和可編輯的元件](./spa-dynamic-routes.md)

## 其他資源

+ [AEM SPA React 可編輯的元件](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
