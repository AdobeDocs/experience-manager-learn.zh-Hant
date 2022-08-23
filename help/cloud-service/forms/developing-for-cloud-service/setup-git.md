---
title: 安裝和設定Git
description: 初始化本地Git儲存庫
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 安裝Git


[安裝Git](https://git-scm.com/downloads)。 您可以選擇預設設定並完成安裝過程。
轉到命令提示符導航到c:\cloudmanager\aem-banking-app type in git  — 版本。 您應看到系統上安裝的GIT版本

## 初始化本地Git儲存庫

確保您位於c:\cloudmanager\aem-banking-app folder目錄中

```
git init
```

以上命令將將項目初始化為Git本地儲存庫

```
git add .
```

這會將所有項目檔案添加到Git儲存庫，以便提交到Git儲存庫

```
git commit -m "initial commit"
```

這會將檔案提交到Git儲存庫



## 將雲管理器儲存庫註冊到我們的本地Git儲存庫

訪問雲管理器回購
![訪問rep資訊](assets/cloud-manager-repo.png)
獲取雲管理器回購憑據
![獲取憑據](assets/cloud-manager-repo1.png)

將用戶名保存在配置檔案中

```java
git config --global credential.username "gbedekar-adobe-com"
```

在配置檔案中保存密碼

```java
git config --global user.password "XXXX"
```

（密碼是您的雲管理器Git儲存庫密碼）

將雲管理器Git儲存庫註冊到您的本地Git儲存庫。 下面的命令關聯 **綁定** 遠程雲管理器git儲存庫。 你可以用任何名稱 **綁定**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

（確保使用儲存庫URL）

檢查遠程儲存庫是否已註冊

```java
git remote -v
```
