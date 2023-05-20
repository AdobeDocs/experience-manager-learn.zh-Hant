---
title: 設定ContextHub以使用AEM Sites個性化
description: ContextHub是用於儲存、操作和呈現上下文資料的框架。 ContextHub Javascript API使您能夠訪問儲存，以根據需要建立、更新和刪除資料。 因此，ContextHub表示頁面上的資料層。 本頁介紹如何將上下文中心添加AEM到網站頁。
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 2%

---

# 設定ContextHub以個性化 {#set-up-contexthub}

ContextHub是用於儲存、操作和呈現上下文資料的框架。 ContextHub Javascript API使您能夠訪問儲存，以根據需要建立、更新和刪除資料。 因此，ContextHub表示頁面上的資料層。 本頁介紹如何將上下文中心添加AEM到網站頁。

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>我們使用WKND參考站點進行此視頻，但它不是發行版的一AEM部分。 您可以下載 [此處是最新版本](https://github.com/adobe/aem-guides-wknd/releases)。

將ContextHub添加到您的頁面以啟用ContextHub功能並連結到ContextHub JavaScript庫。 ContextHub JavaScript API提供對ContextHub管理的上下文資料的訪問。

## 將ContextHub添加到頁面元件 {#adding-contexthub-to-a-page-component}

要啟用ContextHub功能並連結到ContextHub JavaScript庫，請包括 `contexthub` 元件 `<head>` 的子菜單。 頁面元件的HTL代碼與以下示例類似：

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## 站點配置和ContextHub段 {#site-configuration-and-contexthub-segments}

ContextHub包括分段引擎，用於管理段並確定為當前上下文解析哪些段。 定義了若干段。 可以使用Javascript API [確定已解析的段](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)。 在以下位置為您的站點啟用ContextHub段 [[!UICONTROL 配置瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)。

## 建立段 {#create-segments}

創AEM建作為續簽規則的段。 也就是說，它們定義了預告內容出現在網頁上的時間。 然後，內容可以根據訪問者的需要和興趣而特別針對它們匹配的段。

## 將雲配置、段路徑和ContextHub路徑分配給您的站點 {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

將雲配置路徑、分段路徑和ContextHub路徑分配給您的站點根節點，以便您可以為您的受眾建立個性化體驗。 使用ContextHub，可以處理上下文資料並test已解析的段。

![CRXDE Lite](assets/crx-de-properties.png)

您可以閱讀以下有關ContextHub和分段的詳細資訊：

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [將上下文中心添加到頁面和訪問儲存](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [了解區段](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [使用 ContextHub 設定分段](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
