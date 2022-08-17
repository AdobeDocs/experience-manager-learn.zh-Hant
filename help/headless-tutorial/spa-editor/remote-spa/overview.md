---
title: 編輯器和遠SPA程入門SPA — 概述
description: 歡迎使用多部分教程，向希望使用編輯器擴展現有遠程SPA內容AEM的開發AEM人員學習。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 5%

---

# 概觀

歡迎使用多部分教程，向希望使用編輯器擴展現有基於React（或Next.js）的遠程SPA可編AEM輯內容AEM的開SPA發人員。

本教程以 [WKND GraphQL應用](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)，一個在GraphQL API上AEM使用內容片段內AEM容的反應應用，但不提供任何內容的上下文SPA創作。

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## 關於教程

本教程旨在說明如SPA何更新遠SPA程或運行在上下文AEM之外的內容以使用和傳送在中編寫的AEM內容。

本教程中的大多數活動都側重於JavaScript開發，但涉及的關鍵方面卻圍繞AEM其展開。 這些方面包括定義內容創作和儲存在何處以及將路AEM由映SPA射到AEM頁面。

本教程旨在 **AEMas a Cloud Service** 由兩個項目組成：

1. 的 __項AEM目__ 包含必須部署到的配置和內AEM容。
1. __WKND應用__ 項目SPA與編AEM輯SPA器

## 最新代碼

+ 本教程的代碼可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql) 的 `feature/spa-editor` 分支。

## 必備條件

本教程需要以下內容：

+ [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [馬文3.6+](https://maven.apache.org/)
+ [蠢貨](https://git-scm.com/downloads)
+ [aem guides-wknd.all-2.1.0.zip或更高版本](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql原始碼(分支：功能/spa編輯器](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)

本教程假定：

+ [Microsoft® Visual Studio代碼](https://visualstudio.microsoft.com/) 作為IDE
+ 工作目錄 `~/Code/wknd-app`
+ 將SDKAEM作為作者服務運行於 `http://localhost:4502`
+ 使用AEM本地 `admin` 密碼帳戶 `admin`
+ 運行SPA `http://localhost:3000`

>[!NOTE]
>
> **需要幫助設定您的本地開發環境嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。


## 快速設定

快速安裝可在15分鐘內使用WKND應用和編輯SPA器啟AEM動並SPA運行。 此加速設定將直接引導您進入本教程的結束狀態，使您可以在編輯器中SPA瀏覽AEM創SPA作。

+ [瞭解快速設定](./quick-setup.md)

## 1。為編AEM輯器配SPA置

與編AEM輯器整合SPA需AEM要配置 這些配置通過項目進行管AEM理和部署。 在本章中，瞭解哪些配置是必需的以及如何定義它們。

+ [瞭解如何為編AEM輯器配SPA置](./aem-configure.md)

## 2.BootstrapSPA

編AEM輯SPA器要將SPA整合到其創作上下文中，必須添加幾SPA個。

+ [瞭解如何引導SPA編AEM輯SPA器](./spa-bootstrap.md)

## 3.可編輯的固定元件

首先，瀏覽向中添加可編輯的「固定元件」SPA。 這說明了開發人員如何將特定可編輯元件放在SPA中。 雖然作者可以更改元件的內容，但他們不能刪除元件或更改其放置、定位或大小。

+ [瞭解可編輯的固定元件](./spa-fixed-component.md)

## 4.可編輯的容器元件

接下來，瀏覽向中添加可編輯的「容器元件」SPA。 這說明了開發人員如何將容器元件放在SPA中。 容器元件允許作者將允許的元件放入其中，並調整元件的佈局。

+ [瞭解可編輯的容器元件](./spa-container-component.md)

## 5.動態路由和可編輯元件

最後，將前幾章所闡述的概念用於動態路由；根據路由的參數顯示不同內容的路由。 這說明AEM了SPA如何使用編輯器在以寫程式方式驅動和派生的路由上編寫內容。

+ [瞭解動態路由和可編輯元件](./spa-dynamic-routes.md)

## 其他資源

+ [編輯文檔SPA內的AEM外部](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/hybrid/editing-external-spa.html)
+ [WCMAEM元件 — 反應核心實施](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [WCMAEM元件 — Spa編輯器 — React Core實施](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
