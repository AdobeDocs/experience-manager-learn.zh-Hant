---
title: 安裝及設定Git
description: 初始化您的本機Git存放庫
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# 安裝Git


[安裝Git](https://git-scm.com/downloads)。 您可以選取預設設定並完成安裝程式。
移至命令提示字元
導覽至c：\cloudmanager\aem-banking-app
輸入git —version。 您應該會看到系統上已安裝的GIT版本

## 初始化本機Git存放庫

確定您位於c：\cloudmanager\aem-banking-app資料夾中

```
git init
```

上述命令會將專案初始化為Git本機存放庫

```
git add .
```

這會將所有專案檔案新增到Git存放庫，以準備提交到Git存放庫

```
git commit -m "initial commit"
```

這會將檔案提交到Git存放庫



## 在本機Git存放庫中註冊Cloud Manager存放庫

存取您的Cloud Manager存放庫
![存取代表資訊](assets/cloud-manager-repo.png)
取得Cloud Manager存放庫認證
![取得認證](assets/cloud-manager-repo1.png)

將使用者名稱儲存在設定檔案中

```java
git config --global credential.username "gbedekar-adobe-com"
```

將密碼儲存在設定檔案中

```java
git config --global user.password "XXXX"
```

（密碼是您的cloud manager git存放庫密碼）

在本機git存放庫中註冊cloud manager git存放庫。 以下命令會將&#x200B;**bankingapp**&#x200B;與遠端cloud manager git存放庫建立關聯。 您可以使用任何名稱來代替&#x200B;**銀行應用程式**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

（請務必使用您的存放庫URL）

檢查遠端存放庫是否已註冊

```java
git remote -v
```

## 後續步驟

[在IntelliJ中同步AEM與AEM專案](./intellij-and-aem-sync.md)
