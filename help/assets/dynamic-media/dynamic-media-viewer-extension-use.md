---
title: 將Dynamic Media觀眾與Adobe Analytics和Adobe發佈結合使用
description: 適用於 Adobe Launch 的 Dynamic Media 檢視器擴充功能及 Dynamic Media 檢視器 5.13 版的發行，可讓 Dynamic Media、Adobe Analytics 和 Adobe Launch 的客戶以其 Adobe Launch 設定使用 Dynamic Media 檢視器專屬的事件和資料。
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 18%

---

# 將Dynamic Media觀眾與Adobe Analytics和Adobe發佈結合使用{#using-dynamic-media-viewers-adobe-analytics-launch}

對於Dynamic Media和Adobe Analytics的客戶，您現在可以使用Dynamic Media查看器分機在您的網站上跟蹤Dynamic Media查看器的使用情況。

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> 在Dynamic MediaScene7模式下運行Adobe Experience Manager以獲取此功能。 你還需要 [將Adobe Experience Platform Launch與你的實AEM例整合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)。

隨著Dynamic Media查看器擴展的推出，Adobe Experience Manager現在為與Dynamic Media查看器(5.13)一起提供的資產提供高級分析支援，在站點頁面上使用Dynamic Media查看器時，可更精確地控制事件跟蹤。

如果您已經擁有AEM Assets和站點，則可以將Launch屬性與作者實AEM例整合。 一旦您的啟動整合與您的網站相關聯，您就可以在啟用了查看者事件跟蹤的情況下將動態媒體元件添加到您的頁面。

對於只有AEM Assets的客戶或Dynamic Media Classic客戶，用戶可以獲取查看者的嵌入代碼並將其添加到頁面。 然後，可以將啟動指令碼庫手動添加到頁面中以進行查看器事件跟蹤。

下表列出了Dynamic Media查看器事件及其支援的參數：

<table>
   <tbody>
      <tr>
         <td>查看器事件名稱</td>
         <td>參數引用</td>
      </tr>
      <tr>
         <td> 通用 </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> 橫幅 <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.roupler% </td>
      </tr>
      <tr>
         <td> 項目 </td>
         <td> %event.detail.dm.ITEM.roupler% </td>
      </tr>
      <tr>
         <td> 載入 </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> 元資料 </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> 里程碑 </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> 頁面 </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> 暫停 </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> 播放 </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> 自旋 </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> 停止 </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> 交換 </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> 色板 </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> 塔格 </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> 縮放 </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## 其他資源{#additional-resources}

* [將Adobe Experience Manager與Adobe發佈整合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [在Dynamic MediaAdobe Experience Manager模式下運行Scene7](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=zh-Hant)
* [整合 Dynamic Media 檢視器以及 Adobe Analytics 和 Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
