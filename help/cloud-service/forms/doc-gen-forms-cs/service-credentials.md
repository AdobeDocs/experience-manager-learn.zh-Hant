---
title: AEM Forms服務認證
description: 從AEM的Developer Console下載服務認證。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# AEM Forms服務認證

與AEM as a Cloud Service的整合必須能夠安全地向AEM進行驗證。 AEM的Developer Console會產生服務憑證，外部應用程式、系統和服務會使用這些憑證，以程式設計方式透過HTTP與AEM作者或發佈服務互動。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

下載的服務認證檔案會儲存為所提供eclipse中名為service_token.json的資源檔案。 service_token檔案中的值用於產生JWT並將JWT交換為存取權杖。 公用程式類別GetServiceCredentials是用來從service_token.json資源檔案擷取屬性值。
