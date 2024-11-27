---
title: 建置、部署和測試國家/地區元件
description: 建置、部署和測試
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 建置、部署和測試國家/地區元件

若要建置所有模組並將`all`套件部署到AEM的本機執行個體，請在專案根目錄中執行下列命令：

```mvn clean install -PautoInstallSinglePackage```

## 測試元件

若要將國家/地區元件整合至AEM Forms Cloud Ready例項並進行設定，請遵循下列步驟：

* 解壓縮[國家/地區](assets/countries.zip) zip檔案的內容。 每個檔案都應包含特定大陸的資料。
* 在content/dam/corecomponent.This下上傳json檔案是程式碼尋找json檔案的位置。如果您想將JSON檔案儲存在其他位置，您必須更新CountriesDropDownImpl類別中的Java程式碼。 具體來說，請更新init()方法中載入JSON檔案的路徑。例如，如果您想將JSON檔案儲存在content/dam/mydata/中，請更新如下的路徑

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* 登入您的AEM Forms Cloud Ready例項
* 建立最適化表單並將國家元件拖放到表單上
* 使用對話方塊編輯器設定國家/地區元件，並設定包括大陸在內的各種屬性
  ![內容](assets/select-continent.png)
* 預覽表單並確保國家/地區下拉式清單按預期運作

