---
title: 使用通用編輯器編輯React應用程式
description: 瞭解如何使用通用編輯器編輯範例React應用程式的內容。
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 使用通用編輯器編輯React應用程式

瞭解如何使用通用編輯器編輯範例React應用程式的內容。 內容儲存在AEM的內容片段中，並使用GraphQL API擷取。

本教學課程會引導您完成設定本機開發環境的程式、檢測React應用程式以編輯其內容，以及使用通用編輯器編輯內容。

## 您能學到的內容

本教學課程涵蓋下列主題：

- Universal Editor的簡短概述
- 設定本機開發環境
   - **AEM SDK**：它使用GraphQL API為React應用程式提供儲存在內容片段中的內容。
   - **React應用程式**：顯示AEM內容的簡單使用者介面。
   - **通用編輯器服務**：繫結通用編輯器和AEM SDK的&#x200B;_通用編輯器服務_&#x200B;本機復本。
- 如何使用通用編輯器檢測React應用程式以編輯內容
- 如何使用通用編輯器編輯React應用程式內容


## 通用編輯器概觀

Universal Editor可讓內容作者和開發人員（前端和後端）檢視一些Universal Editor的主要優點：

- 專為在「所見即所得」(WYSIWYG)模式中編輯Headless和Headful內容所打造。
- 跨不同的前端技術(如React、Angular、Vue等)提供一致的內容編輯體驗。 因此，內容作者可以編輯內容，而不用擔心基礎的前端技術。
- 在前端應用程式中啟用通用編輯器需要非常少的工具。 因此，開發人員可以發揮最大生產力，並騰出時間專注在打造體驗上。
- 將關注點分隔給三個角色：內容作者、前端開發人員和後端開發人員，讓每個角色都能專注於核心責任。


## 範例React應用程式

本教學課程使用&#x200B;[**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons)作為範例React應用程式。 **WKND Teams** React應用程式會顯示團隊成員清單及其詳細資料。

標題、說明和團隊成員等團隊詳細資料會在AEM中儲存為&#x200B;_團隊_&#x200B;內容片段。 同樣地，姓名、履歷和個人資料圖片等個人詳細資料也會儲存為AEM中的&#x200B;_個人_&#x200B;內容片段。

AEM使用GraphQL API提供React應用程式的內容，而使用者介面是使用兩個React元件`Teams`和`Person`建置。

有對應的教學課程可學習如何建立&#x200B;**WKND Teams** React應用程式。 您可以在[這裡](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview)找到教學課程。

## 下一步

瞭解如何[設定本機開發環境](./local-development-setup.md)
