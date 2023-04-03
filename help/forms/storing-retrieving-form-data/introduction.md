---
title: 從MySQL資料庫儲存和檢索表單資料簡介
description: 多部分教學課程，逐步引導您完成儲存和擷取表單資料的相關步驟
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# 從MySQL資料庫儲存和檢索最適化表單資料

本教學課程將逐步引導您完成從資料庫儲存和擷取適用性表單資料的相關步驟。 本教學課程使用MySQL資料庫來儲存適用性表單資料。 只要您已在AEM中部署了資料庫特定驅動程式，您所選擇的資料庫就可用於儲存資料。 在高層面上，需要執行下列步驟來達成使用案例：

* 使用GuideBridge API存取最適化表單資料

* 對Servlet進行POST呼叫。 此Servlet將資料儲存在資料庫中。 儲存的資料與GUID相關聯

* 若想使用儲存的資料填入適用性表單，請擷取與GUID相關的資料，並使用 **request.setAttribute** 方法。

## 使用案例的展示

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)
