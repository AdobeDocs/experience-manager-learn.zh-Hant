---
title: 建立主要的最適化表單
description: 建立最適化表單來擷取申請人資訊，並建立最適化表單來擷取已儲存的最適化表單
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
duration: 41
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# 建立主要的最適化表單

表單&#x200B;**StoreAFWithAttachments**&#x200B;是主要的最適化表單。 此最適化表單是使用案例的進入點。 在此表單中，會擷取使用者詳細資訊，包括行動電話號碼。 此表單也可新增一些附件。 按一下「儲存並退出」按鈕時，系統會執行伺服器端程式碼，將表單資料儲存在資料庫中，並產生唯一的應用程式ID，然後提供給使用者安全儲存。 此應用程式ID可用來擷取與應用程式相關聯的行動電話號碼。

![主應用程式表單](assets/6552.JPG)

此表單與先前在課程中建立的&#x200B;**bootboxjs540、storeAFWithAttachments**&#x200B;使用者端程式庫相關聯，且在表單提交時觸發的AEM工作流程相關聯。


* 範例表單是以[自訂最適化表單範本](assets/custom-template-with-page-component.zip)為基礎，必須匯入AEM才能正確呈現範例表單。

* 您可以下載已完成的[StoreAfWithAttachments表單](assets/store-af-with-attachments-form.zip)，並將其匯入您的AEM執行個體。

* 與此表單](assets/workflow-model-store-af-with-attachments.zip)相關聯的[AEM工作流程需要匯入您的AEM執行個體，表單才能運作。


## 後續步驟

[建立表單擷取儲存的表單](./retrieve-saved-form.md)