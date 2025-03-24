---
title: 在Forms CS中使用PDF操作來叫用DDX操作
description: 使用呼叫DDX組合PDF檔案。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 713c4e9e-95ac-48e1-a7fc-2b3ec0b145e5
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
duration: 18
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# 簡介

在本課程中，我們將使用PDF CS來操作及封存PDFForms檔案。 若要從外部應用程式使用這些微服務，包括下列步驟：

1. 產生AEM技術帳戶的服務認證
1. 從服務憑證建立JSON Web權杖(JWT)，並將相同的權杖交換為存取權杖
1. 在AEM中設定技術帳戶的存取權
1. 使用存取權杖進行HTTP呼叫以操控PDF檔案/產生和驗證PDF/A檔案
