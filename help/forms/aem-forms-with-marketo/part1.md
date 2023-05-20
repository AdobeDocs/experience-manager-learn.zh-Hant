---
title: AEM Forms與Marketo（上）
description: 使用AEM Forms表單資料模型將Marketo與AEM Forms整合的教程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# AEM Forms與Marketo

Marketo是Adobe的一部分，它提供以基於客戶的營銷為重點的營銷自動化軟體，包括電子郵件、移動、社交、數字廣告、Web管理和分析。

利用AEM Forms的表單資料模型，我們現在可以與AEMMarketo無縫整合。

[瞭解有關表單資料模型的詳細資訊](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo公開了一個REST API，它允許遠程執行系統的許多功能。 從建立程式到批量導入，有許多選項允許對Marketo實例進行細粒度控制。 使用表單資料模型將AEM Forms與Marketo整合起來非常簡單。

本教程將引導您完成使用表單資料模型將AEM Forms與Marketo整合所涉及的步驟。 完成本教程後，您將擁有一個OSGi捆綁包，該捆綁包將針對Marketo進行自定義身份驗證。 您還將使用提供的交換器檔案配置資料源。

要開始，強烈建議您熟悉「先決條件」部分中列出的以下主題。

## 必備條件

1. [安裝了AEMAEM FormsAdd的伺服器](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 地方發AEM展環境
1. 熟悉表單資料模型
1. Swagger檔案的基本知識
1. 建立自適應Forms

**客戶端密鑰ID和客戶端密鑰**

將Marketo與AEM Forms整合的第一步是獲取使用API進行REST調用所需的API憑據。 您需要以下

1. 客戶端ID
1. 客戶端密鑰
1. 標識端點
1. 驗證URL

[請按照Marketo官方檔案獲取上述物業。](https://developers.marketo.com/rest-api/) 或者，您也可以聯繫您的Marketo實例的管理員。

**開始之前**

[下載並解壓縮與本文相關的資產。](assets/aemformsandmarketo.zip) zip檔案包含以下內容：

1. BlankTemplatePackage.zip — 這是自適應表單模板。 使用包管理器導入此項。
1. marketo.json — 這是用於配置資料源的swagger檔案。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — 這是執行自定義身份驗證的捆綁包。 如果您無法完成本教程或您的捆綁包未按預期工作，則可以使用此功能。

## 後續步驟

[建立自定義身份驗證](./part2.md)
