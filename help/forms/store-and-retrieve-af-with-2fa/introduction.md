---
title: 從MySQL資料庫儲存及擷取含有附件的表單資料
description: 多部分教學課程，逐步引導您完成儲存和擷取含有附件的表單資料的步驟
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 148
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# 使用2FA儲存及擷取最適化表單資料

本教學課程將逐步引導您完成使用2FA儲存及擷取含有附件的最適化表單資料的步驟。 本教學課程使用MySQL資料庫來儲存最適化表單資料。 只要您已在AEM中部署資料庫特定驅動程式，就可以使用您選擇的資料庫來儲存資料。 概略來說，完成使用案例需要下列步驟：

* 使用GuideBridge API存取最適化表單資料

* 對servlet進行POST呼叫。 此servlet會將資料儲存在資料庫中，並將表單附件儲存在CRX存放庫中。 資料庫中儲存的資料與GUID相關聯。

* 當您想要將儲存的資料填入最適化表單時，請擷取與GUID相關的資料，並使用&#x200B;**request.setAttribute**&#x200B;方法填入最適化表單。

## 使用案例示範

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 先決條件

此內容的對象預計會在以下領域有一些體驗：

* 最適化表單
* 表單資料模型
* OSGi服務/元件
* AEM使用者端資料庫


## 後續步驟

[設定資料Source](./configure-data-source.md)