---
title: 建立主要的最適化表單
description: 建立最適化表單以擷取申請人資訊，並建立最適化表單以擷取儲存的最適化表單
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 1%

---


# 建立主要的最適化表單

形式&#x200B;**StoreAFWithAttachments**&#x200B;是主要的自適應形式。 此最適化表單是使用案例的入口點。 在此表單中，會擷取使用者詳細資訊，包括行動電話號碼。 此表單還可新增一些附件。 當按一下「儲存並退出」按鈕時，會執行伺服器端程式碼，將表單資料儲存在資料庫中，並產生唯一的應用程式ID，並呈現給使用者以進行安全保留。 此應用程式ID將用來擷取與應用程式相關聯的行動電話號碼。

![主要應用表單](assets/6552.JPG)

此表單與在課程中先前建立的&#x200B;**bootboxjs540,storeAFWithAttachments**&#x200B;客戶端庫以及在表單提交時觸AEM發的工作流相關聯。


* 範例表單以[自訂最適化表單範本](assets/custom-template-with-page-component.zip)為基礎，需要匯入至範例表AEM單才能正確呈現。

* 您可以下載完成的[StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip)並將它匯入您的AEM實例。

* 與此表單AEM](assets/workflow-model-store-af-with-attachments.zip)關聯的[工作流程必須匯入您的例AEM項，表單才能運作。



