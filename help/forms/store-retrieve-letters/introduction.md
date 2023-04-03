---
title: 保存並恢復信函
seo-title: Save and resume letters
description: 了解如何儲存和擷取草稿信函
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

「互動式通訊」可讓代理準備臨機對應，以儲存部分完成的對應，並擷取相同對應，以繼續運作。 AEM Forms提供您 [服務提供商介面](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). 客戶應實作此介面以取得「儲存並繼續」功能。

本文使用MySQL資料庫來儲存字母實例的元資料。 信函資料儲存在檔案系統上。

下列影片示範使用案例：

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## 必要條件

您需要下列項目才能實作解決方案，以符合您的需求

* 使用AEM Forms的經驗
* AEM Server 6.5搭配Forms附加元件
* 在建立OSGI套件組合時應該很熟悉
