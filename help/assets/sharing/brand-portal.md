---
title: 使用Brand Portal
description: AEM作者與AEM Assets Brand Portal整合的影片逐步解說。
feature: Brand Portal
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2460
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1702'
ht-degree: 0%

---

# 搭配AEM Assets使用Brand Portal{#using-brand-portal-with-aem-assets}

Adobe Experience Manager (AEM) Assets Brand Portal整合的影片指南。

## Brand Portal 2019年9月功能和增強功能

Brand Portal 2019年9月最值得一提的是推出了Asset Sourcing，除了可提升內容速度，也允許在Experience Manager作者與協力廠商創意和貢獻者之間輕鬆快速地交換資產。

### Brand Portal資產來源{#asset-sourcing}

Brand Portal的Asset Sourcing用於從第三方機構和團隊收集資產，無縫地將其同步回Experience Manager Author以供審閱和使用。

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*需要Experience Manager Author 6.5 SP2 (6.5.2)或更新版本才能使用Asset Sourcing*

檢閱[啟用Experience Manager Author for Asset Sourcing](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=zh-Hant)，瞭解如何在Experience Manager Author上設定和設定Asset Sourcing。

## Brand Portal 2019年2月功能和增強功能{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

Brand Portal 2019年2月版本的重點在於文字搜尋和主要客戶請求的增強功能。

### 搜尋增強功能

Brand Portal透過篩選窗格中屬性述詞的部分文字搜尋來增強搜尋。 若要允許部分文字搜尋，您需要在搜尋表單的屬性述詞中啟用部分搜尋。

請閱讀下文，深入瞭解部分文字搜尋和萬用字元搜尋。

#### 部分片語搜尋

您現在可以在篩選窗格中僅指定搜尋片語的一部分（即一兩個字）來搜尋資產。

**使用案例** ：當您不確定搜尋到的片語中所出現的確切字片語合時，部分片語搜尋會很有幫助。

例如，如果您在Brand Portal中的搜尋表單使用「屬性述詞」來部分搜尋資產標題，則指定字詞camp會傳回標題片語中包含camp字詞的所有資產。

#### 萬用字元搜尋

Brand Portal允許在搜尋查詢中使用星號(*)，連同搜尋片語中的部分單字。

**使用案例** ：如果您不確定搜尋片語中是否出現確切的字詞，可以使用萬用字元搜尋來填補搜尋查詢中的空白。

例如，如果Brand Portal中的搜尋表單使用屬性述詞來搜尋資產標題，指定climb*會傳回標題片語中字詞開頭為climb的所有資產。

同樣地，指定：

* \*climb會傳回標題片語中字詞結尾為climb的所有資產。
* \*climb\*會傳回所有包含單字的資產，這些單字的標題片語中會有climb。

#### 啟用資料夾階層

管理員現在可以設定在登入時向非管理員使用者（編輯者、檢視者和訪客使用者）顯示資料夾的方式。
已在[管理工具]面板的[一般設定]中新增[啟用資料夾階層](https://helpx.adobe.com/tw/experience-manager/brand-portal/using/brand-portal-general-configuration.html)設定。 如果設定為：

* 啟用後，非管理員使用者可以看到從根資料夾開始的資料夾樹狀結構。 因此，授予他們類似於管理員的導覽體驗。
* 已停用，登入頁面上只會顯示共用資料夾。

[啟用資料夾階層](https://helpx.adobe.com/tw/experience-manager/brand-portal/using/brand-portal-general-configuration.html)功能（啟用時）可協助您區分名稱相同但共用不同階層的資料夾。 登入時，非管理員使用者現在可以看到共用資料夾的虛擬父項（和祖項）資料夾。

共用資料夾會在虛擬資料夾的個別目錄中組織。 您可以使用鎖定圖示來辨識這些虛擬資料夾。

請注意，虛擬資料夾的預設縮圖是第一個共用資料夾的縮圖影像。

### Dynamic Media影片轉譯支援

AEM製作例項位於Dynamic Media混合模式的使用者，除了原始視訊檔案外，也可以預覽和下載Dynamic Media轉譯。

若要允許預覽和下載特定租使用者帳戶上的動態媒體轉譯，管理員需要從管理工具面板的視訊設定中指定Dynamic Media設定(視訊服務URL （DM閘道URL）和註冊ID以擷取動態視訊)。

您可在以下位置預覽Dynamic Media影片：

* 資產詳細資訊頁面
* 資產的卡片檢視
* 連結共用預覽頁面

Dynamic Media視訊編碼可從以下來源下載：

* Brand Portal
* 已分享的連結

### 已排程發佈至Brand Portal

從[AEM (6.4.2.0)](https://helpx.adobe.com/tw/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)製作執行個體到Brand Portal的Assets （和資料夾）發佈工作流程可以排程在之後的日期、時間。

同樣地，已發佈的資產可在稍後的日期（時間）從入口網站移除，方法是排程從Brand Portal取消發佈工作流程。

### URL中可設定的租使用者別名

組織可以在URL中使用替代首碼，自訂入口網站URL。 若要在現有入口網站URL中取得租使用者名稱稱的別名，組織需要聯絡Adobe支援。

請注意，您只能自訂Brand Portal URL的前置詞，而不能自訂整個URL。
例如，具有現有網域`wknd.brand-portal.adobe.com`的組織可以取得根據請求建立的`wkndinc.brand-portal.adobe.com`。

不過，AEM作者執行個體只能使用租使用者ID URL設定[設定](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)，而不能使用租使用者別名（替代） URL。

**使用案例** ：組織可以自訂入口網站URL，而不停留在Adobe提供的URL，藉此滿足品牌需求。

## Brand Portal 2018年12月功能和增強功能{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### 訪客存取

AEM Brand Portal允許訪客存取入口網站。 訪客使用者不需要認證即可進入入口網站，且可以存取及下載所有公用資料夾和集合。 訪客使用者可將資產新增至其Light-Box （私人集合）並下載相同專案。 他們也可以檢視智慧標籤搜尋和搜尋管理員設定的述詞。 訪客工作階段不允許使用者建立收藏集和已儲存的搜尋，或是進一步共用它們、存取資料夾和收藏集設定，以及將資產以連結形式共用。

### 加速下載

Brand Portal使用者可運用Aspera架構的快速下載，以快上25倍的速度進行下載，不論身在何處，都能享有順暢的下載體驗。 若要從Brand Portal或共用連結更快速地下載資產，使用者必須在下載對話方塊中選取「啟用加速下載」選項，前提是他們的組織已啟用加速下載。

* [從Brand Portal加速下載的指南](https://helpx.adobe.com/tw/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera連線測試伺服器](https://test-connect.asperasoft.com/)

### 使用者登入報表

已引入追蹤使用者登入的新報表。 「使用者登入」報表可協助組織稽核並檢視Brand Portal的委派管理員和其他使用者。

報告記錄會顯示每個使用者的名稱、電子郵件ID、角色（管理員、檢視者、編輯者、來賓）、群組、上次登入、活動狀態和登入計數。

### 存取原始轉譯

管理員可限制使用者存取原始影像檔案(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-pixmap、x-rgb、x-xbitmap、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、psd、image/vnd.adobe.photoshop)，並可讓使用者存取他們從Brand Portal或共用連結下載的低解析度轉譯。 您可以在管理工具面板中，從「使用者角色」頁面的「群組」索引標籤中，於使用者群組層級控制此存取權。

### 新設定

管理員新增了六個設定，用以啟用/停用特定租使用者的下列功能：

* 允許訪客存取
* 允許使用者要求存取Brand Portal
* 允許管理員從Brand Portal刪除資產
* 允許建立公開集合
* 允許建立公開的智慧型集合
* 允許加速下載

### 其他增強功能

* *卡片和清單檢視上的資料夾階層路徑* — 可讓使用者知道儲存在Brand Portal執行個體中的資料夾位置。 協助使用者區分不同資料夾階層內具有相同名稱的資料夾。
* *總覽選項* — 選取資產/資料夾，然後從工具列選取總覽選項，即可提供有關資產/資料夾的非管理員使用者中繼資料。 目前，顯示標題、建立日期和路徑

### Adobe I/O代管UI以設定oAuth整合

Brand Portal使用Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)介面建立JWT應用程式，如此可設定oAuth整合，以允許AEM Assets與Brand Portal整合。 之前，用於設定OAuth整合的UI是在`https://marketing.adobe.com/developer/`中託管。 若要進一步瞭解如何整合AEM Assets與Brand Portal，以將資產和集合發佈至Brand Portal，請參閱[設定AEM Assets與Brand Portal的整合](https://helpx.adobe.com/tw/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)。

## Brand Portal 2018年2月功能和增強功能{#brand-portal-features-and-enhancements-632}

新功能強化了旨在將Brand Portal與AEM對齊的功能。

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### 改善導覽

* 升級的使用者介面，此介面與AEM一致，並使用Coral3 UI。
* 透過新的Adobe標誌，輕鬆快速地存取管理工具。
* 覆蓋圖中的產品導覽
* 從子資料夾快速瀏覽至父資料夾。
* 用於導覽至管理工具和內容的Omnisearch選項。
* 「卡片檢視」、「清單」檢視和「欄檢視」可協助您輕鬆瀏覽巢狀資料夾。
* 清單檢視中的資產排序不再受限於畫面上顯示的資產數量。 資料夾中的所有資產都會排序。

### 搜尋改進

* Omnisearch功能可讓您在Brand Portal中快速搜尋資產和檔案。
* Omnisearch也提供可在特定資料夾或位置中搜尋資產的選項
* 自動關鍵字建議，讓搜尋更輕鬆
* 使用其他篩選器改善Omnisearch。 將搜尋結果儲存至Smart Collection以供您稍後重新造訪搜尋的選項。
* 支援智慧標籤資產搜尋
* AEM智慧標籤資產可以從AEM共用至Brand Portal，並在Brand Portal中使用智慧標籤進行資產搜尋。

### 檔案共用改善

* 使用者可使用連結分享選項分享資產。
* 共用資產時，使用者需設定每個資產的到期日。 讓使用者更能掌控共用的資產。
* 具有資產共用連結的外部使用者可以下載影像並檢視其屬性。
* 系統會為下載的資產資料夾保留原始的巢狀資料夾階層。

### 報告與管理功能

* AEM Assets的中繼資料結構現在可從AEM發佈至Brand Portal。
* 管理員可以建立和管理三種型別的報表：已下載、已過期和已發佈的資產
* 可設定需要納入報表的欄。
* 在Brand Portal中建立資產的影像預設集。
* 可修改管理員搜尋邊欄表單或搜尋Forms ，以包含其他篩選選項。
* 更新並預覽您品牌的自訂桌布
* 使用量報表，用於瞭解使用者人數、已使用的儲存空間及資產總數。

## 其他資源{#additional-resources}

* [Brand Portal的新功能](https://helpx.adobe.com/tw/experience-manager/brand-portal/using/whats-new.html)
* [AEM作者復寫代理程式](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [加速下載指南](https://helpx.adobe.com/tw/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand Portal Adobe檔案](https://helpx.adobe.com/tw/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic Media Adobe檔案](https://experienceleague.adobe.com/docs/?lang=zh-Hant)
* [下載Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera連線測試伺服器](https://test-connect.asperasoft.com/)
