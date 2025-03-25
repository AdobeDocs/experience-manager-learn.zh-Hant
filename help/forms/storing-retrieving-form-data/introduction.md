---
title: 儲存和擷取MySQL資料庫的表單資料簡介
description: 多部分教學課程，逐步引導您完成儲存和擷取表單資料的相關步驟
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 236
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# 從MySQL資料庫儲存及擷取最適化表單資料

本教學課程將逐步引導您完成儲存和擷取資料庫中的調適型表單資料的相關步驟。 本教學課程使用MySQL資料庫來儲存最適化表單資料。 只要您已在AEM中部署資料庫特定驅動程式，即可使用您選擇的資料庫來儲存資料。 概略來說，完成使用案例需要下列步驟：

* 使用GuideBridge API存取最適化表單資料

* 對servlet進行POST呼叫。 此servlet會將資料儲存在資料庫中。 儲存的資料與GUID相關聯

* 當您想要將儲存的資料填入最適化表單時，請擷取與GUID相關的資料，並使用&#x200B;**request.setAttribute**&#x200B;方法填入最適化表單。

## 使用案例示範

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


