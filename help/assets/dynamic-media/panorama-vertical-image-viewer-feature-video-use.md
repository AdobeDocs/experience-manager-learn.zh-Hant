---
title: 搭配AEM Assets Dynamic Media使用全景和垂直影像檢視器
description: AEM 6.4中的Dynamic Media檢視器增強功能包括新增全景影像檢視器、全景虛擬現實影像檢視器和垂直影像檢視器。 全景查看器提供一種簡單的方式，讓您無需任何自訂開發，即可享受房間、屬性、位置或景觀的精彩、沈浸式體驗。
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 2%

---

# 搭配AEM Assets Dynamic Media使用全景和垂直影像檢視器{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4中的Dynamic Media檢視器增強功能包括新增全景影像檢視器、全景虛擬現實影像檢視器和垂直影像檢視器。 全景查看器提供一種簡單的方式，讓您無需任何自訂開發，即可享受房間、屬性、位置或景觀的精彩、沈浸式體驗。

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>視訊假設您的AEM例項在Dynamic Media S7模式下執行。 [若需使用Dynamic Media設定AEM的相關指示，請參閱這裡。](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 全景和全景VR檢視器

根據影像的長寬比或關鍵字將影像視為全景。 預設情況下，長寬比為2的影像被視為全景影像。 如果全景影像檢視器預設集滿足上述條件，就可用於影像預覽。 在公司的DMS7設定中，可借由在/conf/global/settings/cloudconfigs/dmscene7/jcr:content指定雙重屬性s7PanoramicAR來修改全景影像外觀比例標準。 關鍵字儲存在資產中繼資料節點的dc:keyword屬性中。 如果關鍵字包含下列任一組合：

* 等矩形，
* 球面+全景，
* 球面+全景，

無論其長寬比為何，都會視為全景影像資產。

## 垂直影像檢視器

使用水準色票時，視消費者的案頭螢幕大小而定，有時在使用者向下捲動頁面前，色票不會顯示。 通過使用垂直影像查看器並放置垂直色板，可確保無論螢幕大小如何，色板都是可見的。 它還使主映像大小最大化。 使用水準色票時，必須在頁面上保留空間，以確保它們有很高的可見性，並且會縮小主影像的大小。 使用垂直版面時，您不需要擔心要分配此空間，因此可最大化主影像大小。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>全景和VR檢視器</td>
   <td>垂直影像檢視器</td>
  </tr>
  <tr>
   <td>Dynamic Media執行模式</td>
   <td>Dynamic Media Scene7模式</td>
   <td>DMS7和Dynamic Media</td>
  </tr>
  <tr>
   <td>使用案例</td>
   <td><p>全景檢視器和虛擬現實檢視器為使用者提供更吸引人的體驗。 用戶甚至在預訂前就可以入住酒店房間，或者無需安排預約即可入住租賃物業。 使用者可以查看位置，並有更多可能性。 這裡的主要重點，是讓消費者在瀏覽您的網站時獲得更好的體驗，並最終提高您的轉換率。</p> <p> </p> </td> 
   <td><p>垂直影像檢視器有助於最大化產品影像檢視體驗，讓消費者能以最佳方式呈現產品，進而促進轉換並將回報減到最少。</p> <p> </p> </td>
  </tr>
  <tr>
   <td>可用 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[在Scene7模式中設定Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[在混合模式中設定Dynamic Media](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>全景檢視器可處理全景影像，不適用於一般影像。
