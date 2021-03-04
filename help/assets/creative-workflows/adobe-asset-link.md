---
title: 將Adobe資產連結擴展與AEM Assets
description: 'Adobe Experience Manager資產現在可供設計人員和創意使用者在其喜愛的Adobe Creative Cloud案頭應用程式中使用。 Adobe Creative Cloud企業的Adobe資產連結擴充功能可擴充搜尋和瀏覽、排序、預覽、上傳資產、結帳、修改、登入和檢視Creative Cloud工具(例如Adobe Photoshop、InDesign和Illustrator)中AEM資產的中繼資料。 '
feature: Adobe資產連結
version: 6.4, 6.5
topic: 內容管理
role: 業務從業人員
level: 初學者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1102'
ht-degree: 1%

---


# 將Adobe資產連結擴展與AEM Assets一起使用{#using-adobe-asset-link-extension-with-aem-assets}

Adobe Experience Manager資產現在可供設計人員和創意使用者在其喜愛的Adobe Creative Cloud案頭應用程式中使用。 Adobe Creative Cloud企業的Adobe資產連結擴充功能可擴充搜尋和瀏覽、排序、預覽、上傳資產、結帳、修改、登入和檢視Creative Cloud工具(例如Adobe Photoshop、InDesign和Illustrator)中AEM資產的中繼資料。


## Adobe資產連結1.1

Adobe資產連結1.1版現在提供Adobe資產連結與AEM Assets之間的InDesign直接連結支援。 有了InDesign直接連結支援，您現在可以透過InDesign資產連結面板，將數位資產從AEM Assets置入（置入連結或置入復本）或從Adobe拖放至。 此外，還介紹「僅用於放置」(*For Placement Only)*(FPO)轉譯。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>僅使用您的Adobe Creative CloudEnterprise ID或Federated ID。 請確定[配置AEMAdobe資產連結](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)。


### Adobe資產連結功能

* Adobe資產連結是可在PS、AI、ID中運作的擴充功能，可讓您直接存取位於AEM Assets的數位資產
* 創作者將會使用其AEMAdobeIMSEnterprise ID或Federated ID自動登入
* 創意人員除了搜尋AEM Assets和Creative Cloud資產外，還可瀏覽居住在AEM Assets的數位資產
* 創意人員可存取位於AEM Assets的資產的檔案詳細資訊；縮圖、基本中繼資料和版本
* 創意人員可將資產置入、下載或拖放至版面
* 創意人員可以修改資產，方法是從AEM Assets簽出資產，並在其「Creative Cloud資產」帳戶中處理資產(WIP)
* 創意人員可在修改完資產後，將資產存回AEM Assets，而新版本將反映在AEM Assets
* 支援Creative Cloud2020、2019和2018InDesign、Photoshop和Illustrator案頭應用程式
* 使用者可以從「Adobe資產連結應用程式內」面板執行資產搜尋，並依大小、字母順序和相關性排序資產
* 使用者可直接從「資產連結」面板存取及瀏覽AEM Assets系列和智慧型系列
* 直接從面板將新建的資產新增至AEM Assets
* 使用者可直接將資產拖放至InDesign影格

### 讓AEM Assets成為InDesign

您可以使用下列其中一個選項，將資產置入InDesign版面：

* **置入復本** -將資產內嵌（使用「置入復本」選項），會在將二進位檔下載至您的本機系統後，將原始資產的復本放入InDesign版面中。Adobe資產連結不會維護內嵌副本與原始資產之間的任何連結。 如果原始資產在AEM Assets修改，您必須從InDesign檔案中刪除內嵌資產，並從AEM Assets重新內嵌資產。

* **置入連結** -使用InDesign文檔時，現在除了直接嵌入資產（使用上下文菜單中的「置入副本」選項）外，您還可以選擇引用AEM Assets的資產。參考資產可讓您與其他使用者共同作業，並納入對AEM Assets原始資產所做的任何更新。 若要參照來自AEM Assets的資產，請使用內容功能表中的「置入連結」選項。

### 僅限位置(FPO)解析度

當使用InDesign資產連結從AEM Assets將大型資產檔案放入Adobe檔案時，創作人員使用者在開始放置作業後需要等待幾秒鐘。 這會影響整體使用者體驗。 使用Adobe資產連結，您現在可以暫時放置來自AEM Assets的原始資產的低解析度影像，以縮短放置影像的時間。 同時，它也可提升整體使用體驗和生產力。 較低解析度的影像會暫時放置，當列印或發佈需要最終輸出時，您必須將FPO轉譯取代為原稿。 如果您想要將多個FPO影像取代為個別的原始影像，請導覽至&#x200B;**_Windows >連結_**&#x200B;面板，然後下載原始資產。 下載原始影像後，選擇「將所有FPO取代為原始影像」。

>[!NOTE]
>
> *「僅限置入(FPO)」轉譯* 僅適用於「置入連結」選項。您也應在「AEM Assets *更新資產*」工作流程中啟用FPO轉譯支援。

FPO轉譯是原始資產的輕量型替代。 它們的長寬比相同，但與原始影像相比尺寸較小。 目前，InDesign僅支援匯入下列影像類型的FPO轉譯：

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

如果FPO轉譯不適用於AEM Assets的特定資產，則會改為參考原始的高解析度資產。 對於FPO影像，狀態FPO顯示在「InDesign連結」面板中。

## 瞭解Adobe資產連結驗證與AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Adobe資產連結驗證如何在AdobeIdentity Management服務(IMS)和Adobe Experience Manager作者的背景下運作。

![Adobe資產連結架構](assets/adobe-asset-link-article-understand.png)

下載[Adobe資產連結體系結構](assets/adobe-asset-link-article-understand-1.png)

1. Adobe資產連結擴充功能透過Adobe Creative Cloud案頭應用程式提出授權要求，要求Adobe身分管理服務(IMS)，並在成功時接收不記名代號。
2. Adobe資產連結擴充功能透過HTTP(S)連線至AEM作者，包括使用擴充功能設定JSON中提供的配置(HTTP/HTTPS)、主機和連接埠在&#x200B;**步驟1**&#x200B;中取得的承載Token。
3. 持AEM有者驗證處理常式從請求擷取持有者Token，並針對AdobeIMS進行驗證。
4. 一旦AdobeIMS驗證承載Token後，就會在AEM（如果尚不存在）中建立使用者，並同步AdobeIMS的設定檔和群組／會籍資料。 使AEM用者獲得標準登AEM入Token，並在HTTP(S)回應中以Cookie形式傳回至Adobe資產連結副檔名。
5. 後續互動(即 瀏覽、搜尋、登入／登出資產等) Adobe資產連結副檔名會產生HTTP(S)要求給AEM Author，這些要求會使用登入Token來驗證，並使AEM用標準Token驗證處AEM理常式。

>[!NOTE]
>
>在登入Token到期後，**步驟1-5**&#x200B;將自動呼叫，使用Bearer Token驗證Adobe資產連結擴充功能，並重新發出新的有效登入Token。

## 其他資源{#additional-resources}

* [Adobe資產連結網站](https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html)