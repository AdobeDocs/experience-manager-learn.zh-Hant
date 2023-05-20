---
title: 從MySQL資料庫中儲存和檢索帶附件的表單資料
description: 多部分教程，引導您完成儲存和檢索帶有附件的表單資料所涉及的步驟
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---

# 基於2FA的自適應表單資料儲存與檢索

本教程將引導您完成使用2FA保存和檢索帶附件的自適應表單資料所涉及的步驟。 本教程使用MySQL資料庫儲存Adaptive Form資料。 只要在中部署了特定於資料庫的驅動程式，您選擇的資料庫就可以用於儲存數AEM據。 在高級別上，需要採取以下步驟來實現使用情形：

* 使用GuideBridge API訪問Adaptive Form資料

* 對servlet進行POST調用。 此Servlet將資料儲存在資料庫中，並將表單附件儲存在CRX儲存庫中。 資料庫中儲存的資料與GUID關聯。

* 當要用儲存的資料填充自適應表單時，請檢索與GUID關聯的資料，並使用 **request.setAttribute** 的雙曲餘切值。

## 演示使用案例

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 必備條件

預計此內容的受眾將在以下領域獲得一些經驗：

* 自適應窗體
* 表單資料模式
* OSGi服務/元件
* 客AEM戶端庫


## 後續步驟

[配置資料源](./configure-data-source.md)