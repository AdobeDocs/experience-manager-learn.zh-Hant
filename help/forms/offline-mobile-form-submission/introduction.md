---
title: 在HTM5表單提交上觸發AEM工作流程
seo-title: 在HTML5表單提交上觸發AEM工作流程
description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
feature: 行動表單
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開發
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 1%

---


# 下載部分完成的行動表單並提交至AEM工作流程

常見的使用案例是能夠將XDP轉譯為資料擷取活動的HTML。 當表單簡單且可線上填入和提交時，此功能就十分有效。 不過，如果表單很複雜，使用者可能無法線上完成表單，我們需要提供讓表單填入程式下載互動式表單版本的功能，以便使用Acrobat/Reader以離線方式填入。 填寫表單後，用戶可以聯機提交表單。
若要完成此使用案例，我們需要執行下列步驟：

* 可透過在行動表單中輸入的資料產生互動式/可填寫的PDF
* 從Acrobat/Reader處理PDF提交
* 觸發Adobe Experience Manager(AEM)工作流程以檢閱提交的PDF

本教學課程會逐步說明完成上述使用案例所需的步驟。 此處提供與本教學課程相關的范常式式碼和資產。](part-four.md)[

以下影片提供使用案例的概觀

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

