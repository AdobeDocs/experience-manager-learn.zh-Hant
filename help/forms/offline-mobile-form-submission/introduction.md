---
title: 在HTM5表單提交上觸發AEM工作流程簡介
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 繼續以離線模式填寫行動表單，並提交行動表單以觸發AEM工作流程
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 下載部分完成的行動表單並提交至AEM工作流程

常見的使用案例是能夠將XDP轉譯為資料擷取活動的HTML。 當表單簡單且可線上填入和提交時，此功能就十分有效。 不過，如果表單很複雜，使用者可能無法線上完成表單，我們需要提供讓表單填入程式下載互動式表單版本的功能，以便使用Acrobat/Reader以離線方式填入。 填寫表單後，用戶可以聯機提交表單。
若要完成此使用案例，我們需要執行下列步驟：

* 能使用行動表單中輸入的資料產生互動式/可填寫PDF
* 從Acrobat/Reader處理PDF提交
* 觸發Adobe Experience Manager(AEM)工作流程以檢閱提交的PDF

本教學課程會逐步說明完成上述使用案例所需的步驟。 本教學課程的范常式式碼和資產如下 [可用。](part-four.md)

以下影片提供使用案例的概觀

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
