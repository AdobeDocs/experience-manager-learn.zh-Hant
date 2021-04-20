---
title: 使用AEM Sites設定個人化的ContextHub
description: ContextHub是儲存、控制和呈現上下文資料的架構。 ContextHub Javascript API可讓您存取商店，以視需要建立、更新和刪除資料。 因此，ContextHub代表您頁面上的資料層。 本頁說明如何將內容中樞新增至網AEM站頁面。
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 1%

---


# 設定個人化的ContextHub {#set-up-contexthub}

ContextHub是儲存、控制和呈現上下文資料的架構。 ContextHub Javascript API可讓您存取商店，以視需要建立、更新和刪除資料。 因此，ContextHub代表您頁面上的資料層。 本頁說明如何將內容中樞新增至網AEM站頁面。

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>本視訊使用WKND參考網站，但它不是版本的一部AEM分。 您可以在此處下載[最新版本](https://github.com/adobe/aem-guides-wknd/releases)。

將ContextHub新增至您的頁面，以啟用ContextHub功能並連結至ContextHub JavaScript程式庫。 ContextHub JavaScript API可讓您存取ContextHub管理的上下文資料。

## 將ContextHub新增至頁面元件{#adding-contexthub-to-a-page-component}

若要啟用ContextHub功能並連結至ContextHub JavaScript程式庫，請在網頁的`<head>`區段中包含`contexthub`元件。 頁面元件的HTL程式碼類似下列範例：

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## 網站設定和ContextHub區段{#site-configuration-and-contexthub-segments}

ContextHub包含區段引擎，可管理區段並判斷哪些區段可針對目前的上下文加以解析。 已定義數個區段。 您可以使用Javascript API來判斷已解析的區段](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)。 [在[[!UICONTROL Configuration Browser]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)下啟用您網站的ContextHub區段。

## 建立區段{#create-segments}

建AEM立可當成茶具規則的區段。 也就是說，它們會定義摘要內容出現在網頁上的時間。 然後，視訪客所符合的區段而定，內容可特別針對訪客的需求和興趣。

## 將雲端設定、區段路徑和ContextHub路徑指派至您的網站{#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

將雲端設定路徑、區段路徑和ContextHub路徑指派至您的網站根節點，以便您為受眾建立個人化體驗。 使用ContextHub，您可以控制上下文資料並測試已解析的區段。

![CRXDE Lite](assets/crx-de-properties.png)

您可以閱讀下列ContextHub和區段的更多資訊：

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [將上下文中心添加到頁面和訪問儲存](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [了解區段](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [使用ContextHub設定區段](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
