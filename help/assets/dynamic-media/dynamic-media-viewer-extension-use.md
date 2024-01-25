---
title: 將Dynamic Media Viewers與Adobe Analytics和Adobe Launch搭配使用
description: 適用於 Adobe Launch 的 Dynamic Media 檢視器擴充功能及 Dynamic Media 檢視器 5.13 版的發行，可讓 Dynamic Media、Adobe Analytics 和 Adobe Launch 的客戶以其 Adobe Launch 設定使用 Dynamic Media 檢視器專屬的事件和資料。
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 623
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 16%

---

# 將Dynamic Media Viewers與Adobe Analytics和Adobe Launch搭配使用{#using-dynamic-media-viewers-adobe-analytics-launch}

對於擁有Dynamic Media和Adobe Analytics的客戶，您現在可以使用Dynamic Media Viewer擴充功能來追蹤網站上的Dynamic Media Viewer使用情形。

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> 在Dynamic Media Scene7模式下執行Adobe Experience Manager，以運用此功能。 您也需要 [將Adobe Experience Platform Launch與您的AEM執行個體整合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html).

透過引入Dynamic Media Viewer擴充功能，Adobe Experience Manager現在為使用Dynamic Media檢視器(5.13)傳送的資產提供進階分析支援，在Sites頁面上使用Dynamic Media Viewer時，提供更精細的事件追蹤控制。

如果您已有AEM Assets和Sites，您可以整合Launch屬性與AEM編寫執行個體。 將Launch整合與您的網站建立關聯後，您可以將動態媒體元件新增至頁面，並啟用檢視器的事件追蹤。

若為僅限AEM Assets的客戶或Dynamic Media Classic客戶，使用者可取得檢視器的內嵌程式碼，並將其新增至頁面。 接著，您就可以手動將Launch指令碼程式庫新增至頁面，以追蹤檢視器事件。

下表列出Dynamic Media Viewer事件及其支援的引數：

<table>
   <tbody>
      <tr>
         <td>檢視器事件名稱</td>
         <td>引數參考</td>
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
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> 項目 </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
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
         <td> 中繼資料 </td>
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
         <td> 迴轉 </td>
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
         <td> 色票 </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
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

* [將Adobe Experience Manager與Adobe Launch整合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [在Dynamic Media Scene7模式上執行Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [整合 Dynamic Media 檢視器以及 Adobe Analytics 和 Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
