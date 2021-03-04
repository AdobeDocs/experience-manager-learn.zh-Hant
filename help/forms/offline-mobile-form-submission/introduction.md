---
title: HTM5表AEM單提交的觸發工作流程
seo-title: HTML5表AEM單提交的觸發工作流程
description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
seo-description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發工AEM作流程
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---


# 下載部分完成的行動表單並提交至工作AEM流程

常見的使用案例是能夠將XDP轉換為HTML，以用於資料擷取活動。 當表單簡單且可線上填寫和提交時，就能正常運作。 但是，如果表單很複雜，使用者可能無法線上上完成表單，我們需要提供讓表單填寫程式下載互動式表單版本的能力，以便使用Acrobat/Reader離線填寫。 填妥表格後，使用者就可以上線提交表格。
要完成此使用案例，我們需要執行以下步驟：

* 能夠使用在行動表單中輸入的資料產生互動／可填寫的PDF
* 處理Acrobat/Reader的PDF提交
* 觸發Adobe Experience Manager(AEM)工作流程以審閱提交的PDF

本教學課程將逐步說明完成上述使用案例所需的步驟。 此處提供與本教學課程相關的范常式式碼和資產[。](part-four.md)

以下影片提供使用案例的概觀

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

