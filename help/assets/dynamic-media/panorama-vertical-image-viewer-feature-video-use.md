---
title: 利用全景和垂直影像查看器實現AEM Assets·Dynamic Media
description: Dynamic Media查看器在AEM6.4中的增強功能包括添加全景影像查看器、全景虛擬現實影像查看器和垂直影像查看器。 Panoramic Viewer提供一種輕鬆的方式，可讓您輕鬆體驗房間、房產、位置或景觀，無需進行任何定制開發。
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

# 利用全景和垂直影像查看器實現AEM Assets·Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

Dynamic Media查看器在AEM6.4中的增強功能包括添加全景影像查看器、全景虛擬現實影像查看器和垂直影像查看器。 Panoramic Viewer提供一種輕鬆的方式，可讓您輕鬆體驗房間、房產、位置或景觀，無需進行任何定制開發。

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>視頻假設AEM您的實例在Dynamic MediaS7模式下運行。 [有關與Dynamic MediaAEM建立關係的說明，請參閱。](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## 全景和全景VR查看器

根據影像的長寬比或關鍵字來考慮影像的全景。 預設情況下，寬高比為2的影像被視為全景影像。 如果滿足上述條件，全景影像查看器預設將可用於影像預覽。 通過在/conf/global/settings/cloudconfigs/dmscene7/jcr:content處指定雙屬性s7PanoramicAR，可以在公司的DMS7配置中修改全景影像縱橫比標準。 關鍵字儲存在資產元資料節點的dc:keyword屬性中。 如果關鍵字包含以下任意組合：

* 等矩形，
* 球面+全景，
* 球面+全景圖，

無論其長寬比如何，它都被視為全景影像資產。

## 垂直影像查看器

使用水準色板（具體取決於使用者的案頭螢幕大小），有時在用戶向下滾動頁面之前，色板不可見。 通過使用垂直影像查看器並放置垂直色板，它可確保無論螢幕大小如何，色板都是可見的。 它還最大化了主影像大小。 使用水準色板時，有必要在頁面上保留空間，以確保這些色板具有很高的可見性，並會減小主影像的大小。 使用垂直佈局時，您無需擔心分配此空間，因此可以最大化主影像大小。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>全景和VR查看器</td>
   <td>垂直影像查看器</td>
  </tr>
  <tr>
   <td>Dynamic Media運行模式</td>
   <td>Dynamic MediaScene7模式</td>
   <td>DMS7和Dynamic Media</td>
  </tr>
  <tr>
   <td>使用案例</td>
   <td><p>全景查看器和虛擬現實查看器為用戶提供了更有吸引力的體驗。 用戶可以在預訂前查看酒店房間，或者無需安排預約即可查看租賃物業。 用戶可以查看一個位置和更多可能性。 此處的主要重點是，當消費者訪問您的網站時，他們可以提供更好的體驗，並最終提高您的轉換率。</p> <p> </p> </td> 
   <td><p>垂直影像查看器幫助最大限度地提高產品影像的查看體驗，為消費者提供產品的最佳表示，從而推動轉換並最大限度地減少回報。</p> <p> </p> </td>
  </tr>
  <tr>
   <td>可用 </td>
   <td>奧特布</td>
   <td>奧特布</td>
  </tr>
 </tbody>
</table>

[在Scene7模式下配置Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)

[在混合模式下配置Dynamic Media](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>全景觀看器使用全景影像，不應用於正常影像。
