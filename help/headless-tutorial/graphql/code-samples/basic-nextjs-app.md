---
title: 基本Next.js應用
description: 一個基本的Next.js應用，顯示WKND冒險及其詳細資訊清單
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11368
thumbnail: KT-11368.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 2d4396dc-2346-4561-b040-eba0ab62a96f
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---

# 基本Next.js應用

此 [Next.js](https://nextjs.org/) app演示了如何使用GraphQLAPIAEM使用永續查詢來查詢內容。 此應用程式將可過濾WKND冒險項，在選擇冒險項時，將顯示冒險項的完整詳細資訊。

此代碼：

+ 連接到AEM發佈服務，不需要身份驗證
+ 使用WKND的永續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`

要更深入地查看此Next.js應用是如何構建的，請查看 [示例Next.js應用文檔](../example-apps/next-js.md)。

>[!IMPORTANT]
>
> Codesandbox.io不支援在嵌入式IDE中編輯Next.js應用程式。 要編輯此代碼示例， [直接在codesandbox.io上開啟Next.js應用](https://codesandbox.io/s/wknd-next-js-app-u8x5f8)。
