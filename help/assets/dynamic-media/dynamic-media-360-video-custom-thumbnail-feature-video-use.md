---
title: 搭配AEM Assets使用Dynamic Media 360影片和自訂影片縮圖
description: AEM 6.5中的Dynamic Media檢視器增強功能包括新增對360視訊轉譯、360媒體檢視器（video360Social和video360VR）的支援，以及選取自訂視訊縮圖的功能。
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 4%

---

# 搭配AEM Assets使用Dynamic Media 360影片和自訂影片縮圖

AEM 6.5中的Dynamic Media檢視器增強功能包括新增對360視訊轉譯、360媒體檢視器（video360Social和video360VR）的支援，以及選取自訂視訊縮圖的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>視訊假設您的AEM例項在Dynamic Media S7模式下執行。  [若需使用Dynamic Media設定AEM的相關指示，請參閱此處](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html). 上傳影片時，如果長寬比為2:1，依預設，Dynamic Media會將影片處理為360影片。 即寬高比為2:1。

>[!NOTE]
>
>Dynamic Media 360媒體元件僅支援360個影片。

## Dynamic Media 360影片

360度的視頻，也稱為球形視頻，其中記錄了每個方向的視圖，同時使用全向攝像機拍攝，或者收集攝像機。 在平面顯示器上播放期間，使用者可控制觀看方向，而行動裝置上的播放通常採用內建的陀螺控制。  它可讓您超越單一攝影的限制。 行銷人員可透過360部影片，為使用者提供引人入勝的體驗。  我們開始吧。 在公司的DMS7設定中，可借由在/conf/global/settings/cloudconfigs/dmscene7/jcr:content指定雙重屬性s7PanoramicAR來修改全景影像外觀比例標準。

## Dynamic Media 360影片

Dynamic Media影片現在支援為影片選取自訂縮圖的功能。 使用者可以從AEM Assets選取現有資產，或選取視訊影格作為縮圖。

## 動態360媒體檢視器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social檢視器**</td>
      <td>**Video360VR查看器**</td>
   </tr>
   <tr>
      <td>Dynamic Media執行模式</td>
      <td>Dynamic Media Scene7模式</td>
      <td>Dynamic Media Scene7模式<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>使用案例</td>
      <td>
         <p>對於不支援陀螺儀的網站和設備</p>
         <p> </p>
      </td>
      <td>
         <p>為支援陀螺儀的設備提供虛擬現實體驗 </p>
      </td>
   </tr>
   <tr>
      <td>音頻 — 立體聲模式</td>
      <td>否</td>
      <td>是</td>
   </tr>
   <tr>
      <td>視訊播放</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>視點導航</td>
      <td>
         <ul>
            <li>滑鼠拖曳（案頭系統上）</li>
            <li>滑動（觸控裝置）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>已禁用滑鼠和拖動選項</li>
            <li>使用內置陀螺儀</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5播放器</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>社交分享選項</td>
      <td>是</td>
      <td>否</td>
   </tr>
</tbody>
</table>

## 其他資源{#additional-resources}

[在Scene7模式中設定Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
