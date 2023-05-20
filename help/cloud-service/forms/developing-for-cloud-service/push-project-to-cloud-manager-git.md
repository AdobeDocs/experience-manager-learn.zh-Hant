---
title: 將項AEM目推送到雲管理器儲存庫
description: 將本地Git儲存庫推送到雲管理器儲存庫
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

# 將項AEM目推送到雲管理器git回購

在上一步中，我們將項目與AEM實例中建立的自適應Forms和主題同AEM步。
現在，我們需要將這些更改添加到我們的本地Git儲存庫，然後將本地Git儲存庫推送到雲管理器Git儲存庫。
開啟命令提示符並導航到c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .
```

這會將新檔案添加到本地Git儲存庫的階段分支

```
git commit -m "My First AF"
```

這會將檔案提交到本地Git儲存庫的主分支

```
git push -f bankingapp master:"MyFirstAF"
```

在以上命令中，我們將主分支從本地Git儲存庫推入雲管理器儲存庫的MyFirstAF分支，該儲存庫由銀行應用友好名稱標識。
