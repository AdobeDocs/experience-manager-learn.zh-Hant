---
title: 部署範例資產
description: 將範例資產部署在本機雲端就緒的系統上。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---

# 部署範例資產

若要讓此使用案例在您的系統上正常運作，請在您的本機雲端就緒系統上部署下列資產。

* 請確定您已建立[簡介檔案](./introduction.md)

* [安裝自訂最適化表單範本和相關聯的頁面元件](./assets/azure-portal-template-page-component.zip)

* [安裝SendGrid表單資料模型](./assets/send-grid-form-data-model.zip). 您必須提供API金鑰和已驗證來自地址的SendGrid，此表單資料模型才能運作。 在表單資料模型編輯器中測試表單資料模型

* [安裝Azure支援的表單資料模型](./assets/azure-storage-fdm.zip). 您必須提供Azure儲存體帳戶認證，表單資料模型才能運作。 在表單資料模型編輯器中測試表單資料模型。

* [匯入範例最適化表單](./assets/credit-applications-af.zip)
* [匯入使用者端資源庫](./assets/client-lib.zip)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). 輸入有效的電子郵件，然後按一下儲存按鈕。 表單資料應儲存在Azure儲存體中，且會傳送一封包含已儲存表單連結的電子郵件至指定的電子郵件地址。
