---
title: 將Dynamic Media360視頻和自定義視頻縮略圖與AEM Assets
description: Dynamic Media觀眾在AEM6.5中的增強功能包括對360個視頻渲染、360個媒體觀看者（video360Social和video360VR）以及選擇自定義視頻縮略圖的能力的增加。
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

# 將Dynamic Media360視頻和自定義視頻縮略圖與AEM Assets

Dynamic Media觀眾在AEM6.5中的增強功能包括對360個視頻渲染、360個媒體觀看者（video360Social和video360VR）以及選擇自定義視頻縮略圖的能力的增加。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=12&learn=on)

>[!NOTE]
>
>視頻假設AEM您的實例在Dynamic MediaS7模式下運行。  [有關與Dynamic MediaAEM建立關係的說明，請參閱](https://helpx.adobe.com/tw/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。 在上傳視頻時，預設情況下，如果視頻的長寬比為2:1,Dynamic Media會將視頻處理為360視頻。 即寬高比為2:1。

>[!NOTE]
>
>Dynamic Media360媒體元件僅支援360個視頻。

## Dynamic Media360個視頻

360度視頻，又稱球形視頻，是錄像，在錄像中，每個方向的視圖都同時被記錄，使用全向攝像機拍攝，或者採集攝像機。 在平面顯示器上回放期間，用戶對觀看方向有控制，在移動設備上回放通常利用內置的陀螺儀控制。  它讓你超越了單一攝影的極限。 營銷人員可以通過360個視頻為用戶提供令人著迷的體驗。  開始吧。 通過在/conf/global/settings/cloudconfigs/dmscene7/jcr:content處指定雙屬性s7PanoramicAR，可以在公司的DMS7配置中修改全景影像縱橫比標準。

## Dynamic Media360個視頻

Dynamic Media視頻現在支援為您的視頻選擇自定義縮略圖。 用戶可以從AEM Assets選擇現有資產或選擇視頻幀作為縮略圖。

## 動態360媒體查看器

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Social Viewer**</td>
      <td>**Video360VR查看器**</td>
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
      <td>視頻播放</td>
      <td>是</td>
      <td>是</td>
   </tr>
   <tr>
      <td>視點導航</td>
      <td>
         <ul>
            <li>滑鼠拖動（在案頭系統上）</li>
            <li>輕掃（觸摸設備）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>禁用滑鼠和拖動選項</li>
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
      <td>社交共用選項</td>
      <td>是</td>
      <td>否</td>
   </tr>
</tbody>
</table>

## 其他資源{#additional-resources}

[在Scene7模式下配置Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
