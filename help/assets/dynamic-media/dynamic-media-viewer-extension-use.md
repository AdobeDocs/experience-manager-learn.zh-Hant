---
title: 搭配Adobe Analytics和標籤使用Dynamic Media檢視器
description: 適用於標籤的Dynamic Media Viewers擴充功能，以及Dynamic Media Viewers 5.13版的發行，可讓Dynamic Media、Adobe Analytics和標籤的客戶在其標籤設定中，使用特定於Dynamic Media Viewers的事件和資料。
sub-product: Dynamic Media
feature: Asset Insights
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 576
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---

# 搭配Adobe Analytics和標籤使用Dynamic Media檢視器{#using-dynamic-media-viewers-adobe-analytics-tags}

對於擁有Dynamic Media和Adobe Analytics的客戶，您現在可以使用Dynamic Media檢視器擴充功能來追蹤網站上的Dynamic Media檢視器使用情況。

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> 在Dynamic Media Scene7模式下執行Adobe Experience Manager，以運用此功能。 您也需要[將標籤與您的AEM執行個體整合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=zh-Hant)。

隨著動態媒體檢視器擴充功能的推出，Adobe Experience Manager現在為使用Dynamic Media檢視器(5.13)傳送的資產提供進階分析支援，在Sites頁面上使用Dynamic Media檢視器時，可提供更精細的事件追蹤控制。

如果您已有AEM Assets和Sites，您可以整合標籤屬性與AEM編寫執行個體。 將Launch整合與您的網站建立關聯後，您可以將動態媒體元件新增至頁面，並啟用檢視器的事件追蹤。

若為僅限AEM Assets的客戶或Dynamic Media Classic客戶，使用者可取得檢視器的內嵌程式碼，並將其新增至頁面。 接著，您就可以手動將標籤指令碼資料庫新增至頁面，以追蹤檢視器事件。

下表列出Dynamic Media檢視器事件及其支援的引數：

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

* [整合AEM與Adobe Experience Platform中的標籤](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=zh-Hant)
* [在Dynamic Media Scene7模式下執行Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=zh-Hant)
* [使用標籤整合Dynamic Media檢視器與Adobe Analytics](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html?lang=zh-Hant)
