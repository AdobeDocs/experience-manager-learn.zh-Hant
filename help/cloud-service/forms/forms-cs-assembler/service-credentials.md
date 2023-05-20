---
title: 服AEM務憑據
description: 從Developer Console下載服AEM務憑據。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# 服務憑據

與AEMas a Cloud Service的整合必須能夠安全地驗證到AEM。 AEM Developer Console生成服務憑據，外部應用程式、系統和服務使用這些憑據以寫程式方式與AEM作者或通過HTTP發佈服務交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

下載的服務憑據檔案儲存為在提供的eclipse中名為service_token.json的資源檔案。 service_token檔案中的值用於生成JWT並將JWT交換為訪問令牌。 實用程式類GetServiceCredentials用於從service_token.json資源檔案中提取屬性值。
