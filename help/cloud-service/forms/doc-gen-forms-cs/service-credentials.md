---
title: AEM Forms服務憑證
description: 從AEM Developer Console下載服務憑證。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# AEM Forms服務憑證

與AEMas a Cloud Service的整合必須能夠安全地驗證AEM。 AEM Developer Console會產生服務憑證，這些憑證供外部應用程式、系統和服務使用，以程式設計方式與AEM製作或透過HTTP發佈服務互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

下載的服務憑證檔案會儲存為提供之eclipse中名為service_token.json的資源檔案。 service_token檔案中的值可用來產生JWT，以及交換JWT以取得存取Token。 公用程式類GetServiceCredentials用於從service_token.json資源檔案中擷取屬性值。
