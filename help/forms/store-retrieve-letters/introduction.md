---
title: 保存並恢復信函
seo-title: Save and resume letters
description: 瞭解如何保存和檢索草稿字母
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# 簡介

互動式通信允許代理準備臨時對應以保存部分完成的對應並檢索該對應以繼續工作。 AEM Forms給你 [服務提供商介面](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html)。 預計客戶將實施此介面以獲得「保存和恢復」功能。

本文使用MySQL資料庫儲存字母實例的元資料。 字母資料儲存在檔案系統上。

以下視頻演示了使用案例：

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## 預設站點

您將需要以下工具來實施解決方案以滿足您的需求

* 在AEM Forms的工作經驗
* 帶AEMForms插件的Server 6.5
* 在構建OSGI捆綁包時應該熟悉
