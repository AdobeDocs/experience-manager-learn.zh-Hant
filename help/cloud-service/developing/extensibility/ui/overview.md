---
title: AEM UI 的擴充性
description: 瞭解如何使用App Builder建立擴充功能的AEM UI擴充性。
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
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 15%

---

# AEM UI 的擴充性 {#aem-ui-extensibility}

Adobe Experience Manager (AEM)提供強大的使用者介面(UI)，可建立數位體驗。 為了自訂和擴充UI，Adobe推出了App Builder。 此工具可讓開發人員在不使用JavaScript和React複雜編碼的情況下增強使用者體驗。

App Builder提供實作層，用於建立與在AEM中妥善定義擴充功能點繫結的擴充功能。 App Builder與AEM緊密整合，可即時預覽和測試。 將變更部署至AEM既快速又簡化。 透過使用App Builder，開發人員可節省時間和精力，實現快速原型製作並與利害關係人合作。

>[!CONTEXTUALHELP]
>id="aemcloud_learn_extensibility_app_builder"
>title="Adobe Developer App Builder 和 AEM Headless 快速入門"
>abstract="了解開發人員如何透過 AEM App Builder 使用 JavaScript 和 React 快速自訂和擴充 AEM UI，同時支援緊密整合和快速部署。"

## 開發AEM UI擴充功能

AEM的各種UI有不同的擴充點，但基本概念相同。

以下連結提供的影片和逐步說明展示了如何使用內容片段主控台擴充功能來說明各種活動。 不過，請務必注意，涵蓋的概念可套用至所有AEM UI擴充功能。

1. [建立Adobe Developer Console專案](./adobe-developer-console-project.md)
1. [初始化擴充功能](./app-initialization.md)
1. [註冊擴充功能](./extension-registration.md)
1. 實作擴充點

   擴充功能點及其實施會因擴充的UI而異。

   + [開發內容片段UI擴充功能](./content-fragments/overview.md)

1. [開發模型](./modal.md)
1. [開發Adobe I/O Runtime動作](./runtime-action.md)
1. [驗證擴充功能](./verify.md)
1. [部署擴充功能](./deploy.md)

## Adobe Developer檔案

Adobe Developer包含有關AEM UI擴充性的開發人員詳細資訊。 請檢閱[Adobe Developer內容，以取得進一步的技術詳細資訊](https://developer.adobe.com/uix/docs/)。
