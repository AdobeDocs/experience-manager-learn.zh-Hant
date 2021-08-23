---
title: 使用AEM Sites設定個人化的ContextHub
description: ContextHub是儲存、操控和呈現內容資料的架構。 ContextHub Javascript API可讓您視需要存取儲存區，以建立、更新和刪除資料。 因此，ContextHub代表您頁面上的資料層。 本頁說明如何將內容中樞新增至AEM網站頁面。
feature: 內容中心
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: 個性化
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 1%

---


# 設定個人化的ContextHub {#set-up-contexthub}

ContextHub是儲存、操控和呈現內容資料的架構。 ContextHub Javascript API可讓您視需要存取儲存區，以建立、更新和刪除資料。 因此，ContextHub代表您頁面上的資料層。 本頁說明如何將內容中樞新增至AEM網站頁面。

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>我們會使用WKND參考網站來處理此影片，但該網站不屬於AEM版本。 您可以在此處](https://github.com/adobe/aem-guides-wknd/releases)下載[最新版本。

將ContextHub新增至您的頁面以啟用ContextHub功能並連結至ContextHub JavaScript程式庫。 ContextHub JavaScript API可讓您存取ContextHub管理的內容資料。

## 將ContextHub新增至頁面元件 {#adding-contexthub-to-a-page-component}

若要啟用ContextHub功能並連結至ContextHub JavaScript程式庫，請在網頁的`<head>`區段中加入`contexthub`元件。 頁面元件的HTL程式碼類似下列範例：

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## 網站設定和ContextHub區段 {#site-configuration-and-contexthub-segments}

ContextHub包含區段引擎，可管理區段並判斷要針對目前內容解析哪些區段。 已定義數個區段。 您可以使用Javascript API來[判斷已解析的區段](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)。 在[[!UICONTROL Configuration Browser]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)下啟用您網站的ContextHub區段。

## 建立區段 {#create-segments}

建立AEM區段，作為茶匙的規則。 也就是說，它們會定義宣傳預告內的內容何時出現在網頁上。 接著，內容便可根據訪客的需求和興趣來明確鎖定目標，具體取決於其相符的區段。

## 指派雲端設定、區段路徑和ContextHub路徑至您的網站 {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

將雲端設定路徑、細分路徑和ContextHub路徑指派至您的網站根節點，以便您為對象建立個人化體驗。 使用ContextHub，您可以控制內容資料並測試您解析的區段。

![CRXDE Lite](assets/crx-de-properties.png)

您可以閱讀更多有關ContextHub和區段的資訊，如下所示：

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [新增Context Hub至頁面及存取商店](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [了解區段](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [使用ContextHub設定區段](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
