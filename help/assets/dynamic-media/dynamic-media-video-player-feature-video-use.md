---
title: 在Dynamic Media使用視訊播AEM放器
description: AEMDynamic Media視訊播放器過去依賴Flash執行時期來支援案頭用戶端和瀏覽器上的可調式視訊串流，現在在快閃內容串流上變得更為積極。 隨著HLS（Apple的HTTP即時串流視訊傳送通訊協定）的推出，內容現在可以串流化，而不需仰賴Flash。
sub-product: 動態媒體
feature: 視訊設定檔
version: 6.3, 6.4, 6.5
topic: 內容管理
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 7%

---


# 在Dynamic Media使AEM用視頻播放器{#using-the-video-player-in-aem-dynamic-media}

AEMDynamic Media視訊播放器過去依賴Flash執行時期來支援案頭用戶端和瀏覽器上的可調式視訊串流，現在在快閃內容串流上變得更為積極。 隨著HLS（Apple的HTTP即時串流視訊傳送通訊協定）的推出，內容現在可以串流化，而不需仰賴Flash。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 快速瞭解非Flash視訊播放器{#quick-look-into-non-flash-video-player}

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