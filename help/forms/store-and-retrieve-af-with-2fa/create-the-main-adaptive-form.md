---
title: 建立主要最適化表單
description: 建立最適化表單以擷取申請人資訊，並建立最適化表單以擷取儲存的最適化表單
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: 開發
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 1%

---


# 建立主要最適化表單

表單&#x200B;**StoreAFWithAttachments**&#x200B;是主要的最適化表單。 此最適化表單是使用案例的入口點。 在此表單中，會擷取使用者詳細資訊，包括行動電話號碼。 此表單也能新增一些附件。 按一下「儲存並退出」按鈕時，執行伺服器端程式碼，將表單資料儲存在資料庫中，並產生唯一的應用程式ID，並呈現給使用者以供安全保管。 此應用程式ID將用於擷取與應用程式相關聯的行動電話號碼。

![主要申請表](assets/6552.JPG)

此表單與課程前建立的&#x200B;**bootboxjs540,storeAFWithAttachments**&#x200B;客戶端庫以及在表單提交時觸發的AEM工作流相關聯。


* 範例表單以[自訂最適化表單範本](assets/custom-template-with-page-component.zip)為基礎，需要匯入至AEM，範例表單才能正確轉譯。

* 已完成的[StoreAfWithAttachments表單](assets/store-af-with-attachments-form.zip)可下載並匯入至您的AEM執行個體。

* 需要將與此表單](assets/workflow-model-store-af-with-attachments.zip)相關聯的[AEM工作流程匯入至您的AEM例項，表單才能運作。



