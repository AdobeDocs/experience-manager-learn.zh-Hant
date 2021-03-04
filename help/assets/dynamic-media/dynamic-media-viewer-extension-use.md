---
title: 將Dynamic Media觀眾與Adobe Analytics及Adobe推出
description: 適用於 Adobe Launch 的 Dynamic Media 檢視器擴充功能及 Dynamic Media 檢視器 5.13 版的發行，可讓 Dynamic Media、Adobe Analytics 和 Adobe Launch 的客戶以其 Adobe Launch 設定使用 Dynamic Media 檢視器專屬的事件和資料。
sub-product: Dynamic Media
feature: 資產 Insights
version: 6.3, 6.4, 6.5
topic: 內容管理
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 16%

---


# 搭配使用Dynamic Media檢視器與Adobe Analytics和Adobe啟動{#using-dynamic-media-viewers-adobe-analytics-launch}

對於擁有Dynamic Media和Adobe Analytics的客戶，您現在可以使用Dynamic Media檢視器擴充功能追蹤網站上Dynamic Media檢視器的使用情況。

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> 以Dynamic MediaScene7模式運行Adobe Experience Manager，以獲得此功能。 您也需要[將Adobe Experience Platform Launch與您的AEM實例](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)整合。

隨著Dynamic Media檢視器擴充功能的推出，Adobe Experience Manager現在針對Dynamic Media檢視器(5.13)所提供的資產提供進階分析支援，在網站頁面上使用Dynamic Media檢視器時，可更精細地控制事件追蹤。

如果您已擁有AEM Assets和網站，您可以將Launch屬性與您的作AEM者例項整合。 在啟動整合與您的網站建立關聯後，您就可以在啟用檢視器事件追蹤的情況下，將動態媒體元件新增至您的頁面。

對於僅限AEM Assets的客戶或Dynamic MediaClassic客戶，使用者可以取得檢視器的內嵌程式碼，並將它新增至頁面。 然後，您可以手動將啟動指令碼程式庫新增至頁面，以進行檢視器事件追蹤。

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

* [將Adobe Experience Manager與Adobe推出整合](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [在Dynamic MediaScene7模式下運行Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dms7.html)
* [整合 Dynamic Media 檢視器以及 Adobe Analytics 和 Adobe Launch](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
