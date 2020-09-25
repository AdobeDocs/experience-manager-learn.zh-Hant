---
title: 在HTM5表單提交上觸發AEM工作流程
seo-title: 在HTML5表單提交上觸發AEM工作流程
description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: 在離線模式下繼續填寫行動表單，並提交行動表單以觸發AEM工作流程
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---


# 下載部分完成的行動表單並送出至AEM工作流程

常見的使用案例是能夠將XDP轉換為HTML，以用於資料擷取活動。 當表單簡單且可線上填寫和提交時，就能正常運作。 但是，如果表單很複雜，使用者可能無法線上填寫表單，我們需要提供讓填表者下載互動式表單版本的功能，以便使用Acrobat/Reader離線填寫。 填妥表格後，使用者就可以上線提交表格。
要完成此使用案例，我們需要執行以下步驟：

* 能夠使用在行動表單中輸入的資料產生互動／可填寫的PDF
* 處理從Acrobat/Reader提交的PDF
* 觸發Adobe Experience Manager(AEM)工作流程以審核提交的PDF

本教學課程將逐步說明完成上述使用案例所需的步驟。 您可在這裡取得與本教學課程相關的范常式 [式碼和資產。](part-four.md)

以下影片提供使用案例的概觀

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

