---
title: 在AEM Sites中擴充頁面屬性
description: 瞭解如何在Adobe Experience Manager Sites中擴充頁面屬性的中繼資料欄位。 本影片詳細介紹使用Sling Resource Merger功能達成此目標的最有效方式。
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Experience Manager as a Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 1%

---

# 擴充頁面屬性 {#extending-page-properties-in-aem-sites}

自訂頁面屬性的中繼資料欄位是任何Sites實作中的常見要求。 本影片詳細介紹使用Sling Resource Merger功能達成此目標的最有效方式。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

上述影片顯示為[WKND參考網站](https://github.com/adobe/aem-guides-wknd)自訂頁面屬性。

## WKND頁面屬性封裝範例

您可以使用提供的[範例WKND頁面屬性封裝](./assets/WKND-PageProperties-Example-Dialog-1.0.zip)，其中包含&#x200B;**WKND**&#x200B;和&#x200B;**Basic**&#x200B;標籤自訂（如上述影片所示）。 未提供&#x200B;**SocialMedia**&#x200B;標籤自訂，因為[WKND頁面元件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5)現在使用V3版本的WCM核心元件，而在V3版本中，[社交共用已過時](https://github.com/adobe/aem-core-wcm-components/pull/1930)。

不過，為方便學習，您可以使用`sling:resourceSuperType`屬性值將WKND頁面元件指向WCM核心元件的V2版本，並覆蓋[社群媒體](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95)索引標籤。 如需詳細資訊，請參閱[設定頁面屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=zh-Hant#configuring-your-page-properties)

本範例套件應安裝在本機AEM SDK或AEM 6.X.X執行個體上，以供學習之用。
