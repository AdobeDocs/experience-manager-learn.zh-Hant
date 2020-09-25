---
title: 搭配AEM Assets使用Adobe Asset Link Extension
description: '設計人員和創意使用者現在可以在他們最愛的Adobe Creative Cloud案頭應用程式中使用Adobe Experience Manager資產。 適用於Adobe Creative Cloud Enterprise的Adobe Asset Link擴充功能可讓您在Creative Cloud工具（例如Adobe Photoshop、InDesign和Illustrator）中搜尋和瀏覽、排序、預覽、上傳資產、結帳、修改、登入和檢視AEM資產的中繼資料。 '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 1%

---


# 搭配AEM Assets使用Adobe Asset Link Extension{#using-adobe-asset-link-extension-with-aem-assets}

設計人員和創意使用者現在可以在他們最愛的Adobe Creative Cloud案頭應用程式中使用Adobe Experience Manager資產。 適用於Adobe Creative Cloud Enterprise的Adobe Asset Link擴充功能可讓您在Creative Cloud工具（例如Adobe Photoshop、InDesign和Illustrator）中搜尋和瀏覽、排序、預覽、上傳資產、結帳、修改、登入和檢視AEM資產的中繼資料。


## Adobe Asset Link 1.1

Adobe Asset Link v1.1現在提供Adobe Asset Link和AEM Assets之間的InDesign直接連結支援。 有了InDesign直接連結支援，您現在可以透過Adobe Asset Link面板，將數位資產從AEM Assets置入（置入連結或置入復本）或拖放至InDesign。 此外，還引入「 *僅供放置* (FPO)」轉譯。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>請僅使用您的Adobe Creative Cloud Enterprise ID或Federated ID。 請確定您 [已設定Adobe Asset Link的AEM](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html)。


### Adobe Asset Link功能

* Adobe Asset Link是可在PS、AI、ID中運作的擴充功能，可讓您直接存取位於AEM Assets中的數位資產
* 創作人員將會使用其Adobe IMS Enterprise ID或Federated ID自動登入AEM
* 創意人員除了可以瀏覽AEM Assets和Creative Cloud資產的搜尋外，還可以瀏覽AEM Assets中的數位資產
* 創意人員可存取駐留在AEM Assets中之資產的檔案詳細資訊；縮圖、基本中繼資料和版本
* 創意人員可將資產置入、下載或拖放至版面
* 創作人員可以從AEM Assets中取出資產，並在其Creative Cloud資產帳戶中處理(WIP)，以修改資產
* 創作人員可在完成修改資產後，將資產簽回AEM Assets，而新版本將會反映在AEM Assets中
* 支援Creative Cloud 2020、2019和2018 InDesign、Photoshop和Illustrator案頭應用程式
* 使用者可以從「Adobe Asset Link應用程式內」面板執行資產搜尋，並依大小、字母順序和相關性來排序
* 使用者可直接從「資產連結」面板存取及瀏覽AEM Assets系列和智慧型系列
* 直接從面板將新建的資產新增至AEM Assets
* 使用者可直接將資產拖放至InDesign影格

### 將AEM資產放入InDesign

您可以使用下列其中一個選項，將資產置入InDesign版面：

* **置入復本** -內嵌資產（使用「置入復本」選項）會在將二進位檔下載至您的本機系統後，將原始資產的復本放入您的InDesign版面。 Adobe Asset Link不會維護內嵌副本與原始資產之間的任何連結。 如果原始資產已在AEM Assets中修改，您必須從InDesign檔案中刪除內嵌資產，並從AEM Assets重新內嵌資產。

* **置入連結** -使用InDesign檔案時，您現在除了直接內嵌資產（使用內容選單中的「置入複製」選項）外，還可以選擇參考AEM Assets中的資產。 參照資產可讓您與其他使用者共同作業，並將對原始資產所做的任何更新併入AEM Assets中。 若要從AEM Assets參考資產，請使用內容選單中的「置入連結」選項。

### 僅限位置(FPO)解析度

當使用Adobe Asset Link從AEM Assets將大型資產檔案置入InDesign檔案時，創意使用者在開始置入作業後需要等上幾秒鐘。 這會影響整體使用者體驗。 有了Adobe Asset Link，您現在可以暫時從AEM Assets置入原始資產的低解析度影像，進而縮短置入影像的時間。 同時，它也可提升整體使用體驗和生產力。 較低解析度的影像會暫時放置，當列印或發佈需要最終輸出時，您必須將FPO轉譯取代為原稿。 如果您想要將多個FPO影像取代為個別的原始影像，請導覽至 **_Windows >連結面板_** ，然後下載原始資產。 下載原始影像後，選擇「將所有FPO取代為原始影像」。

>[!NOTE]
>
> *「僅限置入(FPO)」轉譯* ，僅適用於「置入連結」選項。 您也應該在AEM Assets *Dam Update Asset工作流程中啟用FPO轉譯支援* 。

FPO轉譯是原始資產的輕量型替代。 它們的長寬比相同，但與原始影像相比尺寸較小。 目前，InDesign僅支援匯入下列影像類型的FPO轉譯：

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

如果FPO轉譯無法用於AEM Assets中的特定資產，則會改為參考原始的高解析度資產。 對於FPO影像，狀態FPO會顯示在「InDesign連結」面板中。



## 瞭解Adobe Asset Link與AEM Assets的驗證{#understanding-adobe-asset-link-authentication-with-aem-assets}

Adobe Asset Link驗證如何在Adobe Identity Management Services(IMS)和Adobe Experience Manager Author的環境中運作。

![Adobe Asset Link架構](assets/adobe-asset-link-article-understand.png)

下載 [Adobe Asset Link架構](assets/adobe-asset-link-article-understand-1.png)

1. Adobe Asset Link擴充功能會透過Adobe Creative Cloud案頭應用程式向Adobe Identity Manage Service(IMS)提出授權要求，並在成功時收到不記名代號。
2. Adobe Asset Link擴充功能會透過HTTP(S)連線至AEM Author，包括使用擴充功能設定JSON中提供的配置(HTTP/HTTPS **)、主機和連接埠，在** Step 1中取得的Bearer Token。
3. AEM的「承載者驗證處理常式」會從請求中擷取「承載者」Token，並針對Adobe IMS進行驗證。
4. 一旦Adobe IMS驗證Bearer Token後，就會在AEM中建立使用者（如果尚不存在），並同步來自Adobe IMS的設定檔和群組／會籍資料。 AEM使用者會獲得標準AEM登入Token，此Token會在HTTP(S)回應中以Cookie形式傳回至Adobe Asset Link擴充功能。
5. 後續互動(即 瀏覽、搜尋、登入／登出資產等) 使用Adobe Asset Link擴充功能會產生HTTP(S)要求給AEM Author，這些要求會使用AEM登入Token，使用標準AEM Token驗證處理常式進行驗證。

>[!NOTE]
>
>登入Token到期後，步 **驟1-5** （步驟1-5）將自動呼叫、使用Bearer Token驗證Adobe Asset Link擴充功能，並重新發出新的有效登入Token。

## 其他資源{#additional-resources}

* [Adobe Asset Link網站](https://www.adobe.com/tw/creativecloud/business/enterprise/adobe-asset-link.html)