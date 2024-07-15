---
title: 在HTM5表單提交時觸發AEM工作流程
description: 以離線模式繼續填寫行動表單並提交行動表單以觸發AEM工作流程
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
duration: 342
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 下載部分完成的行動表單並提交至AEM工作流程

一個常見的使用案例是能夠將XDP轉譯為資料擷取活動的HTML。 當表單簡單且可以線上上填寫和提交時，此功能會很好用。 但是，如果表單很複雜，使用者可能無法線上上完成表單，我們需要提供讓表單填寫者下載互動式表單版本的功能，以便使用Acrobat/Reader以離線方式填寫。 填寫表單後，使用者可以線上上提交表單。
若要完成此使用案例，我們需要執行下列步驟：

* 能夠使用在行動表單中輸入的資料產生互動式/可填寫的PDF
* 處理來自Acrobat/Reader的PDF提交
* 觸發Adobe Experience Manager (AEM)工作流程以檢閱提交的PDF

本教學課程將逐步解說完成上述使用案例所需的步驟。 此教學課程的相關範常式式碼和資產[可在此取得。](part-four.md)

以下影片提供使用案例的概觀

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
