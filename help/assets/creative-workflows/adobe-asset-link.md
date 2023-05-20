---
title: Adobe資產鏈AEM接
description: Adobe Experience Manager資產可供設計師和創意用戶在其喜愛的Adobe Creative Cloud案頭應用程式中使用。 AdobeAdobe Creative Cloud for enterprise資產連結擴展擴展了在Adobe XD、Photoshop、InDesign和Illustrator等Creative Cloud工具中搜索和瀏覽、排序、預覽、上傳資產、簽出、修改、簽入和查看資產元資料的功能AEM。
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
last-substantial-update: 2022-06-25T00:00:00Z
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 1%

---

# Adobe資產連結3.0

Adobe Experience Manager資產可供設計師和創意用戶在其喜愛的Adobe Creative Cloud案頭應用程式中使用。

AdobeAdobe Creative Cloud for enterprise的資產連結擴展擴展了搜索和瀏覽、排序、預覽、上載資產、簽出、修改、簽入和查看Creative Cloud應用程式內AEM資產的元資料的功能。

>[!TIP]
>
> 瞭解有關 [Adobe XD高級培訓計畫](https://spark.adobe.com/page/wU7OXv8qKGugO/) 可幫助您將資產連結與您的Adobe Experience Manager工作流整合。

## Adobe資產連結和創AEM作工作流

下面的視頻展示了在Adobe Creative Cloud應用程式中工作的創意人員使用的通用工作流，並直接與使用「Adobe資產鏈AEM接」進行整合。

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## Adobe資產連結功能

+ Adobe資產連結與AEM Assets和Assets Essentials整合。
+ Adobe資產連結自動配置到基於雲的環境(AEMAEM Assetsas a Cloud Service和Assets Essentials)的連接
+ Adobe資產連結是Adobe Creative Cloud應用程式中的擴展：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用其AdobeAEMEnterprise ID或Federated ID進行自動身份驗證
+ 瀏覽並搜索中的數字資AEM產
+ 通過面板訪問駐留在AEM中的資產的檔案詳細資訊：
   + 縮圖
   + 基本元資料
   + 版本
+ 將資產放置、下載或拖放到其佈局中
+ 通過從其Creative Cloud資產帳AEM戶中籤出資產並處理其(WIP)來修改資產
+ 在資產完成修AEM改後將其檢回，新版本將反映在
+ 在「Adobe資AEM產連結In-App」面板中搜索資產
+ 直接從「資產連結」面板瀏覽AEM Assets收藏和智慧收藏
+ 將新建立的資AEM產直接從面板
+ 將資產直接拖放到InDesign幀中

## 將資產置於InDesign

Adobe資產連結在InDesign資產連結和之間提供Adobe直接連結支AEM持。 通過InDesign直接連結支援，您可以放置(__連結位置__ 或 __放置副本__)或通過「InDesign資產連結」面板將數AEM字資產拖放到Adobe中。 此外，還介紹了*For Placement Only+(FPO)格式副本。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>僅使用您的Adobe Creative CloudEnterprise ID或Federated ID。 確保 [配置AEMAdobe資產連結](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)。

您可以使用以下選項之一將資產置於InDesign佈局中：

+ **放置副本**  — 在將二進位檔案下載到本地系統後，嵌入資產（使用「放置複製」選項）會將原始資產的副本放入InDesign佈局中。 Adobe資產連結不維護嵌入副本與原始資產之間的任何連結。 如果在中修改了原始AEM資產，則必須從InDesign檔案中刪除嵌入的資產，然後從中重新嵌入該AEM資產。

+ **連結位置**  — 使用InDesign文檔時，除了直接嵌入資產外，您AEM還可以選擇引用資產（使用上下文菜單中的「放置複製」選項）。 引用資產允許您與其他用戶協作並合併對原始資產所做的任何更AEM新。 要從中引用資AEM產，請使用上下文菜單中的「放置連結」選項。

### 僅用於放置影像

當使用InDesign資產連結將大型資產文AEM件放入Adobe文檔時，創意用戶需要在啟動放置操作後等待幾秒鐘。 這會影響總體用戶體驗。 使用Adobe資產連結，您可以暫時放置原始資產的低解析度影像AEM，從而縮短放置影像所花的時間。 同時，它提高了整體用戶體驗和工作效率。 將臨時放置解析度較低的影像，當打印或發佈需要最終輸出時，您需要將FPO格式副本替換為原始格式。 如果要將多個FPO影像替換為各自的原始影像，請導航至 **_Windows >連結_** 下載原始資產。 下載原始影像後，選擇「Replace all FPO&#39;s With Originals（用原始影像替換所有FPO）」。

FPO格式副本是原始資產的輕量替代。 它們具有相同的長寬比，但與原始影像相比，它們的尺寸較小。 目前，InDesign僅支援為以下映像類型導入FPO格式副本：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

如果FPO格式副本不可用於中的特定資AEM產，則引用原始高解析度資產。 對於FPO影像，狀態FPO顯示在「InDesign連結」面板中。

## Adobe資產連結與AEM Assets的身份驗證

Adobe資產連結認證如何在AdobeIdentity Management服務公司(IMS)和Adobe Experience Manager作者的背景下工作。

![Adobe資產連結體系結構](assets/adobe-asset-link-article-understand.png)

1. Adobe資產鏈路擴展通過Adobe Creative Cloud案頭應用向Adobe身份管理服務(IMS)發出授權請求，並且一旦成功，接收持牌令牌。
1. Adobe資產鏈路擴展通過HTTP(S)連接到AEM作者，包括在 **步驟1**，使用擴展設定JSON中提供的方案(HTTP/HTTPS)、主機和埠。
1. 承AEM載驗證處理程式從請求中提取承載令牌並用Adobe IMS驗證。
1. 一旦Adobe IMS驗證了Bearer令牌，就會在中建立用戶AEM（如果尚不存在），並從Adobe IMS同步配置檔案和組/成員身份資料。 向用AEM戶頒發標準登AEM錄令牌，該令牌作為HTTP(S)響應上的Cookie發回Adobe資產連結擴展。
1. 後續交互(即 瀏覽、搜索、簽入/簽出資產等) Adobe資產連結擴展導致向AEM作者發出HTTP(S)請求，這些請求使用標準令牌驗證處理程式AEM使用登錄令牌AEM進行驗證。

>[!NOTE]
>
>登錄令牌到期後， **步驟1-5** 將自動調用、驗證Adobe資產鏈路擴展，並使用持有者令牌，重新發出新的有效登錄令牌。

## 其他資源

+ [Adobe資產連結網站](https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html)
