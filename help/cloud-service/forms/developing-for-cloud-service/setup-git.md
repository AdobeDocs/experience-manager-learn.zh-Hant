---
title: 安裝及設定Git
description: 初始化本機的Git存放庫
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 安裝Git


[安裝Git](https://git-scm.com/downloads). 您可以選取預設設定並完成安裝程式。
轉到命令提示符Navigate to c:\cloudmanager\aem-banking-app type in git —version。 您應該會看到系統上安裝的GIT版本

## 初始化本機Git存放庫

請確定您位於c:\cloudmanager\aem-banking-app folder

```
git init
```

以上命令會將專案初始化為Git本機存放庫

```
git add .
```

這會將所有專案檔案新增至Git存放庫，準備提交至Git存放庫

```
git commit -m "initial commit"
```

這會將檔案提交至Git存放庫



## 在本機的Git存放庫中註冊cloud manager存放庫

存取您的cloud manager存放庫
![訪問rep資訊](assets/cloud-manager-repo.png)
取得cloud manager存放庫憑證
![get-credentials](assets/cloud-manager-repo1.png)

將使用者名稱儲存在設定檔案中

```java
git config --global credential.username "gbedekar-adobe-com"
```

在設定檔案中儲存密碼

```java
git config --global user.password "bqwxfvxq2akawtqx3oztacb5wax5a7"
```

（密碼為您的cloud manager git存放庫密碼）

在本機的Git存放庫中註冊cloud manager git存放庫。 以下命令關聯 **adobe** 與遠端cloud manager git存放庫。 您本可以使用任何名稱，而非 **adobe**


```java
git remote add adobe https://git.cloudmanager.adobe.com/techmarketingdemos/Program2-p24107/
```

（請務必使用您的存放庫URL）

檢查遠程儲存庫是否已註冊

```java
git remote -v
```



