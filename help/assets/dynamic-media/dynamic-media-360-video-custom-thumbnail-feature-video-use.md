---
title: 搭配AEM Assets使用Dynamic Media360視訊和自訂視訊縮圖
description: Dynamic Media檢視器AEM6.5版的增強功能包括支援360視訊轉換、360種媒體檢視器（video360Social和video360VR），以及選取自訂視訊縮圖的功能。
sub-product: 動態媒體
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 4%

---


# 搭配AEM Assets使用Dynamic Media360視訊和自訂視訊縮圖

Dynamic Media檢視器AEM6.5版的增強功能包括支援360視訊轉換、360種媒體檢視器（video360Social和video360VR），以及選取自訂視訊縮圖的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>視訊假設您AEM的例項在Dynamic MediaS7模式下執行。  [有關與Dynamic MediaAEM設定的說明可在這裡找到](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。在您上傳視訊時，依預設，如果視訊長寬比為2:1,Dynamic Media會將視訊素材處理為360視訊。 即，寬高比為2:1。

>[!NOTE]
>
>Dynamic Media360媒體元件僅支援360個視訊。

## Dynamic Media360影片

360度視頻，又稱為球形視頻，在錄制過程中，每個方向的視圖都被同時錄制，使用全向攝像機拍攝，或者使用攝像機集合拍攝。 在平面顯示器上播放時，使用者可控制觀看方向，而在行動裝置上播放通常採用內建的陀螺控制。  它可讓您突破單一攝影的限制。 行銷人員可透過360個視訊，為使用者提供精彩的體驗。  我們開始吧。 在公司的DMS7配置中，可以通過在/conf/global/settings/cloudconfigs/dmscene7/jcr:content中指定雙屬性s7PanoramicAR來修改全景影像外觀比例標準。

## Dynamic Media360影片

Dynamic Media視訊現在支援為視訊選取自訂縮圖的功能。 使用者可以從AEM Assets選取現有資產，或選取視訊影格做為縮圖。

## 動態360媒體檢視器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social檢視器**</td>
      <td>**Video360VR檢視器**</td>
   </tr>
   <tr>
      <td>Dynamic Media運行模式</td>
      <td>Dynamic MediaScene7模式</td>
      <td>Dynamic MediaScene7模式<br>
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

[在Scene7模式下配置Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
