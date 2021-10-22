---
title: 部署至開發環境
description: 從您的cloud manager存放庫分支部署程式碼
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---


# 部署至開發環境

在上一步中，我們將主分支從本機的Git存放庫推送至雲端管理員存放庫的MyFirstAF分支。

下一步是將程式碼部署至開發環境。
登入cloud manager並選取您的方案

選擇部署到開發，如下所示


![第一步](assets/deploy-first-step1.png)


選擇部署管道，如所示
![第一步](assets/deploy1.png)

選取原始碼和適當的Git分支
![第一步](assets/deploy2.png)
請務必更新變更

執行管道
![運行管道](assets/run-pipeline.png)

部署程式碼後，您應該會在AEM Forms的雲端服務例項中看到變更。
