---
title: 擴充AEM Sites中的頁面屬性
description: 了解如何擴充Adobe Experience Manager Sites中「頁面屬性」的中繼資料欄位。 此影片會詳細說明使用Sling Resource Merger功能的最有效方式。
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 0%

---

# 擴充頁面屬性 {#extending-page-properties-in-aem-sites}

自訂頁面屬性的中繼資料欄位是任何Sites實作中的常見需求。 此影片會詳細說明使用Sling Resource Merger功能的最有效方式。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

上述影片顯示自訂 [WKND參考站點](https://github.com/adobe/aem-guides-wknd).

## 範例WKND頁面屬性套件

您可以使用提供的 [WKND頁面屬性套件範例](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 包含 **WKND** 和 **基本** 標籤自訂。 此 **社交媒體** 標籤自訂未提供為 [WKND頁面元件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) 現在使用V3版的WCM核心元件，而V3版則使用 [社交分享已淘汰](https://github.com/adobe/aem-core-wcm-components/pull/1930).

不過，為了學習的目的，您可以使用 `sling:resourceSuperType` 屬性值並覆蓋 [社交媒體](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 標籤。 如需詳細資訊，請參閱 [設定頁面屬性](https://experienceleague.adobe.com/docs/experience-manager-64/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

本範例套件應安裝在本機AEM SDK或AEM 6.X.X執行個體上，以利學習之用。
