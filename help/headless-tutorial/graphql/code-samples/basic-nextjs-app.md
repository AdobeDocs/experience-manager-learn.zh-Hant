---
title: 基本Next.js應用程式
description: 基本的Next.js應用程式，可顯示WKND冒險清單及其詳細資訊
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# 基本Next.js應用程式

此[Next.js](https://nextjs.org/)應用程式示範如何使用AEM的GraphQL API透過持續性查詢來查詢內容。 此應用程式會呈現WKND Adventures的可篩選內容，並在選取冒險後顯示該冒險的完整詳細資訊。

此程式碼：

+ 連線至AEM Publish服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all`和`wknd-shared/adventures-by-slug`

如需如何建立此Next.js應用程式的深入檢閱，請檢閱[Next.js應用程式範例](../example-apps/next-js.md)。

>[!IMPORTANT]
>
> Codesandbox.io不支援在內嵌IDE中編輯Next.js應用程式。 若要編輯此程式碼範例，請[直接在codesandbox.io](https://codesandbox.io/s/wknd-next-js-app-u8x5f8)上開啟Next.js應用程式。
