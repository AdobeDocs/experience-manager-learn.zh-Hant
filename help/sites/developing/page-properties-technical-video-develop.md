---
title: 在AEM Sites擴展頁面屬性
description: 瞭解如何擴展Adobe Experience Manager Sites的頁面屬性的元資料欄位。 這段視頻詳細描述了使用Sling Resource Compurate的功能實現這一目標的最有效方法。
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 0%

---

# 擴展頁面屬性 {#extending-page-properties-in-aem-sites}

在任何Sites實現中，自定義頁屬性的元資料欄位都是常見要求。 這段視頻詳細描述了使用Sling Resource Compurate的功能實現這一目標的最有效方法。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

上面的視頻顯示了自定義 [WKND參考站點](https://github.com/adobe/aem-guides-wknd)。

## 示例WKND頁屬性包

您可以使用提供的 [示例WKND頁屬性包](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 含 **WKND** 和 **基本** 頁籤自定義設定。 的 **社交媒體** 頁籤自定義未提供為 [WKND頁元件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) 現在使用V3版的WCM核心元件，在V3版中使用 [不建議使用社交共用](https://github.com/adobe/aem-core-wcm-components/pull/1930)。

但是，為了學習目的，您可以使用 `sling:resourceSuperType` 屬性值並覆蓋 [社交媒體](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 頁籤。 有關詳細資訊，請參見 [配置頁面屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

本示例軟體包應安裝在本AEM地SDK或AEM6.X.X實例上，以便學習。
