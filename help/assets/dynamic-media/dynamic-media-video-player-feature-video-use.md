---
title: 在AEM Dynamic Media中使用視訊播放器
seo-title: 在AEM Dynamic Media中使用視訊播放器
description: AEM Dynamic Media視訊播放器過去依賴Flash執行時期來支援案頭用戶端和瀏覽器上的可調式視訊串流，現在在Flash內容串流上變得更為積極。 隨著HLS（Apple的HTTP即時串流視訊傳送通訊協定）的推出，內容現在可以串流化，而不需仰賴Flash。
seo-description: AEM Dynamic Media視訊播放器過去依賴Flash執行時期來支援案頭用戶端和瀏覽器上的可調式視訊串流，現在在Flash內容串流上變得更為積極。 隨著HLS（Apple的HTTP即時串流視訊傳送通訊協定）的推出，內容現在可以串流化，而不需仰賴Flash。
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: 動態媒體
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 5%

---


# 在AEM Dynamic Media中使用視訊播放器{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media視訊播放器過去依賴Flash執行時期來支援案頭用戶端和瀏覽器上的可調式視訊串流，現在在Flash內容串流上變得更為積極。 隨著HLS（Apple的HTTP即時串流視訊傳送通訊協定）的推出，內容現在可以串流化，而不需仰賴Flash。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 快速檢視非Flash Video Player {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLS瀏覽器支援如下：針對不支援的瀏覽器，我們會後援至漸進式視訊傳送

<table> 
 <thead> 
  <tr> 
   <th> <p>裝置</p> </th>
   <th> <p>瀏覽器</p> </th>
   <th > <p>視訊播放模式</p> </th>
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
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr>
   <td> <p>桌面</p> </td>
   <td> <p>Firefox 23-44</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Firefox 45或更新版本</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>桌面</p> </td>
   <td> <p>Safari(Mac)</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Chrome（Android 6或更舊版本）</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Chrome（Android 7或更新版本）</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Android（預設瀏覽器）</p> </td>
   <td> <p>漸進式下載</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Safari(iOS)</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Chrome(iOS)</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
  <tr> 
   <td> <p>行動</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS視訊串流</p> </td>
  </tr>
 </tbody>
</table>