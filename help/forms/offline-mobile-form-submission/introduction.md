---
title: HTM5表AEM單提交的觸發工作流簡介
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 以離線模式繼續填寫移動表單並提交移動表單以觸發工AEM作流
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
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 下載部分完成的移動表單並提交到工AEM作流

通用使用情形是能夠將XDP呈現為資料捕獲活動的HTML。 當表單簡單且可以線上填寫和提交時，此操作非常有效。 但是，如果表單複雜，用戶可能無法線上完成表單，則需要提供讓表單填寫者下載交互版本表單，以Acrobat/Reader離線方式填充。 填寫完表單後，用戶可以聯機提交表單。
要完成此使用情形，我們需要執行以下步驟：

* 能夠與以移動形式輸入的資料生成互動式/可填充PDF
* 處理Acrobat/Reader提交的PDF
* 觸發Adobe Experience Manager(AEM)工作流以審查提交的PDF

本教程將介紹完成上述使用情形所需的步驟。 與本教程相關的示例代碼和資產包括 [的子菜單。](part-four.md)

以下視頻將概述使用案例

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
