---
title: 使用工作流程步驟將資料提交至SharePoint清單
description: 使用叫用FDM工作流程步驟將資料插入到SharePoint清單中
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-15126
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# 使用叫用FDM工作流程步驟將資料插入SharePoint清單


本文說明在AEM工作流程中使用叫用FDM步驟將資料插入SharePoint清單所需的步驟。

本文假設您擁有 [已成功設定最適化表單以提交資料至SharePoint清單。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)


## 根據SharePoint清單資料來源建立表單資料模型

* 根據SharePoint清單資料來源建立新的表單資料模型。
* 新增適當的模型和表單資料模型的get服務。
* 設定插入服務以插入頂層模型物件。
* 測試插入服務。


## 建立工作流程

* 使用叫用FDM步驟建立簡單的工作流程。
* 設定呼叫FDM步驟，使用先前步驟建立的表單資料模型。
* ![associate-fdm](assets/fdm-insert-1.png)

* ![對映輸入引數](assets/fdm-insert-2.png)
* 請留意是否使用JSON點標籤法。 提交的資料採用以下格式，我們正在從提交的資料中擷取ContactUS物件。

```json
{
  "ContactUS": {
    "Title": "Mr",
    "Products": "Photoshop",
    "HighNetWorth": "1",
    "SubmitterName": "John Does"
  }
}
```



## 設定自適應表單以觸發AEM工作流程

* 使用先前步驟建立的表單資料模型建立最適化表單。
* 將資料來源中的一些欄位拖放至您的表單上。
* 設定表單的提交動作，如下所示
* ![submit-action](assets/configure-af.png)



## 測試表單

預覽在上一步建立的表單。 填寫表單並提交。 表單中的資料應插入SharePoint清單中。

