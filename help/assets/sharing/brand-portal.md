---
title: 使用品牌入口網站
description: AEM作者與AEM Assets品牌入口網站整合的視訊逐步介紹。
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1777'
ht-degree: 2%

---


# 搭配AEM Assets使用品牌入口網站{#using-brand-portal-with-aem-assets}

Adobe Experience Manager(AEM)Assets Brand Portal整合的視訊指南。

## 品牌入口網站2019年9月功能與增強功能

品牌入口網站於2019年9月推出資產採購，可提高內容速度，並讓Experience Manager作者與第三方創意人員與參與者之間輕鬆快速地交換資產。

### 品牌門戶資產來源補充{#asset-sourcing}

Brand Portal的「資產來源搜尋」可用來收集第三方代理商和團隊的資產，並順暢地將資產同步回Experience Manager作者，以進行審核和使用。

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager作者6.5 SP2(6.5.2)或更新版本才能使用資產來源補充*

請參閱[啟用資產來源補充的Experience Manager作者](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html)以瞭解如何在Experience Manager作者上配置和設定資產來源補充的說明。

## 品牌入口網站2019年2月功能與增強{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

品牌入口網站2019年2月發行的版本著重於文字搜尋和主要客戶要求的增強功能。

### 搜尋增強功能

Brand Portal在篩選窗格中使用屬性謂語的部分文字搜尋功能來增強搜尋。 要允許部分文本搜索，您需要在搜索表單中啟用「屬性謂語中的部分搜索」。

閱讀以進一步瞭解部分文字搜尋和萬用字元搜尋。

#### 部分片語搜尋

您現在可以在篩選窗格中，只指定搜尋過的片語的一或兩個零件，以搜尋資產。

**使用案例** :當您不確定搜尋的片語中發生的字詞的確切組合時，部分片語搜尋會很有幫助。

例如，如果您在Brand Portal中的搜尋表單使用Property Predicate對資產標題進行部分搜尋，則指定詞語camp會傳回所有資產，其標題片語中包含單字camp。

#### 通配符搜索

品牌入口網站允許在搜尋查詢中使用星號(*)以及您搜尋的片語中的部分字詞。

**使用案例** ：如果您不確定搜尋的片語中發生的確切字詞，則可使用萬用字元搜尋來填補搜尋查詢中的空白。

例如，如果Brand Portal中的搜尋表單使用屬性謂詞對資產標題進行部分搜尋，則指定climb*會傳回所有具有字詞的資產，其標題片語中的字詞以字元climb開頭。

同樣地，指定：

* \*climb會傳回所有資產，其標題片語中的字詞結尾會以字元clamp為結尾。
* \*climb\*會傳回所有包含字元sclipt的資產，並在其標題片語中加入字元sclipt。

#### 啟用資料夾階層

管理員現在可以設定資料夾在登入時如何顯示給非管理員使用者（編輯者、檢視者和來賓使用者）。
[「管理工](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 具」面板的「一般設定」中新增了「啟用檔案夾階層設定」。如果配置為：

* 啟用後，從根資料夾開始的資料夾樹對非管理員用戶可見。 因此，可授予他們類似管理員的導覽體驗。
* 停用時，登陸頁面上只會顯示共用資料夾。

[啟用「資](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 料夾階層」功能（啟用後）可協助您區分具有不同階層共用之相同名稱的資料夾。登入時，非管理員使用者現在會看到共用資料夾的虛擬父（和上階）資料夾。

共用資料夾被組織在虛擬資料夾中的各個目錄內。 您可以使用鎖定表徵圖來識別這些虛擬資料夾。

請注意，虛擬資料夾的預設縮略圖是第一個共用資料夾的縮略圖。

### Dynamic Media視訊轉譯支援

AEM Author例項位於Dynamic Media混合模式的使用者，除了原始的視訊檔案外，還可以預覽及下載動態媒體轉譯。

若要允許在特定租用戶帳戶上預覽及下載動態媒體轉譯，管理員需要在「視訊設定」中，從「管理工具」面板指定「Dynamic Media設定」(視訊服務URL(DM-Gateway URL)和註冊ID，以擷取動態視訊)。

Dynamic Media視訊可在以下位置預覽：

* 資產詳細資訊頁面
* 資產的卡片檢視
* 連結共用預覽頁面

Dynamic Media視訊編碼可從以下網址下載：

* 品牌入口網站
* 共用連結

### 排程發佈至品牌入口網站

資產（和資料夾）發佈工作流程(從[AEM(6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)作者例項發佈至品牌入口網站)可排程在稍後的日期、時間。

同樣地，發佈的資產也可以在稍後（時間）從入口網站移除，方法是排程「從品牌入口網站取消發佈」工作流程。

### URL中的可設定租用戶別名

組織可在URL中使用替代首碼，以自訂其入口網站URL。 若要在現有的入口網站URL中取得租用戶名稱的別名，組織必須聯絡Adobe支援。

請注意，只能自訂品牌入口網站URL的首碼，而不能自訂整個URL。
例如，現有網域`wknd.brand-portal.adobe.com`的組織可依要求建立`wkndinc.brand-portal.adobe.com`。

不過，AEM Author例項可以[設定](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)，但只能使用租用戶ID URL，而不能使用租用戶別名（替代）URL。

**使用案例** :組織可自訂入口網站URL，而不是固定Adobe提供的URL，以符合其品牌需求。

## 品牌入口網站2018年12月功能與增強{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### 訪客存取

AEM品牌入口網站可讓訪客存取入口網站。 來賓用戶不需要憑據才能進入門戶，並且可以訪問和下載所有公共資料夾和系列。 Guest使用者可將資產新增至其Light-box（私用系列）並下載相同的資產。 他們也可以檢視管理員設定的智慧型標籤搜尋和搜尋謂詞。 來賓工作階段不允許使用者建立系列和儲存的搜尋，或進一步共用、存取檔案夾和系列設定，以及將資產共用為連結。

### 加速下載

品牌入口網站使用者可運用以Aspera為基礎的快速下載，以快上25倍的速度下載，而且不論其在全球的位置為何，都能享受順暢的下載體驗。 若要從品牌入口網站或共用連結更快速下載資產，使用者必須在下載對話方塊中選取「啟用下載加速」選項，前提是組織已啟用下載加速。

* [加速從品牌入口網站下載的指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### 使用者登入報表

引入了追蹤使用者登入的新報表。 「使用者登入」報表有助於讓組織審核及檢查品牌入口網站的委派管理員和其他使用者。

報表會記錄每個使用者的顯示名稱、電子郵件ID、角色（管理員、檢視器、編輯器、訪客）、群組、上次登入、活動狀態和登入計數。

### 存取原始轉譯

管理員可限制使用者存取原始影像檔案(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-rgb、x-xbitmap、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、psd、image/vnd.adobe.photoshop)，並提供低解析度轉譯本的存取權，供他們從Brand下載入口網站或共用連結。 此存取權可在使用者群組層級從「管理工具」面板的「使用者角色」頁面的「群組」標籤加以控制。

### 新配置

管理員可新增6種新設定，以針對特定租戶啟用／停用下列功能：

* 允許訪客存取
* 允許使用者要求存取品牌入口網站
* 允許管理員從品牌入口網站刪除資產
* 允許建立公開系列
* 允許建立公用智慧型系列
* 允許下載加速

### 其他增強功能

* *卡片和清單檢視上的檔案夾階層路徑* — 可讓使用者知道儲存在品牌入口網站例項中的資料夾位置。幫助用戶區分不同資料夾層次結構中具有相同名稱的資料夾。
* *概述選項* — 選擇資產／資料夾，然後從工具列中選取概述選項，提供非管理員使用者有關資產／資料夾的中繼資料。目前，顯示標題、建立日期和路徑

### Adobe I/O主機UI以設定驗證整合

品牌入口網站使用Adobe I/O[https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)介面來建立JWT應用程式，此應用程式可設定oAuth整合，讓AEM Assets與品牌入口網站整合。 之前，用於設定OAuth整合的UI是裝載在`https://marketing.adobe.com/developer/`中。 若要進一步瞭解如何整合AEM Assets與品牌入口網站，以便將資產和系列發佈至品牌入口網站，請參閱[設定AEM Assets與品牌入口網站的整合](https://helpx.adobe.com/tw/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)。

## 品牌入口網站2018年2月功能與增強{#brand-portal-features-and-enhancements-632}

新功能增強功能，讓品牌入口網站與AEM一致。

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### 導覽改進

* 升級的使用者介面，與AEM並使用Coral3 UI。
* 透過全新的Adobe標誌，快速輕鬆地存取管理工具。
* 透過覆蓋進行產品導覽
* 從子資料夾快速導航到父資料夾。
* Omnisearch選項，以導覽至管理工具和內容。
* 「卡片檢視」、「清單檢視」和「欄檢視」可協助您輕鬆瀏覽巢狀資料夾。
* 「清單檢視」中的資產排序不再限制為螢幕上顯示的資產數。 資料夾中的所有資產都會排序。

### 搜尋改進

* Omnisearch功能可讓您在品牌入口網站中快速搜尋資產和檔案。
* Omnisearch也提供一個選項，可搜尋特定資料夾或位置中的資產
* 自動關鍵字建議，讓搜尋更輕鬆
* 使用其他篩選器來改善您的Omnisearch。 將搜尋結果儲存至智慧型系列的選項，讓您稍後再次造訪搜尋。
* 支援智慧型標籤資產搜尋
* 智AEM慧標籤資產可從共用AEM至品牌入口網站，並在品牌入口網站內使用智慧標籤進行資產搜尋。

### 檔案共用改良

* 使用者可使用連結共用選項來共用資產。
* 共用資產時，使用者可設定每個資產的到期日。 為使用者提供對共用資產的更多控制權。
* 具有資產共用連結的外部使用者可以下載影像並檢視其屬性。
* 已下載的資產檔案夾會保留原始巢狀檔案夾階層。

### 報告和管理功能

* 來自AEM Assets的中繼資料架構現在可從發佈AEM至品牌入口網站。
* 管理員可以建立和管理三種報表類型：資產下載、過期和發佈
* 能夠設定需要包含在報表中的欄。
* 建立品牌入口網站內資產的影像預設集。
* 能夠修改管理搜尋邊欄表單或搜尋Forms，以包含其他篩選選項。
* 更新並預覽您品牌的自訂壁紙
* 使用狀況報告，以瞭解使用者人數、已使用的儲存空間和資產總計。

## 其他資源{#additional-resources}

* [品牌入口網站的新增功能](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM Author複製代理](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [加速下載指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets品牌入口網站Adobe檔案](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM AssetsDynamic MediaAdobe文檔](https://docs.adobe.com/docs/en/aem/6-3/author/assets/dynamic-media.html)
* [下載Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)