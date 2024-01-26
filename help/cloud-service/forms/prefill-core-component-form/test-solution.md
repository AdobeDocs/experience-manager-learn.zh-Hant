---
title: 根據最適化表單預先填寫核心元件
description: 瞭解如何使用資料預先填寫最適化表單
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
duration: 28
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# 測試解決方案

部署程式碼後，根據核心元件建立最適化表單。 將最適化表單與預填服務建立關聯，如底下熒幕擷圖所示。
![預填服務](assets/pre-fill-service.png)

每次呈現表單時，都會執行相關的預填服務，且表單會填入預填服務傳回的資料。

例如，使用與guid相關聯的資料預先填寫表單 **d815a2b3-5f4c-4422-8197-d0b73479bf0e**，則會使用下列url。
預填服務中的程式碼將會擷取guid引數的值，並從資料來源擷取與guid相關聯的資料。

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
