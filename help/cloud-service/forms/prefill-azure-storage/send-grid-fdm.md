---
title: 使用SendGrid傳送電子郵件
description: 觸發包含已儲存表單連結的電子郵件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 1%

---

# 與SendGrid整合

AEM Forms資料整合可設定不同的資料來源，並將其與AEM Forms連線。 它提供直覺式使用者介面，可跨連線的資料來源建立商業實體和服務的統一資料呈現結構描述。

我們使用SendGrid API來傳送使用動態範本的電子郵件。 假設您熟悉SendGrid API，可使用動態範本傳送電子郵件。 在本教學課程中，已提供描述API的Swagger檔案。

## 建立整合

請依照下列步驟，建立AEM Forms與SendGrid之間的整合

* 建立RESTful資料來源，使用 [swagger檔案](./assets/SendGridWithDynamicTemplate.yaml). [請觀看此影片，瞭解詳細指示](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在AEM Forms中建立資料來源時
* 根據上一步驟建立的資料來源建立表單資料模型。[請依照詳細檔案操作](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) 建立表單資料模型時。

為本教學課程建立的表單資料模型會包含在文章資產中。

### 後續步驟

[建立Azure儲存體整合](./create-fdm.md)
