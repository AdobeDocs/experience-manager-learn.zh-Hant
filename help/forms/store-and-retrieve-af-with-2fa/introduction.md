---
title: 從MySQL資料庫儲存和檢索帶有附件的表單資料
description: 多部分教學課程，可引導您逐步瞭解如何儲存和擷取含附件的表單資料
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 1%

---


# 基於2FA的自適應表單資料儲存與檢索

本教程將引導您完成使用2FA保存和檢索帶有附件的最適化表單資料的步驟。 本教程使用MySQL資料庫儲存Adaptive Form資料。 只要您已在AEM中部署資料庫特定的驅動程式，您所選擇的資料庫就可以用來儲存資料。 在高層級上，需要執行下列步驟才能實現使用案例：

* 使用GuideBridge API存取Adaptive Form資料

* 對servlet進行POST調用。 此servlet將資料儲存在資料庫中，並將表單附件儲存在CRX儲存庫中。 資料庫中儲存的資料與GUID相關聯。

* 當您想用儲存的資料填入最適化表單時，請擷取與GUID關聯的資料，並使用&#x200B;**request.setAttribute**&#x200B;方法填入最適化表單。

## 使用案例的展示

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## 必備條件

本內容的讀者應具備下列方面的經驗：

* 最適化表單
* 表單資料模型
* OSGi服務／元件
* AEM用戶端程式庫
