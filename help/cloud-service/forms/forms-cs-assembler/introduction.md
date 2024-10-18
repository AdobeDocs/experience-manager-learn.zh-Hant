---
title: 使用叫用DDX操作在Forms CS中進行PDF操作
description: 使用呼叫DDX組合PDF檔案。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
duration: 18
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# 簡介

在本課程中，我們將使用Forms CS的PDF操作和PDF檔案封存。 若要從外部應用程式使用這些微服務，包括下列步驟：

1. 產生AEM技術帳戶的服務認證
1. 從服務憑證建立JSON Web權杖(JWT)，並將相同的權杖交換為存取權杖
1. 設定AEM中技術帳戶的存取權
1. 使用存取權杖進行HTTP呼叫以操控PDF檔案/產生和驗證PDF/A檔案
