---
title: 搭配使用Dynamic Media檢視器及Adobe Analytics和AdobeLaunch
description: 適用於 Adobe Launch 的 Dynamic Media 檢視器擴充功能及 Dynamic Media 檢視器 5.13 版的發行，可讓 Dynamic Media、Adobe Analytics 和 Adobe Launch 的客戶以其 Adobe Launch 設定使用 Dynamic Media 檢視器專屬的事件和資料。
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 15%

---


# 搭配使用Dynamic Media檢視器及Adobe Analytics和AdobeLaunch{#using-dynamic-media-viewers-adobe-analytics-launch}

若為擁有Dynamic Media和Adobe Analytics的客戶，您現在可以使用Dynamic Media檢視器擴充功能，追蹤您網站上Dynamic Media檢視器的使用情況。

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> 針對此功能，以Dynamic Media Scene7模式執行Adobe Experience Manager。 您也需要[將Adobe Experience Platform Launch與您的AEM例項](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)整合。

隨著Dynamic Media檢視器擴充功能的推出，Adobe Experience Manager現在為Dynamic Media檢視器(5.13)提供的資產提供進階分析支援，讓您在網站頁面上使用Dynamic Media檢視器時，能更精細地控制事件追蹤。

如果您已有AEM Assets和Sites，則可將Launch屬性與AEM作者例項整合。 將launch整合與網站建立關聯後，您就可以在啟用檢視器的事件追蹤的情況下，將動態媒體元件新增至頁面。

若為僅限AEM Assets的客戶或Dynamic Media Classic客戶，使用者可以取得檢視器的內嵌程式碼，並將其新增至頁面。 接著，您就可以手動將Launch指令碼程式庫新增至頁面，以便進行檢視器事件追蹤。

下表列出Dynamic Media檢視器事件及其支援的引數：

<table>
   <tbody>
      <tr>
         <td>檢視器事件名稱</td>
         <td>引數參考</td>
      </tr>
      <tr>
         <td> 常見 </td>
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
         <td> %event.detail.dm.HREF.rolver% </td>
      </tr>
      <tr>
         <td> 項目 </td>
         <td> %event.detail.dm.ITEM.rolver% </td>
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
         <td> 回轉 </td>
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

* [將Adobe Experience Manager與AdobeLaunch整合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
* [在Dynamic Media Scene7模式中執行Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=en)
* [整合 Dynamic Media 檢視器以及 Adobe Analytics 和 Adobe Launch](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
