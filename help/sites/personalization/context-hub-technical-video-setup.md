---
title: 使用AEM Sites設定ContextHub以進行個人化
description: ContextHub是一種用於儲存、操控和呈現內容資料的架構。 ContextHub Javascript API可讓您視需要存取存放區以建立、更新和刪除資料。 因此，ContextHub代表頁面上的資料層。 本頁面說明如何將Context Hub新增至AEM網站頁面。
feature: Context Hub
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 375
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 2%

---

# 設定ContextHub以進行個人化 {#set-up-contexthub}

ContextHub是一種用於儲存、操控和呈現內容資料的架構。 ContextHub Javascript API可讓您視需要存取存放區以建立、更新和刪除資料。 因此，ContextHub代表頁面上的資料層。 本頁面說明如何將Context Hub新增至AEM網站頁面。

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>我們在此影片中使用WKND參考網站，它不是AEM版本的一部分。 您可以下載 [最新版本在此](https://github.com/adobe/aem-guides-wknd/releases).

將ContextHub新增至您的頁面，以啟用ContextHub功能並連結至ContextHub JavaScript程式庫。 ContextHub JavaScript API提供對ContextHub管理之內容資料的存取權。

## 將ContextHub新增至頁面元件 {#adding-contexthub-to-a-page-component}

若要啟用ContextHub功能並連結至ContextHub JavaScript程式庫，請包含 `contexthub` 中的元件 `<head>` 區段。 頁面元件的HTL程式碼類似於以下範例：

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## 網站設定和ContextHub區段 {#site-configuration-and-contexthub-segments}

ContextHub包含區段引擎，可管理區段並決定針對目前內容解析哪些區段。 已定義數個區段。 您可以使用Javascript API來 [決定已解析的區段](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). 在下啟用您網站的ContextHub區段 [[!UICONTROL 設定瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html).

## 建立區段 {#create-segments}

建立做為Teaser規則的AEM區段。 也就是說，這類規則可定義Teaser中的內容何時出現在網頁上。 內容就可以特別鎖定至訪客的需求和興趣，實際取決於他們相符的區段。

## 指派雲端設定、區段路徑和ContextHub路徑至您的網站 {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

指派雲端設定路徑、分段路徑和ContextHub路徑至您的網站根節點，好讓您可以為對象建立個人化體驗。 使用ContextHub，您可以操作內容資料並測試已解析的區段。

![CRXDE Lite](assets/crx-de-properties.png)

您可以閱讀以下有關ContextHub和區段的更多資訊：

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [將Context Hub新增至頁面並存取存放區](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [了解區段](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [使用 ContextHub 設定分段](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
