---
title: 從MySQL資料庫儲存和檢索表單資料
description: 多部分教學課程，引導您逐步瞭解儲存和擷取表單資料的相關步驟
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 2%

---


# 從MySQL資料庫儲存和檢索自適應表單資料

本教程將引導您完成從資料庫中保存和檢索最適化表單資料的步驟。 本教程使用MySQL資料庫儲存Adaptive Form資料。 只要您已在中部署了資料庫特定驅動程式，就可以使用您選擇的資料庫來儲存數AEM據。 在高層級上，需要執行下列步驟才能實現使用案例：

* 使用GuideBridge API存取Adaptive Form資料

* 對servlet進行POST調用。 此servlet將資料儲存在資料庫中。 儲存的資料與GUID相關聯

* 當您想用儲存的資料填入最適化表單時，請擷取與GUID關聯的資料，並使用&#x200B;**request.setAttribute**&#x200B;方法填入最適化表單。

## 使用案例的展示

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
