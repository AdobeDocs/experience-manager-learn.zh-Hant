---
title: 在AEM Dynamic Media中使用視訊播放器
description: AEM Dynamic Media視訊播放器過去依賴Flash執行階段來支援案頭用戶端上的最適化視訊串流，而現在則更積極採用flash內容串流。 隨著HLS(Apple的HTTP即時串流視訊傳送通訊協定)的推出，內容現在可以串流化，而不需依賴Flash。
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: d8738ef33940efc856e6fc00f6a57abb641598e5
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 6%

---

# 在AEM Dynamic Media中使用視訊播放器{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media視訊播放器過去依賴Flash執行階段來支援案頭用戶端上的最適化視訊串流，而現在則更積極採用flash內容串流。 隨著HLS(Apple的HTTP即時串流視訊傳送通訊協定)的推出，內容現在可以串流化，而不需依賴Flash。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 快速了解非Flash視訊播放器 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

針對不支援的瀏覽器，我們會回復至漸進式視訊傳送，HLS瀏覽器支援如下

>[!NOTE]
>
> Dynamic Media Hybrid在2022年5月後將不支援Internet Explorer 11。

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
   <td> <p>鉻黃</p> </td>
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
