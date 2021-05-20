---
title: 搭配AEM Assets使用Adobe資產連結擴充功能
description: 'Adobe Experience Manager資產現在可供設計人員和創意使用者在喜愛的Adobe Creative Cloud案頭應用程式中使用。 Adobe Creative Cloud企業版的Adobe資產連結擴充功能可擴充搜尋和瀏覽、排序、預覽、上傳資產、結帳、修改、簽入及檢視AEM資產的中繼資料，這些功能可在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中使用。 '
feature: Adobe資產連結
version: 6.4, 6.5
topic: 內容管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 1%

---


# 搭配AEM Assets使用Adobe資產連結擴充功能{#using-adobe-asset-link-extension-with-aem-assets}

Adobe Experience Manager資產現在可供設計人員和創意使用者在喜愛的Adobe Creative Cloud案頭應用程式中使用。 Adobe Creative Cloud企業版的Adobe資產連結擴充功能可擴充搜尋和瀏覽、排序、預覽、上傳資產、結帳、修改、簽入及檢視AEM資產的中繼資料，這些功能可在Adobe Photoshop、InDesign和Illustrator等Creative Cloud工具中使用。


## Adobe資產連結1.1

Adobe資產連結v1.1現在提供Adobe資產連結和AEM Assets之間的InDesign直接連結支援。 透過InDesign直接連結支援，您現在可以放置（放置連結或放置副本），或透過Adobe資產連結面板從AEM Assets拖放數位資產至InDesign。 此外，也推出&#x200B;*For Placement Only*(FPO)轉譯。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>僅使用Adobe Creative CloudEnterprise ID或Federated ID。 請務必[為Adobe資產連結](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)設定AEM。


### Adobe資產連結功能

* Adobe資產連結是可在PS、AI、ID中運作的擴充功能，可直接存取存放於AEM Assets的數位資產
* 創作者將會使用其AdobeIMSEnterprise ID或Federated ID自動登入AEM
* 除了在AEM Assets和Creative Cloud資產間搜尋外，創意人員還可以瀏覽存放在AEM Assets的數位資產
* 創意人員可存取位於AEM Assets的資產的檔案詳細資訊；從面板取得縮圖、基本中繼資料和版本
* 創意人員可以放置、下載或拖放資產至版面
* 創作者可從AEM Assets中籤出資產，並在其Creative Cloud資產帳戶中處理資產(WIP)，借此修改資產
* 創意人員完成修改後，可以將資產簽回AEM Assets，新版本將反映在AEM Assets
* 支援Creative Cloud2020、2019和2018InDesign、Photoshop和Illustrator案頭應用程式
* 使用者可以從「Adobe資產連結應用程式內」面板執行資產搜尋，並依大小、字母順序和關聯性來排序
* 使用者可以直接從「資產連結」面板存取及瀏覽AEM Assets集合和智慧型集合
* 直接從面板將新建立的資產新增至AEM Assets
* 使用者可以直接將資產拖放至InDesign框架中

### 將AEM Assets放入InDesign

您可以使用下列其中一個選項，將資產放置到InDesign配置：

* **置入復本**  — 內嵌資產（使用「置入復本」選項）會在將二進位檔下載至本機系統後，將原始資產的復本放入您的InDesign版面中。Adobe資產連結不會維護內嵌副本與原始資產之間的任何連結。 如果原始資產在AEM Assets中修改，您必須從InDesign檔案中刪除內嵌資產，然後從AEM Assets重新內嵌資產。

* **放置連結**  — 使用InDesign檔案時，除了直接內嵌資產外，現在還可以選擇參照AEM Assets的資產（使用內容功能表中的「放置復本」選項）。參考資產可讓您與其他使用者共同作業，並合併對AEM Assets中原始資產所做的任何更新。 若要從AEM Assets參考資產，請使用內容功能表中的「放置連結」選項。

### 僅限投放(FPO)解析度

使用「InDesign資產連結」從AEM Assets將大型資產檔案放入Adobe檔案時，創意人員使用者在開始放置作業後需要等候幾秒鐘。 這會影響整體使用者體驗。 透過Adobe資產連結，您現在可以暫時放置來自AEM Assets之原始資產的低解析度影像，借此縮短放置影像所花的時間。 同時，還能提升整體使用者體驗和生產力。 較低解析度的影像會暫時放置，當需要最終輸出才能打印或發佈時，您需要將FPO轉譯替換為原稿。 如果您想要將多個FPO影像取代為個別的原始影像，請導覽至&#x200B;**_Windows >連結_**&#x200B;面板，然後下載原始資產。 下載原始影像後，選擇「Replace all FPO&#39;s With Originals（用原始影像替換所有FPO）」。

>[!NOTE]
>
> *僅適用於Placement(FPO)* 轉譯只適用於「放置連結」選項。您也應該在AEM Assets *Dam更新資產*&#x200B;工作流程中啟用FPO轉譯支援。

FPO轉譯是原始資產的輕量型取代。 它們具有相同的長寬比，但與原始影像相比尺寸較小。 目前，InDesign僅支援針對下列影像類型匯入FPO轉譯：

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

如果AEM Assets中的特定資產無法使用FPO轉譯，則會改為參考原始的高解析度資產。 對於FPO影像，狀態FPO顯示在「InDesign連結」面板中。

## 了解Adobe資產連結驗證與AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Adobe資產連結驗證如何在AdobeIdentity Management服務(IMS)和Adobe Experience Manager作者的情境下運作。

![Adobe資產連結架構](assets/adobe-asset-link-article-understand.png)

下載[Adobe資產連結架構](assets/adobe-asset-link-article-understand-1.png)

1. Adobe資產連結擴充功能會透過Adobe Creative Cloud案頭應用程式，向Adobe身分管理服務(IMS)提出授權要求，並在成功時收到記號。
2. Adobe資產連結擴充功能會透過HTTP(S)連線至AEM作者，包括透過&#x200B;**Step 1**&#x200B;取得的承載權杖，使用擴充功能設定JSON中提供的配置(HTTP/HTTPS)、主機和連接埠。
3. AEM Bearer Authentication Handler會從請求中擷取Bearer Token，並針對AdobeIMS進行驗證。
4. 一旦AdobeIMS驗證承載權杖，就會在AEM中建立使用者（如果尚未存在），並從AdobeIMS同步設定檔和群組/成員資料。 AEM使用者會獲得標準AEM登入Token，並在HTTP(S)回應時以Cookie形式傳回至Adobe資產連結擴充功能。
5. 後續互動(即 瀏覽、搜尋、簽入/簽出資產等) 若AdobeAsset Link擴充功能出現HTTP(S)要求，而AEM Author會使用AEM登入Token，使用標準AEM Token Authentication Handler進行驗證。

>[!NOTE]
>
>登入Token到期後， **步驟1-5**&#x200B;將自動叫用、使用承載Token驗證Adobe資產連結擴充功能，並重新發出新的有效登入Token。

## 其他資源{#additional-resources}

* [Adobe資產連結網站](https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html)