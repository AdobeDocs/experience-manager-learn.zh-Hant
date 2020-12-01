---
title: 建立主要的最適化表單
description: 建立最適化表單以擷取申請人資訊，並建立最適化表單以擷取儲存的最適化表單
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# 建立主要的最適化表單

形式&#x200B;**StoreAFWithAttachments**&#x200B;是主要的自適應形式。 此最適化表單是使用案例的入口點。 在此表單中，會擷取使用者詳細資訊，包括行動電話號碼。 此表單還可新增一些附件。 當按一下「儲存並退出」按鈕時，會執行伺服器端程式碼，將表單資料儲存在資料庫中，並產生唯一的應用程式ID，並呈現給使用者以進行安全保留。 此應用程式ID將用來擷取與應用程式相關聯的行動電話號碼。

![主要應用表單](assets/6552.JPG)

此表單與在課程中先前建立的&#x200B;**bootboxjs540,storeAFWithAttachments**&#x200B;用戶端程式庫以及在表單提交時觸發的AEM工作流程相關聯。


* 範例表單以[自訂最適化表單範本](assets/custom-template-with-page-component.zip)為基礎，需要匯入至AEM，讓範例表單正確呈現。

* 完成的[StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip)可下載並匯入您的AEM例項。

* 與此表單](assets/workflow-model-store-af-with-attachments.zip)關聯的[AEM工作流程需要匯入至您的AEM例項，表單才能運作。



