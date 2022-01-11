---
title: 將AEM專案推送至cloud manager存放庫
description: 將本機的Git存放庫推送至Cloud Manager存放庫
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# 將AEM專案推送至cloud manager git repo

在上一步中，我們將AEM專案與AEM例項中建立的最適化Forms和主題同步。
現在，我們需要將這些變更新增至本機的git存放庫，然後將本機的git存放庫推送至cloud manager git存放庫。
開啟命令提示符並導航到c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .
```

這會將新檔案新增至本機Git存放庫的預備分支

```
git commit -m "My First AF"
```

這會將檔案提交至本機Git存放庫的主分支

```
git push -f bankingapp master:"MyFirstAF"
```

在上述命令中，我們將主分支從本機的git存放庫推送至雲端管理員存放庫的MyFirstAF分支，該分支由封存應用程式易記名稱所識別。
