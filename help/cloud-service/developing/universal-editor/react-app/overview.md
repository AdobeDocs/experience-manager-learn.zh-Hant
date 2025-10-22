---
title: 使用通用編輯器編輯 React 應用程式
description: 了解如何使用通用編輯器編輯範例 React 應用程式的內容。
short-description: 了解如何使用通用編輯器編輯範例 React 應用程式的內容。內容儲存在 AEM 的內容片段中，並使用 GraphQL API 擷取。
version: Experience Manager as a Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 100%

---

# 使用通用編輯器編輯 React 應用程式

了解如何使用通用編輯器編輯範例 React 應用程式的內容。內容儲存在 AEM 的內容片段中，並使用 GraphQL API 擷取。

本教學課程會引導您完成本機開發環境設定、配置 React 應用程式以編輯其內容，以及使用通用編輯器編輯內容的流程。

## 學習內容

本教學課程涵蓋以下主題：

- 通用編輯器簡略概觀
- 設定本機開發環境
   - **AEM SDK**：提供儲存於內容片段中供使用 GraphQL API 的 React 應用程式使用的內容。
   - **React 應用程式**：一個簡單的使用者介面，會顯示來自 AEM 的內容。
   - **通用編輯器服務**：_通用編輯器服務的本機副本_，將通用編輯器和 AEM SDK 連結在一起。
- 如何配置 React 應用程式以便使用通用編輯器來編輯內容
- 如何使用通用編輯器編輯 React 應用程式內容


## 通用編輯器概觀

通用編輯器賦予內容作者和開發人員 (前端及後端) 更多能力，我們來看看通用編輯器的一些主要優勢：

- 能夠以所見即所得 (WYSIWYG) 模式編輯無周邊和有周邊內容。
- 針對不同的前端技術 (如 React、Angular、Vue 等) 提供一致的內容編輯體驗。因此，內容作者可以編輯內容，而不必擔心基礎的前端技術。
- 只需極少的配置即可在前端應用程式中啟用通用編輯器。因此能夠發揮開發人員的最大生產力，並讓他們能夠專注於建置體驗。
- 內容作者、前端開發人員和後端開發人員三個角色分別關注不同的事項，讓每個角色都能夠專注於其核心職責。


## 範例 React 應用程式

本教學課程使用 [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) 作為範例 React 應用程式。**WKND Teams** React 應用程式會顯示團隊成員清單及其詳細資訊。

團隊詳細資訊，例如標題、說明和團隊成員，會儲存在 AEM 中成為 _Team_ 內容片段。同樣地，姓名、個人簡介和個人資料圖片等人員詳細資訊會儲存在 AEM 中成為 _Person_ 內容片段。

React 應用程式的內容由 AEM 透過 GraphQL API 提供，而使用者介面使用兩個 React 元件 `Teams` 和 `Person` 來進行建置。

提供相對應的教學課程，可供學習如何建置 **WKND Teams** React 應用程式。您可以在[此處](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview)找到教學課程。

## 下一步

了解如何[設定本機開發環境](./local-development-setup.md)。
