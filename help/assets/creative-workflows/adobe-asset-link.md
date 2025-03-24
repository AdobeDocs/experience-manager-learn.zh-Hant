---
title: Adobe Asset Link和AEM
description: 設計師和創意使用者可在他們最愛的Adobe Experience Manager案頭應用程式中使用Adobe Creative Cloud資產。 適用於企業的Adobe Creative Cloud的Adobe Asset Link擴充功能加強在Creative Cloud工具(如Adobe XD、Photoshop、InDesign和Illustrator)中搜尋和瀏覽、排序、預覽、上傳資產、取出、修改、存回和檢視AEM資產的中繼資料的功能。
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
doc-type: Feature Video
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
duration: 1027
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 1%

---

# Adobe資產連結3.0

設計師和創意使用者可在他們最愛的Adobe Experience Manager案頭應用程式中使用Adobe Creative Cloud資產。

適用於企業的Adobe Creative Cloud的Adobe Asset Link擴充功能加強在Creative Cloud應用程式中搜尋和瀏覽、排序、預覽、上傳資產、取出、修改、存回和檢視AEM資產的中繼資料的功能。

>[!TIP]
>
> 深入瞭解[Adobe XD Premium訓練計畫](https://helpx.adobe.com/support/xd.html)如何協助您將Asset Link與Adobe Experience Manager工作流程整合。

## Adobe Asset Link和AEM創意工作流程

以下影片說明在Adobe Creative Cloud應用程式中工作，並使用Adobe Asset Link直接與AEM整合的創意人員所使用的常見工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## Adobe Asset Link功能

+ Adobe Asset Link已與AEM Assets和Assets Essentials整合。
+ Adobe Asset Link會自動設定與雲端型AEM環境(AEM Assets as a Cloud Service和Assets Essentials)的連線
+ Adobe Asset Link是適用於Adobe Creative Cloud應用程式的擴充功能：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用其Adobe Enterprise ID或Federated ID自動驗證AEM
+ 在AEM中瀏覽及搜尋數位資產
+ 使用面板存取AEM中資產的檔案詳細資訊：
   + 縮圖
   + 基本中繼資料
   + 版本
+ 將資產放置、下載或拖放至其版面
+ 從AEM中籤出資產，並在其Creative Cloud Assets帳戶中處理資產(WIP)，以修改資產
+ 資產完成修改後，將資產簽回AEM，新版本會反映在AEM中
+ 從Adobe Asset Link應用程式內面板搜尋AEM中的資產
+ 直接從Asset Link面板瀏覽AEM Assets集合和智慧型集合
+ 直接從面板將新建立的資產新增至AEM
+ 將資產直接拖放至InDesign框架

## 在InDesign中放置資產

Adobe Asset Link提供InDesign Asset Link與Adobe AEM之間的直接連結支援。 透過InDesign直接連結支援，您可以透過Adobe Asset Link面板，置入（__置入已連結__&#x200B;或&#x200B;__置入副本__）或從AEM拖放數位資產至InDesign。 此外，也介紹*For Placement Only+ (FPO)轉譯。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>僅使用您的Adobe Creative Cloud Enterprise ID或Federated ID。 請確定您[為Adobe資產連結設定AEM](https://helpx.adobe.com/tw/enterprise/using/adobe-asset-link.html)。

您可以使用下列其中一個選項，將資產放置到InDesign版面配置中：

+ **置入副本** — 內嵌資產（使用「置入副本」選項）會在將二進位檔下載至本機系統後，將原始資產的副本置入您的InDesign版面中。 Adobe Asset Link不會維持內嵌復本與原始資產之間的任何連結。 如果原始資產在AEM中經過修改，您必須從InDesign檔案中刪除內嵌資產，然後從AEM重新內嵌資產。

+ **置入已連結** — 使用InDesign檔案時，除了直接內嵌資產外，您還可以選擇參照AEM的資產（使用內容功能表中的「置入副本」選項）。 引用資產可讓您與其他使用者共同作業，並在AEM中合併對原始資產所做的任何更新。 若要參照AEM中的資產，請使用快顯選單中的「置入連結」選項。

### 僅供置入的影像

當使用InDesign Asset Link從AEM將大型資產檔案置入Adobe檔案時，創意使用者在起始置入操作後需要等候幾秒鐘。 這會影響整體的使用者體驗。 透過Adobe Asset Link，您可以暫時從AEM置入原始資產的低解析度影像，減少置入影像所需的時間。 同時，還能提升整體使用者體驗和生產力。 較低解析度的影像會暫時放置，當列印或發佈需要最終輸出時，您需要以原始影像取代FPO轉譯。 如果您想要以個別的原始影像取代多個FPO影像，請瀏覽至&#x200B;**_Windows >連結_**&#x200B;面板，然後下載原始資產。 下載原始影像後，請選擇以原始影像取代所有FPO。

FPO轉譯是原始資產的輕量級替代專案。 它們的外觀比例相同，但與原始影像相比，大小較小。 目前InDesign僅支援匯入下列影像型別的FPO轉譯：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

如果FPO轉譯無法用於AEM中的特定資產，則會改為參考原始高解析度資產。 若為FPO影像，狀態FPO會顯示在「InDesign連結」面板中。

## 使用AEM Assets進行Adobe Asset Link驗證

Adobe Asset Link驗證如何在Adobe Identity Management Services (IMS)和Adobe Experience Manager Author的內容中運作。

![Adobe資產連結架構](assets/adobe-asset-link-article-understand.png)

1. Adobe Asset Link擴充功能會透過Adobe Creative Cloud案頭應用程式向Adobe Identity Manage Service (IMS)提出授權請求，並在成功後接收持有人權杖。
1. Adobe Asset Link擴充功能會透過HTTP(S)連線至AEM Author，包括在&#x200B;**步驟1**&#x200B;中取得的持有人權杖，使用擴充功能設定JSON中提供的配置(HTTP/HTTPS)、主機和連線埠。
1. AEM的持有者驗證處理常式會從要求中擷取持有者權杖，並使用Adobe IMS進行驗證。
1. Adobe IMS驗證持有人權杖後，就會在AEM中建立使用者（如果尚未存在），並從Adobe IMS同步設定檔和群組/成員資格資料。 AEM使用者會獲得標準AEM登入權杖，並會根據HTTP(S)回應以Cookie形式傳回Adobe Asset Link擴充功能。
1. 後續互動(即 使用Adobe Asset Link擴充功能來瀏覽、搜尋、存回/取出資產等，會對AEM作者產生HTTP(S)請求，這些請求會使用標準AEM登入權杖驗證處理常式，通過AEM登入權杖進行驗證。

>[!NOTE]
>
>登入權杖到期後，**步驟1-5**&#x200B;將自動叫用，使用持有人權杖驗證Adobe Asset Link擴充功能，並重新發出新的有效登入權杖。

## 其他資源

+ [Adobe資產連結網站](https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html)
