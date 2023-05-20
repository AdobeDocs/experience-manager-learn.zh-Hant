---
title: 正在安裝IntelliJ社區版
description: 將項目安裝AEM並導入IntelliJ
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

# 正在安裝IntelliJ

安裝 [IntelliJ社區版](https://www.jetbrains.com/idea/download/#section=windows)。 在安裝期間建議時，您可以接受預設設定。

## 導入項AEM目

* 啟動IntelliJ
* 導入在AEM前一步中建立的項目。 在導入項目後，螢幕應該是這樣的 ![銀行應用](assets/aem-banking-app.png)。 通常，您將使用core、ui.apps、ui.config和ui.content子項目。
* 如果未看到主窗口和終端窗口，請轉到「查看」 — >「工具」窗口，然後選擇「主窗口和終端」

## 添加字型模組

如果想在PDF檔案中使用自定義字型，則需要將自定義字型推送到AEM FormsCS實例。 請執行以下步驟

* 建立名為 **字型** C:\CloudManager\aem-banking-application
* 提取 [字型.zip](assets/fonts.zip) 到新建立的字型資料夾
* 字型模組中包括一些自定義字型。您可以將組織的自定義字型添加到C:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts module
* 開啟C:\CloudManager\aem-banking-application\pom.xml檔案
* 添加以下行  ```<module>fonts</module>``` 在pom.xml的模組部分
* 保存pom.xml
* 在IntelliJ中刷新銀行應用程式項目

具有字型模組的項目結構
![字型模組](assets/fonts-module.png)

項目POM中包含的字型模組
![字型pom](assets/fonts-module-pom.png)
