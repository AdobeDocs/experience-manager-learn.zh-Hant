---
title: 儲存並繼續字母
description: 瞭解如何儲存和擷取草稿信件
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 160
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# 簡介

互動式通訊可讓準備臨機通訊的代理程式儲存部分完成的通訊，並擷取相同的通訊以繼續工作。 AEM Forms提供[服務提供者介面](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html)。 客戶應實作此介面，以取得「儲存並繼續」功能。

本文使用MySQL資料庫來儲存信件執行個體的中繼資料。 信件資料儲存在檔案系統中。

以下影片示範此使用案例：

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## 先決條件

您將需要下列專案來實作解決方案，以符合您的需求

* AEM Forms的使用體驗
* AEM Server 6.5含Forms附加元件
* 應該熟悉如何建立OSGI套件組合
