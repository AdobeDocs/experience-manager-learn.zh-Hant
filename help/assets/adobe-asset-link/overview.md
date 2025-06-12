---
title: Adobe Asset Link 和 AEM
description: 設計師和創意使用者可以在他們最喜歡的 Adobe Creative Cloud 桌面應用程式中使用 Adobe Experience Manager Assets。適用於企業的 Adobe Creative Cloud 的 Adobe Asset Link 擴充功能，可以加強在 Creative Cloud 工具 (如 Adobe XD、Photoshop、InDesign 和 Illustrator) 內搜尋和瀏覽、排序、預覽、上傳資產、簽出、修改、簽入和檢視 AEM Assets 中繼資料的功能。
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '983'
ht-degree: 100%

---


# Adobe Asset Link 3.0

設計師和創意使用者可以在他們最喜歡的 Adobe Creative Cloud 桌面應用程式中使用 Adobe Experience Manager Assets。

適用於企業的 Adobe Creative Cloud 的 Adobe Asset Link 擴充功能，可以加強在 Creative Cloud 應用程式內搜尋和瀏覽、排序、預覽、上傳資產、簽出、修改、簽入和檢視 AEM Assets 中繼資料的功能。

## Adobe Asset Link 功能

+ Adobe Asset Link 與 AEM Assets 和 Assets Essentials 整合
+ Adobe Asset Link 會自動設定與雲端型 AEM 環境 (AEM Assets as a Cloud Service 和 Assets Essentials) 的連線
+ Adobe Asset Link 是在 Adobe Creative Cloud 應用程式內運作的擴充功能：

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ 使用其 Adobe Enterprise ID 或 Federated ID 自動向 AEM 進行驗證
+ 在 AEM 中瀏覽和搜尋數位資產
+ 從面板存取 AEM 中資產的檔案詳細資料：
   + 縮圖
   + 基本中繼資料
   + 版本
+ 將資產放置、下載或拖放到其版面中
+ 修改資產的方法是將其從 AEM 簽出，並在其 Creative Cloud Assets 帳戶中進行處理 (WIP)
+ 修改完成後，將資產重新簽入 AEM，新版本便會反映在 AEM 中
+ 在 Adobe Asset Link 應用程式內面板搜尋 AEM 中的資產
+ 直接在 Asset Link 面板瀏覽 AEM Assets 集合和智慧型集合
+ 直接於面板將新建立的資產新增至 AEM
+ 將資產直接拖放到 InDesign 框架中

## 將資產放置在 InDesign 中

Adobe Asset Link 支援 Adobe Asset Link 和 AEM 之間使用 InDesign 直接連結。藉助 InDesign 直接連結支援，您可以透過 Adobe Asset Link 面板，從 AEM 把數位資產放置 (「__放置連結__」或「__放置副本__」) 或者拖放到 InDesign 中。此外，也推出 *For Placement Only+ (FPO) 轉譯。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>僅能使用您的 Adobe Creative Cloud Enterprise ID 或 Federated ID。確保您有[針對 Adobe Asset Link 進行 AEM 設定](https://helpx.adobe.com/tw/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)。

您可以使用下列其中一個選項將資產放置到 InDesign 版面中：

+ **放置副本** - 嵌入資產 (使用「放置副本」選項) 會在下載二進位檔案到本機系統後，將原始資產的副本放置到 InDesign 版面內。Adobe Asset Link 不會維持嵌入副本和原始資產之間的任何連結。若在 AEM 中修改原始資產，則必須從 InDesign 檔案中刪除嵌入的資產，然後從 AEM 重新嵌入該資產。

+ **放置連結** - 使用 InDesign 文件時，除了直接嵌入資產外，您還可以選擇 (使用內容選單中的「放置副本」選項) 參照來自 AEM 的資產。透過參照資產，您可以和其他使用者協作，並納入對 AEM 中原始資產所做的任何更新。若要參照 AEM 中的資產，請使用內容選單中的「放置連結」選項。

### For Placement Only 影像

使用 Adobe Asset Link 將大型資產檔案從 AEM 放入 InDesign 文件內時，創意使用者在啟動放置作業後需要等待幾秒鐘。這會影響整體的使用者體驗。藉助 Adobe Asset Link，您可以暫時放置來自 AEM 之原始資產的低解析度影像，進而減少放置影像所需的時間。同時，也會提高整體的使用者體驗和生產力。低解析度的影像僅是暫時放置，當需要最終的輸出以進行列印或發佈時，您需要將 FPO 轉譯替換為原始內容。若您想要將多張 FPO 影像替換為對應的原始影像，請導覽至&#x200B;**_「Windows」>「連結」_**&#x200B;面板，然後下載原始資產。下載原始影像後，選擇「將所有 FPO 替換為原始內容」。

FPO 轉譯是原始資產的輕巧版替代品。它們具有相同的外觀比例，但與原始影像相比檔案較小。目前，InDesign 僅支援匯入下列影像類型的 FPO 轉譯：

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

若 AEM 中的特定資產沒有可使用的 FPO 轉譯，則會參照原始的高解析度資產。對於 FPO 影像，狀態 FPO 會顯示在 InDesign 連結面板中。

## 使用 AEM Assets 進行 Adobe Asset Link 驗證

Adobe Asset Link 驗證如何在 Adobe Identity Management Services (IMS) 和 Adobe Experience Manager Author 環境中運作。

![Adobe Asset Link 架構](assets/adobe-asset-link-article-understand.png)

1. Adobe Asset Link 擴充功能會透過 Adobe Creative Cloud 桌面應用程式，向 Adobe Identity Manage Service (IMS) 發出授權要求，若成功後便會收到 Bearer 權杖。
1. Adobe Asset Link 擴充功能會使用擴充功能的設定 JSON 所提供的配置 (HTTP/HTTPS)、主機和連接埠，透過 HTTP(S) 連接至 AEM Author (包括在&#x200B;**步驟 1** 中所取得的 Bearer 權杖)。
1. AEM 的 Bearer 驗證處理常式會從要求中擷取 Bearer 權杖，並使用 Adobe IMS 對其進行驗證。
1. 當 Adobe IMS 完成 Bearer 權杖的驗證後，就會在 AEM 中建立一個使用者 (若尚未建立)，並從 Adobe IMS 同步設定檔和群組/會籍資料。AEM 使用者會獲得一個標準的 AEM 登入權杖，該權杖會以 HTTP 回應上之 Cookie 的形式，傳送回 Adobe Asset Link 擴充功能。
1. 與 Adobe Asset Link 擴充功能的後續互動 (即瀏覽、搜尋、簽入/簽出資產等)，會產生傳送至 AEM Author 的 HTTP 要求，而該要求是使用 AEM 登入權杖，透過標準 AEM 權杖驗證處理常式進行驗證。

>[!NOTE]
>
>登入權杖過期後，**步驟 1 至 5** 會自動叫用，使用 Bearer 權杖對 Adobe Asset Link 擴充功能進行驗證，並重新發出新的有效登入權杖。

## 其他資源

+ [Adobe Asset Link 網站](https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html)
