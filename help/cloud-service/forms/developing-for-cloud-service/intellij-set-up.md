---
title: 安裝IntelliJ社區版
description: 安裝AEM專案並匯入IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# 安裝IntelliJ

安裝 [IntelliJ社群版](https://www.jetbrains.com/idea/download/#section=windows). 您可以在安裝期間接受建議的預設設定。

## 匯入AEM專案

* 啟動IntelliJ
* 匯入您在先前步驟中建立的AEM專案。 匯入專案後，畫面應該會像這樣 ![aem-banking-app](assets/aem-banking-app.png). 您通常會使用核心、ui.apps、ui.config和ui.content子專案。
* 如果您沒有看見Maven和終端機視窗，請前往檢視 — >工具視窗，然後選取Maven和終端機

## 新增字型模組

如果您想在PDF檔案中使用自訂字型，則需將自訂字型推送至AEM Forms CS執行個體。 請遵循下列步驟

* 建立名為 **字型** C:\CloudManager\aem-banking-application
* 擷取 [font.zip](assets/fonts.zip) 新建立的字型資料夾
* 字型模組中包含一些自定義字型。您可以將貴組織的自定義字型添加到C:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts module
* 開啟C:\CloudManager\aem-banking-application\pom.xml檔案
* 新增下列行  ```<module>fonts</module>``` 在pom.xml的「模組」區段中
* 儲存您的pom.xml
* 在IntelliJ中重新整理aem-banking-application專案

具有字型模組的專案結構
![字型模組](assets/fonts-module.png)

專案POM中包含的字型模組
![fonts-pom](assets/fonts-module-pom.png)
