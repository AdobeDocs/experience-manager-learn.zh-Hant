---
title: 搭配AEM Assets使用Dynamic Media 360視訊和自訂視訊縮圖
seo-title: 搭配AEM Assets使用Dynamic Media 360視訊和自訂視訊縮圖
description: AEM 6.5中的Dynamic Media Viewer增強功能包括新增支援360視訊演算、360媒體檢視器（video360Social和video360VR），以及選取自訂視訊縮圖的功能。
seo-description: AEM 6.5中的Dynamic Media Viewer增強功能包括新增支援360視訊演算、360媒體檢視器（video360Social和video360VR），以及選取自訂視訊縮圖的功能。
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: 動態媒體
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 2%

---


# 搭配AEM Assets使用Dynamic Media 360視訊和自訂視訊縮圖

AEM 6.5中的Dynamic Media Viewer增強功能包括新增支援360視訊演算、360媒體檢視器（video360Social和video360VR），以及選取自訂視訊縮圖的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>視訊會假設您的AEM例項在Dynamic Media S7模式中執行。  [如需有關使用動態媒體設定AEM的指示，請參閱這裡](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。 在您上傳視訊時，如果視訊的外觀比例為2:1,Dynamic Media會依預設將視訊素材處理為360視訊。 即，寬高比為2:1。

>[!NOTE]
>
>動態媒體360媒體元件僅支援360個視訊。

## 動態媒體360影片

360度視頻，又稱為球形視頻，在錄制過程中，每個方向的視圖都被同時錄制，使用全向攝像機拍攝，或者使用攝像機集合拍攝。 在平面顯示器上播放時，使用者可控制觀看方向，而在行動裝置上播放通常採用內建的陀螺控制。  它可讓您突破單一攝影的限制。 行銷人員可透過360個視訊，為使用者提供精彩的體驗。  我們開始吧。 在公司的DMS7配置中，可以通過在/conf/global/settings/cloudconfigs/dmscene7/jcr:content中指定雙屬性s7PanoramicAR來修改全景影像外觀比例標準。

## 動態媒體360影片

動態媒體視訊現在支援為視訊選取自訂縮圖的功能。 使用者可以從AEM Assets選取現有資產，或選取影片影格做為縮圖。

## 動態360媒體檢視器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social檢視器**</td>
      <td>**Video360VR檢視器**</td>
   </tr>
   <tr>
      <td>動態媒體執行模式</td>
      <td>僅限動態媒體Scene7模式</td>
      <td>僅限動態媒體Scene7模式<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>使用案例</td>
      <td>
         <p>針對不支援陀螺儀的網站和裝置</p>
         <p> </p>
      </td>
      <td>
         <p>為支援陀螺儀的裝置提供虛擬實境體驗 </p>
      </td>
   </tr>
   <tr>
      <td>音頻——立體聲模式</td>
      <td>否</td>
      <td>是</td>
   </tr>
   <tr>
      <td>視訊播放</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>視點導覽</td>
      <td>
         <ul>
            <li>滑鼠拖曳（在案頭系統上）</li>
            <li>滑動（觸控裝置）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>停用滑鼠和拖曳選項</li>
            <li>使用內置陀螺</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5 Player</td>
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

[在Scene7模式中設定動態媒體](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
