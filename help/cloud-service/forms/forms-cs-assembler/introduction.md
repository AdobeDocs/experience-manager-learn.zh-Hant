---
title: 使用調用DDX操作在FormsCS中PDF操作
description: 使用調用DDX匯編PDF檔案。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9980
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 1%

---

# 簡介

在本課程中，我們將使用FormsCS對PDF文檔進行PDF處理和歸檔。 要從外部應用程式使用這些微服務，請執行以下步驟：

1. 生成技術帳戶的服AEM務憑據
1. 從服務憑據建立JSON Web令牌(JWT)，並為訪問令牌交換該憑據
1. 配置中技術帳戶的訪問權AEM限
1. 使用訪問令牌進行HTTP調用以處理PDF檔案/生成和驗證PDF/A檔案
