---
title: 搭配Adobe Analytics和Adobe Launch使用動態媒體檢視器
seo-title: 搭配Adobe Analytics和Adobe Launch使用動態媒體檢視器
description: Adobe Launch的Dynamic Media Viewers擴充功能以及Dynamic Media Viewers 5.13的發行，可讓Dynamic Media、Adobe Analytics和Adobe Launch的客戶在其Adobe Launch設定中使用Dynamic Media Viewers專屬的事件和資料。
seo-description: 'Adobe Launch的Dynamic Media Viewers擴充功能以及Dynamic Media Viewers 5.13的發行，可讓Dynamic Media、Adobe Analytics和Adobe Launch的客戶在其Adobe Launch設定中使用Dynamic Media Viewers專屬的事件和資料。 '
sub-product: 動態媒體，分析
feature: asset-insights, media-player
topics: images, videos, renditions, authoring, integrations, publishing
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 3%

---


# 搭配Adobe Analytics和Adobe Launch使用動態媒體檢視器{#using-dynamic-media-viewers-adobe-analytics-launch}

對於使用Dynamic Media和Adobe Analytics的客戶，您現在可以使用Dynamic Media Viewer Extension來追蹤網站上Dynamic Media Viewer的使用情形。

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> 在Dynamic Media Scene7模式中執行Adobe Experience Manager，以取得此功能。 您也需要[將Adobe Experience Platform Launch與AEM實例](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)整合。

隨著Dynamic Media Viewer擴充功能的推出，Adobe Experience Manager現在針對動態媒體檢視器(5.13)所提供的資產提供進階分析支援，在「網站」頁面上使用動態媒體檢視器時，可更精細地控制事件追蹤。

如果您已擁有AEM Assets和Sites，您可以將Launch屬性與AEM作者例項整合。 在啟動整合與您的網站建立關聯後，您就可以在啟用檢視器事件追蹤的情況下，將動態媒體元件新增至您的頁面。

對於僅限AEM Assets的客戶或Dynamic Media Classic客戶，使用者可以取得檢視器的內嵌程式碼，並將它新增至頁面。 然後，您可以手動將啟動指令碼程式庫新增至頁面，以進行檢視器事件追蹤。

下表列出動態媒體檢視器事件及其支援的引數：

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

* [將Adobe Experience Manager與Adobe Launch整合](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [在動態媒體場景7模式上執行Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [整合 Dynamic Media 檢視器以及 Adobe Analytics 和 Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
