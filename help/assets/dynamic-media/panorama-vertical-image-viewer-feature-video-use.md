---
title: 搭配AEM Assets動態媒體使用全景和垂直影像檢視器
seo-title: 搭配AEM Assets動態媒體使用全景和垂直影像檢視器
description: AEM 6.4中的Dynamic Media Viewer增強功能包括新增全景影像檢視器、全景虛擬實境影像檢視器和垂直影像檢視器。 全景檢視器提供輕鬆的方式，提供引人入勝、如臨現場的房間、房產、位置或風景體驗，毋需自訂開發。
seo-description: AEM 6.4中的Dynamic Media Viewer增強功能包括新增全景影像檢視器、全景虛擬實境影像檢視器和垂直影像檢視器。 全景檢視器提供輕鬆的方式，提供引人入勝、如臨現場的房間、房產、位置或風景體驗，毋需自訂開發。
sub-product: 動態媒體
feature: video-profiles, video-profiles, vr-360
topics: videos, renditions, authoring
doc-type: feature video
audience: all
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 1%

---


# 搭配AEM Assets動態媒體使用全景和垂直影像檢視器{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4中的Dynamic Media Viewer增強功能包括新增全景影像檢視器、全景虛擬實境影像檢視器和垂直影像檢視器。 全景檢視器提供輕鬆的方式，提供引人入勝、如臨現場的房間、房產、位置或風景體驗，毋需自訂開發。

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>視訊會假設您的AEM例項在Dynamic Media S7模式中執行。 [如需有關使用動態媒體設定AEM的指示，請參閱這裡。](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 全景與全景VR檢視器

根據影像的長寬比或關鍵字，將影像視為全景圖。 依預設，寬高比為2的影像會視為全景影像。 如果全景影像檢視器預設集符合上述條件，就可用於影像預覽。 在公司的DMS7配置中，可以通過在/conf/global/settings/cloudconfigs/dmscene7/jcr:content中指定雙屬性s7PanoramicAR來修改全景影像外觀比例標準。 關鍵字會儲存在資產中繼資料節點的dc:keyword屬性中。 如果關鍵字包含下列任一組合：

* 等矩形，
* 球面+全景，
* 球面+全景，

無論其外觀比例如何，都會被視為全景影像資產。

## 垂直影像檢視器

使用水準色票時，視消費者的案頭螢幕大小而定，有時只有使用者向下捲動頁面後，才會顯示色票。 使用垂直影像檢視器並放置垂直色票，可確保不論螢幕大小，都能顯示色票。 它也可最大化主影像大小。 使用水準色票時，必須保留頁面上的空間，以確保其可見性高，並且會減少主影像的大小。 使用垂直版面時，您不需要擔心分配此空間，因此可以最大化主影像大小。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>全景和VR檢視器</td>
   <td>垂直影像檢視器</td>
  </tr>
  <tr>
   <td>動態媒體執行模式</td>
   <td>僅限動態媒體Scene7模式</td>
   <td>DMS7和動態媒體</td>
  </tr>
  <tr>
   <td>使用案例</td>
   <td><p>全景檢視器和虛擬實境檢視器為使用者提供更吸引人的體驗。 使用者可以在預訂前查看酒店房間，或者不需要安排預約即可查看租賃房產。 使用者可以查看位置和更多可能性。 這裡的主要重點是為消費者在瀏覽您的網站時提供更好的體驗，並最終提高您的轉換率。</p> <p> </p> </td> 
   <td><p>垂直影像檢視器可協助最大化產品影像檢視體驗，讓消費者獲得產品的最佳呈現方式，進而推動轉化並將回報降至最低。</p> <p> </p> </td>
  </tr>
  <tr>
   <td>可用 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[在Scene7模式中設定動態媒體](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[以混合模式配置動態媒體](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>全景檢視器可處理全景影像，不能用於一般影像。
