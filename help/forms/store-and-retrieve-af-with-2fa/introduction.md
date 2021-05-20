---
title: 從MySQL資料庫儲存和檢索含附件的表單資料
description: 多部分教學課程，逐步引導您完成儲存和擷取含附件的表單資料的步驟
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 3%

---


# 使用2FA儲存和擷取最適化表單資料

本教學課程將逐步引導您完成使用2FA儲存及擷取附件適用性表單資料的相關步驟。 本教學課程使用MySQL資料庫來儲存適用性表單資料。 只要您已在AEM中部署了資料庫特定驅動程式，您所選擇的資料庫就可用於儲存資料。 在高層面上，需要執行下列步驟來達成使用案例：

* 使用GuideBridge API存取最適化表單資料

* 對Servlet進行POST呼叫。 此Servlet將資料儲存在資料庫中，並將表單附件儲存在CRX儲存庫中。 資料庫中儲存的資料與GUID相關聯。

* 如果想用儲存的資料填入適用性表單，請擷取與GUID相關的資料，並使用&#x200B;**request.setAttribute**&#x200B;方法填入適用性表單。

## 使用案例的展示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## 必備條件

此內容的對象應在下列方面擁有一些體驗：

* 適用性表單
* 表單資料模型
* OSGi服務/元件
* AEM用戶端程式庫
