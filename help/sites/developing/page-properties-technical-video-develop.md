---
title: 在AEM Sites中擴充頁面屬性
description: 瞭解如何在Adobe Experience Manager Sites中擴充頁面屬性的中繼資料欄位。 本影片詳細介紹使用Sling Resource Merger功能達成此目標的最有效方式。
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 1%

---

# 擴充頁面屬性 {#extending-page-properties-in-aem-sites}

自訂頁面屬性的中繼資料欄位是任何Sites實作中的常見要求。 本影片詳細介紹使用Sling Resource Merger功能達成此目標的最有效方式。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

以上影片說明自訂頁面的頁面屬性 [WKND參考網站](https://github.com/adobe/aem-guides-wknd).

## WKND頁面屬性封裝範例

您可以使用提供的 [WKND頁面屬性封裝範例](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 包含 **WKND** 和 **基本** 以上影片中顯示的索引標籤自訂。 此 **社群媒體** 索引標籤自訂未提供為 [WKND頁面元件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) 現在使用V3版本的WCM核心元件，而在V3版本中， [已棄用社交分享](https://github.com/adobe/aem-core-wcm-components/pull/1930).

不過，為方便學習，您可以使用將WKND頁面元件指向WCM核心元件V2版本 `sling:resourceSuperType` 屬性值並覆蓋 [社群媒體](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 標籤。 如需詳細資訊，請參閱 [設定頁面屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

此範例套件應安裝在本機AEM SDK或AEM 6.X.X執行個體上，以供學習之用。
