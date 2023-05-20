---
title: 部署到開發環境
description: 從雲管理器儲存庫分支部署代碼
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
source-git-commit: f0beb8b32aa25d6c26243879c9c0e42095488e23
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# 部署到開發環境

在上一步中，我們將主分支從本地Git儲存庫推送到雲管理器儲存庫的MyFirstAF分支。

下一步是將代碼部署到開發環境中。
登錄到雲管理器並選擇您的程式

選擇部署到開發人員，如下所示


![第一步](assets/deploy-first-step1.png)


如所示選擇部署管道
![第一步](assets/deploy1.png)

選擇原始碼和相應的Git分支
![第一步](assets/deploy2.png)
確保更新更改

運行管線
![運行管道](assets/run-pipeline.png)

部署代碼後，您應看到雲服務實例中的更改，即AEM Forms。
