---
title: 安裝先決條件
description: 安裝必要的軟體以設定您的開發環境
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---


# 安裝所需軟體

本教學課程將引導您完成建立AEM Forms專案、使用IntelliJ和存放庫工具將AEM Forms專案與本機AEM例項同步所需的步驟。 您也會學習如何將專案新增至本機Git存放庫，以及將本機Git存放庫推送至雲端管理程式存放庫。




本教學課程將參考此資料夾結構。

* [安裝JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). 我已下載jdk-11.0.6_windows-x64_bin.zip
* [馬文](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).例如，如果您已在c:\maven folder中安裝Maven，則需要建立一個名為M2_HOME的環境變數，該環境變數的值為C:\maven\apache-maven-3.6.0。然後將M2_HOME\bin添加到路徑中，並保存您的設定。

## 使用AEM專案原型建立Maven專案

* 建立名為 **cloudmanager**（可在c驅動器中指定任何名稱）
* 開啟命令提示窗口並導航到 **c:\cloudmanager**
* 複製並貼上 [文字檔案](assets/creating-maven-project.txt) 在命令提示符窗口中。 您可能必鬚根據 [最新版本](https://github.com/adobe/aem-project-archetype/releases). 最新版本是撰寫本文時的30版。
* 按Enter鍵執行命令。如果一切正常，您應該會看到生成成功消息。





