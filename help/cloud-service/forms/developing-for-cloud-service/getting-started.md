---
title: 安裝先決條件
description: 安裝必要的軟體以設定您的開發環境
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# 安裝必要的軟體

本教學課程將引導您完成建立AEM Forms專案所需的步驟，並使用IntelliJ和repo工具將AEM Forms專案與本機AEM執行個體同步。 您還將瞭解如何將專案新增到本機Git存放庫，並將本機Git存放庫推送到Cloud Manager存放庫。





本教學課程後續會參照此檔案夾結構。

* [安裝JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). 我已下載jdk-11.0.6_windows-x64_bin.zip
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html)例如，如果您已將Maven安裝在c：\maven資料夾中，則需要使用值C:\maven\apache-maven-3.6.0建立名為M2_HOME的環境變數。然後將M2_HOME\bin新增至路徑並儲存您的設定。

## 使用AEM專案原型建立Maven專案

* 建立名為的資料夾 **cloudmanager**（您可以為其指定任何名稱）
* 開啟命令提示視窗並瀏覽至 **c：\cloudmanager**
* 複製並貼上 [文字檔](assets/creating-maven-project.txt) 在命令提示字元視窗中。 您可能必須變更DarchetypeVersion=30，視 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 撰寫本文時，最新版本是30。
* 按下Enter鍵來執行命令。如果一切順利，您應該會看到建置成功訊息。

## 後續步驟

[安裝IntelliJ](./intellij-set-up.md)