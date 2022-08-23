---
title: 安裝先決條件
description: 安裝必要的軟體以設定您的開發環境
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# 安裝所需軟體

本教程將指導您完成建立AEM Forms項目、使AEM Forms項目與您的本地實例同AEM步（使用IntelliJ和回購工具）所需的步驟。 您還將瞭解如何將項目添加到本地git儲存庫並將本地git儲存庫推送到雲管理器儲存庫。




本教程將參考此資料夾結構。

* [安裝JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows)。 我已下載jdk-11.0.6_windows-x64_bin.zip
* [馬文](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).例如，如果您在c:\maven folder中安裝了Maven，則需要建立一個名為M2_HOME的環境變數，其值為C:\maven\apache-maven-3.6.0。然後將M2_HOME\bin添加到路徑並保存設定。

## 使用項目原型創AEM建Maven項目

* 建立名為 **雲管理器**（您可以在c驅動器中提供任何名稱）
* 開啟命令提示符窗口並導航到 **c:\cloudmanager**
* 複製和貼上 [文本檔案](assets/creating-maven-project.txt) 命令提示符窗口中。 您可能必鬚根據 [最新版本](https://github.com/adobe/aem-project-archetype/releases)。 最新版本是30歲，
* 按Enter鍵執行命令。如果一切正常，您應看到生成成功消息。
