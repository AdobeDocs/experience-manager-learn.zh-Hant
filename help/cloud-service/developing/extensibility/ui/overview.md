---
title: AEM 使用者介面擴展性
description: 了解使用 App Builder 建立擴充功能的 AEM 使用者介面擴展性。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 73f5d90d-e007-41a0-9bb3-b8f36a9b1547
duration: 50
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '275'
ht-degree: 100%

---

# AEM 使用者介面擴展性 {#aem-ui-extensibility}

Adobe Experience Manager (AEM) 提供可以建立數位體驗且功能強大的使用者介面 (UI)。為自訂和擴展使用者介面，Adobe 推出 App Builder。此項工具讓開發人員透過 JavaScript 和 React 增強使用者體驗，無需編寫複雜的程式碼。

App Builder 提供一個實施層，用於建立與 AEM 中定義完整之擴充點繫結的擴充功能。App Builder 與 AEM 緊密整合，能夠即時預覽和測試。用快速又精簡的方式將變更部署至 AEM。使用 App Builder，開發人員可以省下時間和精力，還可以快速完成原型設計以及與利害關係人共同作業。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_extensibility_app_builder"
>title="Adobe Developer App Builder 和 AEM Headless 快速入門"
>abstract="了解開發人員如何透過 AEM App Builder 使用 JavaScript 和 React 快速自訂和擴充 AEM UI，同時支援緊密整合和快速部署。"

## 開發 AEM 使用者介面擴充功能

AEM 的各個使用者介面有不同的擴充點，但基本概念是相同的。

下方提供的影片和逐步解說連結，會展示如何使用內容片段主控台擴充功能，以利說明各種活動。不過，需要注意的是，其中所涵蓋的概念適用於所有的 AEM 使用者介面擴充功能。

1. [建立 Adobe Developer Console 專案](./adobe-developer-console-project.md)
1. [初始化擴充功能](./app-initialization.md)
1. [註冊擴充功能](./extension-registration.md)
1. 實施擴充點

   擴充點及其實施會根據所擴展的使用者介面而有所不同。

   + [開發內容片段使用者介面擴充功能](./content-fragments/overview.md)

1. [開發模態視窗](./modal.md)
1. [開發 Adobe I/O Runtime 動作](./runtime-action.md)
1. [驗證擴充功能](./verify.md)
1. [部署擴充功能](./deploy.md)

## Adobe Developer 文件

Adobe Developer 內含關於 AEM 使用者介面擴展性的開發人員詳細資訊。請檢閱 [Adobe Developer 內容，了解更多技術詳細資訊](https://developer.adobe.com/uix/docs/)。
