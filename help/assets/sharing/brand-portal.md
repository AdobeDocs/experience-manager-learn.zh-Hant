---
title: 使用Brand Portal
description: AEM作者和AEM Assets Brand Portal整合的影片教學。
feature: 品牌入口網站
version: 6.3, 6.4, 6.5
topic: 內容管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 2%

---


# 將Brand Portal與AEM Assets搭配使用{#using-brand-portal-with-aem-assets}

Adobe Experience Manager(AEM)Assets Brand Portal整合的影片指南。

## Brand Portal 2019年9月功能與增強功能

Brand Portal 2019年9月推出的Asset Sourcing最為引人注目，可提升內容速度，並可讓Experience Manager作者與第三方創意人員及貢獻者輕鬆快速地交換資產。

### Brand Portal Asset Sourcing{#asset-sourcing}

Brand Portal的Asset Sourcing可用來收集來自協力廠商和團隊的資產，順暢地將資產同步回Experience Manager作者，以供審核及使用。

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager作者6.5 SP2(6.5.2)或更新版本才能使用Asset Sourcing*

請參閱[為Asset Sourcing啟用Experience Manager作者](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html) ，了解如何在Experience Manager作者上設定Asset Sourcing的指示。

## Brand Portal 2019年2月功能與增強功能{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portal 2019年2月發行版本著重於文字搜尋和主要客戶請求的增強功能。

### 搜尋增強功能

Brand Portal增強了篩選窗格中針對屬性述詞的搜尋功能，並提供部分文字搜尋。 若要允許部分文字搜尋，您必須在搜尋表單的「屬性述詞」中啟用「部分搜尋」。

請閱讀以深入了解部分文字搜尋和萬用字元搜尋。

#### 部分片語搜尋

您現在可以在篩選窗格中，僅指定所搜尋片語的一個部分（即一兩個字），以搜尋資產。

**使用案例** :如果您不確定搜尋的片語中出現的字詞的確切組合，部分片語搜尋將有所幫助。

例如，如果您在Brand Portal中的搜尋表單使用「屬性述詞」來進行資產標題的部分搜尋，則指定詞語camp會傳回標題片語中包含camp字詞的所有資產。

#### 通配符搜索

Brand Portal允許在搜尋查詢中使用星號(*)，以及搜尋片語中單字的一部分。

**使用案例** ：如果您不確定搜尋的片語中出現的確切字詞，可使用萬用字元搜尋來填滿搜尋查詢中的間隙。

例如，如果Brand Portal中的搜尋表單使用「屬性述詞」對資產標題進行部分搜尋，指定climb*會傳回標題片語中所有字詞開頭為climb字元的資產。

同樣地，指定：

* \*climb會傳回標題片語中字元會翻的所有資產。
* \*climb\*會傳回所有包含字元在標題片語中攀爬的資產。

#### 啟用資料夾階層

管理員現在可以設定資料夾在登入時如何顯示給非管理員使用者（編輯者、檢視者和訪客使用者）。
[「啟用資](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 料夾階層配置」會新增至「一般設定」的「管理工具」面板。如果設定為：

* 啟用後，非管理員使用者可看到從根資料夾開始的資料夾樹狀結構。 因此，會授予他們類似管理員的導覽體驗。
* 停用時，登錄頁面上只會顯示共用資料夾。

[啟用資](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 料夾層次結構功能（啟用後）有助於區分具有從不同層次結構共用的相同名稱的資料夾。現在，非管理員使用者登入時可以看到共用資料夾的虛擬上層（和上階）資料夾。

共用資料夾在虛擬資料夾中的各目錄內組織。 您可以使用鎖表徵圖識別這些虛擬資料夾。

請注意，虛擬資料夾的預設縮圖是第一個共用資料夾的縮圖影像。

### Dynamic Media影片轉譯支援

AEM製作執行個體位於Dynamic Media混合模式的使用者，除了原始視訊檔案，還可以預覽和下載動態媒體轉譯。

若要允許在特定租用戶帳戶上預覽和下載動態媒體轉譯，管理員需要從管理工具面板在視訊設定中指定Dynamic Media設定(視訊服務URL（DM — 閘道URL）和註冊ID，以擷取動態視訊)。

Dynamic Media影片可在下列位置預覽：

* 資產詳細資訊頁面
* 資產的卡片檢視
* 連結共用預覽頁面

Dynamic Media視訊編碼可從以下位置下載：

* 品牌入口網站
* 共用連結

### 排程發佈至Brand Portal

可從[AEM(6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)製作例項發佈至Brand Portal的資產（和資料夾）工作流程可排程在之後的日期、時間內執行。

同樣地，您也可以排程從Brand Portal中取消發佈工作流程，在稍後（時間）從入口網站移除已發佈的資產。

### URL中可設定的租用戶別名

組織可在URL中加上替代首碼，以取得自訂的入口網站URL。 若要在現有入口URL中取得租用戶名稱的別名，組織需要聯絡Adobe支援。

請注意，只能自訂Brand Portal URL的首碼，不能自訂整個URL。
例如，現有網域`wknd.brand-portal.adobe.com`的組織可依請求建立`wkndinc.brand-portal.adobe.com`。

不過，AEM Author例項只能使用租用戶ID URL來設定[](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)，而不能使用租用戶別名（替代）URL。

**使用案例** :組織可透過自訂入口網站URL來滿足其品牌需求，而不需堅持由Adobe提供的URL。

## Brand Portal 2018年12月功能與增強功能{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### 訪客存取

AEM Brand Portal可讓訪客存取入口網站。 來賓用戶不需要憑據進入門戶，並且可以訪問和下載所有公共資料夾和集合。 訪客使用者可將資產新增至其簡易方塊（私人系列）並下載相同的資產。 他們也可以檢視智慧標籤搜尋和搜尋管理員設定的述詞。 來賓工作階段不允許使用者建立集合和儲存的搜尋，或進一步共用，存取資料夾和集合設定，以及以連結形式共用資產。

### 加速下載

Brand Portal使用者可善用Aspera的快速下載功能，將速度提升至25倍，無論其在全球的位置為何，都能享有順暢的下載體驗。 若要從Brand Portal或共用連結更快下載資產，使用者必須在下載對話方塊中選取「啟用下載加速」選項，前提是組織已啟用下載加速功能。

* [從Brand Portal加速下載的指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect測試伺服器](https://test-connect.asperasoft.com/)

### 使用者登入報表

推出新報表以追蹤使用者登入。 「使用者登入」報表有助於讓組織稽核及檢查委派的管理員和其他Brand Portal使用者。

報表會記錄每個使用者的顯示名稱、電子郵件ID、角色（管理員、檢視器、編輯器、訪客）、群組、上次登入、活動狀態和登入計數。

### 存取原始轉譯

管理員可限制使用者存取原始影像檔案(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-portable-pixmap、x-rgb、x-xbitmap、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、psd、image/vnd.adobe.photoshop)，並允許使用者存取從Brand Portal或共用連結下載的低解析度轉譯。 您可以在使用者群組層級，從管理工具面板的使用者角色頁面的群組標籤控制此存取。

### 新配置

新增6項新設定供管理員啟用/停用特定租戶的下列功能：

* 允許訪客存取
* 允許使用者要求存取Brand Portal
* 允許管理員從Brand Portal刪除資產
* 允許建立公用集合
* 允許建立公用智慧型集合
* 允許下載加速

### 其他增強功能

* *卡片和清單檢視上的資料夾階層路徑*  — 可讓使用者知道儲存在Brand Portal例項中的資料夾位置。幫助用戶區分不同資料夾層次結構中具有相同名稱的資料夾。
* *概覽選項*  — 通過選擇資產/資料夾，然後從工具欄中選擇概覽選項，提供有關資產/資料夾的非管理員用戶元資料。目前，顯示標題、建立日期和路徑

### Adobe I/O主機UI以設定oAuth整合

Brand Portal使用Adobe I/O[https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)介面來建立JWT應用程式，這可啟用設定oAuth整合以允許AEM Assets與Brand Portal整合。 之前，用於設定OAuth整合的UI托管於`https://marketing.adobe.com/developer/`。 若要深入了解如何整合AEM Assets與Brand Portal，以便將資產和集合發佈至Brand Portal，請參閱[設定AEM Assets與Brand Portal的整合](https://helpx.adobe.com/tw/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)。

## Brand Portal 2018年2月功能與增強功能{#brand-portal-features-and-enhancements-632}

新功能增強的功能，主要針對將Brand Portal與AEM協調。

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### 導覽改善

* 符合AEM且使用Coral3 UI的升級使用者介面。
* 透過新的Adobe標誌，快速輕鬆存取管理工具。
* 透過覆蓋圖導覽產品
* 從子資料夾快速導覽至父資料夾。
* 導覽至管理工具和內容的Omnisearch選項。
* 卡片檢視、清單檢視和欄檢視可協助您輕鬆瀏覽巢狀資料夾。
* 清單檢視中的資產排序不再受限於畫面上顯示的資產數量。 資料夾中的所有資產都會排序。

### 搜尋改善

* Omnisearch功能可讓您快速搜尋Brand Portal內的資產和檔案。
* Omnisearch也提供在特定資料夾或位置中搜尋資產的選項
* 自動關鍵字建議可讓搜尋更輕鬆
* 使用其他篩選器改善您的Omnisearch。 將搜尋結果儲存至智慧型集合的選項，供您稍後重新造訪搜尋。
* 支援智慧標籤資產搜尋
* AEM智慧標籤資產可從AEM共用至Brand Portal，並在Brand Portal內使用智慧標籤進行資產搜尋。

### 檔案共用改善

* 使用者可以使用連結共用選項來共用資產。
* 共用資產時，使用者會設定每個資產的到期日。 讓使用者對共用的資產有更多控制權。
* 具有資產共用連結的外部使用者可以下載影像並檢視其屬性。
* 系統會為下載的資產資料夾保留原始的巢狀資料夾階層。

### 報告和管理功能

* 來自AEM Assets的中繼資料結構現在可從AEM發佈至Brand Portal。
* 管理員可以建立和管理三種報表類型：下載、過期和發佈的資產
* 可設定需要納入報表的欄。
* 在Brand Portal中建立資產的影像預設集。
* 可修改「管理員搜尋邊欄表單」或「搜尋Forms」以包含其他篩選選項。
* 更新並預覽您品牌的自訂壁紙
* 使用狀況報表，了解使用者人數、已使用的儲存空間和資產總計。

## 其他資源{#additional-resources}

* [Brand Portal的新增功能](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM製作復寫代理](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [加速下載指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand PortalAdobe檔案](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic MediaAdobe檔案](https://docs.adobe.com/docs/en/aem/6-3/author/assets/dynamic-media.html)
* [下載Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect測試伺服器](https://test-connect.asperasoft.com/)