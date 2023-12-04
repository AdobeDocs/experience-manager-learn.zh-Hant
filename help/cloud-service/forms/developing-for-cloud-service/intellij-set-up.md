---
title: 安裝IntelliJ社群版
description: 安裝AEM專案並將其匯入IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
duration: 65
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---

# 安裝IntelliJ

安裝 [IntelliJ社群版](https://www.jetbrains.com/idea/download/#section=windows). 在安裝期間建議時，您可以接受預設設定。

## 匯入AEM專案

* 啟動IntelliJ
* 匯入您在上一步建立的AEM專案。 專案匯入後，您的畫面應該類似這樣 ![aem-banking-app](assets/aem-banking-app.png). 您通常會使用core、ui.apps、ui.config和ui.content子專案。
* 如果您沒有看到Maven和終端機視窗，請前往「檢視 — >工具視窗」並選擇「Maven和終端機」

## 新增字型模組

如果您想在PDF檔案中使用自訂字型，則需要將自訂字型推送到AEM Forms CS執行個體。 請遵循下列步驟

* 建立名為的資料夾 **字型** 在C:\CloudManager\aem-banking-application中
* 擷取內容 [font.zip](assets/fonts.zip) 到新建立的字型資料夾中
* 字型模組中包括一些自訂字型。您可以將組織的自訂字型新增至字型模組的C:\CloudManager\aem-banking-application\fonts\src\main\resources資料夾
* 開啟C:\CloudManager\aem-banking-application\pom.xml檔案
* 新增下列行  ```<module>fonts</module>``` 在pom.xml的模組區段中
* 儲存您的pom.xml
* 在IntelliJ中重新整理aem-banking-application專案

具有字型模組的專案結構
![fonts-module](assets/fonts-module.png)

專案POM中包含的字型模組
![fonts-pom](assets/fonts-module-pom.png)

## 後續步驟

[設定Git](./setup-git.md)
