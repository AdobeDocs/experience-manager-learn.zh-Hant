---
title: AEM SPA Editor 和 React 快速入門
description: 建立您的第一個 React 單頁應用程式 (SPA)，該應用程式可在 Adobe Experience Manager (AEM) 中使用 WKND SPA 進行編輯。了解如何使用 React JS 框架和 AEM 的 SPA 編輯器建立 SPA。此多部分教學課程會逐步引導您為虛擬生活風格品牌 WKND 實作 React 應用程式。教學課程涵蓋 SPA 從開始到結束的建立過程及其與 AEM 的整合。
version: Experience Manager as a Cloud Service
jira: KT-5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
duration: 71
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '417'
ht-degree: 100%

---

# 建立您在 AEM 的第一個 React SPA {#overview}

{{edge-delivery-services}}

歡迎使用專為初次接觸 Adobe Experience Manager (AEM) 中 **SPA Editor** 功能的開發人員設計的多部分教學課程。此教學課程會逐步解說虛構生活風格品牌 WKND 實施 React 應用程式的情形。React 應用程式是為了搭配 AEM 的 SPA 編輯器進行部署而進行開發與設計的，而 SPA 編輯器會將 React 元件對應至 AEM 元件。部署至 AEM 的完成 SPA 即可使用 AEM 傳統的內嵌編輯工具動態製作。

![實作的最終 SPA](assets/wknd-spa-implementation.png)

*WKND SPA 實作*

## 關於

此教學課程旨在與 **AEM as a Cloud Service** 搭配使用，並向下相容於 **AEM 6.5.4+** 和 **AEM 6.4.8+**。

## 最新的程式碼

您可以在 [GitHub](https://github.com/adobe/aem-guides-wknd-spa) 上找到教學課程的所有程式碼。

[最新的程式碼基底](https://github.com/adobe/aem-guides-wknd-spa/releases)可以當做可下載的 AEM 套件使用。

## 必備條件

開始進行此教學課程前，您需要具備以下條件：

* HTML、CSS 和 JavaScript 的基本知識
* 對於 [React](https://reactjs.org/tutorial/tutorial.html) 有基本的認識

*雖然不一定需要，但對於[開發傳統的 AEM Sites 元件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)有基本的了解也會有幫助。*

## 本機開發環境 {#local-dev-environment}

本機開發環境是完成此教學課程不可或缺的條件。螢幕截圖和影片都是使用在 Mac OS 環境上執行的 AEM as a Cloud Service SDK 擷取，並將 [Visual Studio Code](https://code.visualstudio.com/) 當做 IDE 使用。除非另有註明，否則命令和程式碼應不受本機作業系統的限制。

### 必要的軟體

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)、[AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=zh-Hant#aem-65) 或 [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=zh-Hant#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 或更新版本)
* [Node.js](https://nodejs.org/en/) 和 [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **AEM as a Cloud Service 的新手嗎？** 請參閱[以下指南以使用 AEM as a Cloud Service SDK 設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5 的新手嗎？** 請參閱[以下指南以設定本機開發環境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 後續步驟 {#next-steps}

您還在等什麼？！請導覽至[建立專案](create-project.md)章節，開始進行此教學課程並了解如何使用 AEM 專案原型產生啟用 SPA Editor 的專案。
