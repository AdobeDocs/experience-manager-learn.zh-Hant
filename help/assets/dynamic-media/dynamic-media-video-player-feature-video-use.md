---
title: 在Dynamic Media使用視頻播AEM放器
description: AEMDynamic Media視頻播放器過去依賴Flash運行時支援案頭客戶端上的自適應視頻流，而瀏覽器在基於快閃記憶體的內容流處理方面變得更加積極。 隨著HLS(Apple的HTTP即時流視頻傳輸協定)的推出，現在無需依賴快閃記憶體就可以流式傳輸內容。
feature: Video Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 5%

---


# 在Dynamic Media使用視頻播AEM放器{#using-the-video-player-in-aem-dynamic-media}

AEMDynamic Media視頻播放器過去依賴Flash運行時支援案頭客戶端上的自適應視頻流，而瀏覽器在基於快閃記憶體的內容流處理方面變得更加積極。 隨著HLS(Apple的HTTP即時流視頻傳輸協定)的推出，現在無需依賴快閃記憶體就可以流式傳輸內容。

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## 快速查看非Flash視頻播放器 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

HLS瀏覽器支援如下所示，對於不支援的瀏覽器，我們退回到漸進式視頻傳輸

>[!NOTE]
>
> Dynamic MediaHybrid自2022年3月15日起不支援Internet Explorer 11上的視頻流。 請升級到6.5.12或更高版本，然後返回到IE 11上的逐步播放。

<table> 
 <thead> 
  <tr> 
   <th> <p>裝置</p> </th>
   <th> <p>瀏覽器</p> </th>
   <th > <p>視頻播放模式</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Internet Explorer 9和10</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr>
   <td> <p>桌面</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>Dynamic Media — 場景7模式：HLS視頻流</p> 
        <p>Dynamic Media — 混合模式：漸進式下載</p>
   </td>
  </tr>
  <tr>
   <td> <p>桌面</p> </td>
   <td> <p>火狐23-44</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Firefox 45或更高版本</p> </td>
   <td> <p>HLS視頻流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>鉻</p> </td>
   <td> <p>HLS視頻流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>薩法里(Mac)</p> </td>
   <td> <p>HLS視頻流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Chrome（Android 6或更早版本）</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Chrome（Android 7或更高版本）</p> </td>
   <td> <p>HLS視頻流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Android（預設瀏覽器）</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>薩法里(iOS)</p> </td>
   <td> <p>HLS視頻流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>克羅姆語(iOS)</p> </td>
   <td> <p>HLS視頻流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS視頻流</p> </td>
  </tr>
 </tbody>
</table>
