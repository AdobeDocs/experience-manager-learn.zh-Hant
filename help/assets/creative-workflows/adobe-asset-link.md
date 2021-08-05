---
title: Adobe資產連結和AEM
description: 'Adobe Experience Manager資產可供設計人員和創意使用者在喜愛的Adobe Creative Cloud案頭應用程式中使用。 適用於Adobe Creative Cloud for enterprise的Adobe資產連結擴充功能可擴充在Adobe XD、Photoshop、InDesign和Illustrator等Creative Cloud工具中，搜尋及瀏覽、排序、預覽、上傳資產、結帳、修改、登入及檢視AEM資產的中繼資料。 '
feature: Adobe資產連結
version: 6.4, 6.5, cloud-service
topic: 內容管理
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: 0cfa83bdbd534f0fa06b3fa0013971feb188224e
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 1%

---


# Adobe資產連結3.0

Adobe Experience Manager資產可供設計人員和創意使用者在喜愛的Adobe Creative Cloud案頭應用程式中使用。

適用於Adobe Creative Cloud for enterprise的Adobe資產連結擴充功能可擴充搜尋及瀏覽、排序、預覽、上傳資產、結帳、修改、登入及檢視Creative Cloud應用程式中AEM資產的中繼資料。


## Adobe資產連結和AEM創意工作流程

以下影片說明在Adobe Creative Cloud應用程式中工作、並使用Adobe資產連結直接與AEM整合的創意人員所使用的通用工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/335927/?quality=12&learn=on)

## Adobe資產連結功能

+ Adobe資產連結整合了AEM Assets和Assets Essentials。
+ Adobe資產連結可自動設定與雲端型AEM環境(AEM Assets as a Cloud Service和Assets Essentials)的連線
+ Adobe資產連結是可在Adobe Creative Cloud應用程式中運作的擴充功能：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用AEM的AdobeEnterprise ID或Federated ID自動驗證
+ 在AEM中瀏覽及搜尋數位資產
+ 透過面板存取AEM中資產的檔案詳細資訊：
   + 縮圖
   + 基本中繼資料
   + 版本
+ 放置、下載或拖放資產至其版面
+ 從AEM中籤出資產，並在其「Creative Cloud資產」帳戶中使用資產(WIP)，以修改資產
+ 在資產完成修改後，將資產簽回AEM，新版本將會反映在AEM中
+ 從「Adobe資產連結應用程式內」面板在AEM中搜尋資產
+ 直接從「資產連結」面板瀏覽AEM Assets集合和智慧型集合
+ 直接從面板將新建立的資產新增至AEM
+ 將資產直接拖放至InDesign框架中

## 將資產置於InDesign

Adobe資產連結提供Adobe資產連結和AEM之間的InDesign直接連結支援。 透過InDesign直接連結支援，您可以放置（__放置連結的__&#x200B;或&#x200B;__放置復本__），或透過「Adobe資產連結」面板從AEM拖放數位資產至InDesign。 此外，也推出*For Placement Only+(FPO)轉譯。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>僅使用Adobe Creative CloudEnterprise ID或Federated ID。 請務必[為Adobe資產連結](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)設定AEM。

您可以使用下列其中一個選項，將資產放置到InDesign配置：

+ **置入復本**  — 內嵌資產（使用「置入復本」選項）會在將二進位檔下載至本機系統後，將原始資產的復本放入您的InDesign版面中。Adobe資產連結不會維護內嵌副本與原始資產之間的任何連結。 如果在AEM中修改原始資產，您必須從InDesign檔案中刪除內嵌資產，然後從AEM重新內嵌資產。

+ **放置連結**  — 使用InDesign檔案時，除了直接內嵌資產外，您還可以選擇參照AEM中的資產（使用內容功能表中的「放置復本」選項）。參考資產可讓您與其他使用者共同作業，並在AEM中併入對原始資產所做的任何更新。 若要從AEM參考資產，請使用內容功能表中的「置入連結」選項。

### 僅用於放置影像

使用「InDesign資產連結」從AEM將大型資產檔案放入Adobe檔案時，創意人員使用者在開始放置作業後需要等候幾秒鐘。 這會影響整體使用者體驗。 透過Adobe資產連結，您可以暫時從AEM放置原始資產的低解析度影像，借此縮短放置影像所花的時間。 同時，還能提升整體使用者體驗和生產力。 較低解析度的影像會暫時放置，當需要最終輸出才能打印或發佈時，您需要將FPO轉譯替換為原稿。 如果您想要將多個FPO影像取代為個別的原始影像，請導覽至&#x200B;**_Windows >連結_**&#x200B;面板，然後下載原始資產。 下載原始影像後，選擇「Replace all FPO&#39;s With Originals（用原始影像替換所有FPO）」。

FPO轉譯是原始資產的輕量型取代。 它們具有相同的長寬比，但與原始影像相比尺寸較小。 目前，InDesign僅支援針對下列影像類型匯入FPO轉譯：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

如果AEM中的特定資產無法使用FPO轉譯，則會改為參考原始高解析度資產。 對於FPO影像，狀態FPO顯示在「InDesign連結」面板中。

## Adobe資產連結驗證與AEM Assets

Adobe資產連結驗證如何在AdobeIdentity Management服務(IMS)和Adobe Experience Manager作者的情境下運作。

![Adobe資產連結架構](assets/adobe-asset-link-article-understand.png)

1. Adobe資產連結擴充功能會透過Adobe Creative Cloud案頭應用程式，向Adobe身分管理服務(IMS)提出授權要求，並在成功時收到記號。
1. Adobe資產連結擴充功能會透過HTTP(S)連線至AEM作者，包括透過&#x200B;**Step 1**&#x200B;取得的承載權杖，使用擴充功能設定JSON中提供的配置(HTTP/HTTPS)、主機和連接埠。
1. AEM Bearer Authentication Handler會從請求中擷取Bearer Token，並使用AdobeIMS驗證它。
1. 一旦AdobeIMS驗證承載權杖，就會在AEM中建立使用者（如果尚未存在），並從AdobeIMS同步設定檔和群組/成員資料。 AEM使用者會獲得標準AEM登入Token，並在HTTP(S)回應時以Cookie形式傳回至Adobe資產連結擴充功能。
1. 後續互動(即 瀏覽、搜尋、簽入/簽出資產等) 若AdobeAsset Link擴充功能出現HTTP(S)要求，而AEM Author會使用AEM登入Token，使用標準AEM Token Authentication Handler進行驗證。

>[!NOTE]
>
>登入Token到期後， **步驟1-5**&#x200B;將自動叫用、使用承載Token驗證Adobe資產連結擴充功能，並重新發出新的有效登入Token。

## 其他資源

+ [Adobe資產連結網站](https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html)
