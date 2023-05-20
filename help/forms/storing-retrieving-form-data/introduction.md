---
title: 從MySQL資料庫儲存和檢索表單資料簡介
description: 多部分教程，引導您完成儲存和檢索表單資料所涉及的步驟
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# 從MySQL資料庫儲存和檢索自適應表單資料

本教程將引導您完成保存和從資料庫檢索自適應表單資料所涉及的步驟。 本教程使用MySQL資料庫儲存Adaptive Form資料。 只要在中部署了特定於資料庫的驅動程式，您選擇的資料庫就可以用於儲存數AEM據。 在高級別上，需要採取以下步驟來實現使用情形：

* 使用GuideBridge API訪問Adaptive Form資料

* 對servlet進行POST調用。 此Servlet將資料儲存在資料庫中。 儲存的資料與GUID關聯

* 當要用儲存的資料填充自適應表單時，請檢索與GUID關聯的資料，並使用 **request.setAttribute** 的雙曲餘切值。

## 演示使用案例

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


