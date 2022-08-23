---
title: 使用Brand Portal
description: AEM作者和AEM Assets Brand Portal整合的視頻瀏覽。
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '1764'
ht-degree: 2%

---

# 將Brand Portal與AEM Assets{#using-brand-portal-with-aem-assets}

Adobe Experience Manager(AEMAssets Brand Portal)整合視頻指南

## Brand Portal2019年9月功能和增強

Brand Portal在2019年9月最引人注目地推出了「資產採購」，它提高了內容速度，允許Experience Manager作者與第三方創意人和貢獻者之間輕鬆、快速地交換資產。

### Brand Portal資產來源{#asset-sourcing}

Brand Portal的資產來源補充用於從第三方機構和團隊收集資產，並無縫地將這些資產同步回Experience Manager作者以供審閱和使用。

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager作者6.5 SP2(6.5.2)或更高版本才能使用資產來源補充*

審閱 [啟用資產來源補充的Experience Manager作者](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=zh-Hant) 有關如何配置和設定Experience Manager作者上的資產來源補充的說明。

## Brand Portal2019年2月功能和增強{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portal2019年2月發佈的版本側重於對文本搜索和頂級客戶請求的增強。

### 搜索增強

Brand Portal在篩選窗格中對屬性謂語進行部分文本搜索，以增強搜索。 要允許部分文本搜索，您需要在搜索表單中啟用屬性謂語中的部分搜索。

閱讀以瞭解有關部分文本搜索和通配符搜索的詳細資訊。

#### 部分短語搜索

現在，您可以通過在篩選窗格中僅指定搜索短語的一個部分來搜索資產。

**用例** :如果您不確定搜索的短語中出現的單詞的確切組合，則部分短語搜索會有所幫助。

例如，如果您在Brand Portal的搜索表單使用屬性謂詞對資產標題進行部分搜索，則指定術語camp將返回標題短語中帶有詞camp的所有資產。

#### 通配符搜索

Brand Portal允許在搜索查詢中使用星號(*)，並在搜索短語中使用單詞的一部分。

**用例** ：如果您不確定搜索短語中出現的確切字詞，則可以使用通配符搜索來填補搜索查詢中的空白。

例如，如果Brand Portal的搜索表單使用屬性謂詞對資產標題進行部分搜索，則指定clim*將返回所有具有以字元在其標題短語中爬升開頭的詞的資產。

同樣，指定：

* \*climb返回其標題短語中單詞以字元結尾的所有資產。
* \*climb\*返回所有具有片語的資產，這些片語的標題短語中包含字元。

#### 啟用資料夾階層

管理員現在可以配置在登錄時向非管理員用戶（編輯器、查看器和來賓用戶）顯示資料夾的方式。
[啟用資料夾層次結構](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 配置將添加到「常規設定」(General Settings)中的「管理工具」(admin tools)面板中。 如果配置為：

* 啟用後，從根資料夾開始的資料夾樹對非管理員用戶可見。 因此，為他們提供類似於管理員的導航體驗。
* 已禁用，登錄頁上只顯示共用資料夾。

[啟用資料夾層次結構](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 功能（啟用後）有助於區分具有不同層次共用的相同名稱的資料夾。 登錄時，非管理員用戶現在可以查看共用資料夾的虛擬父資料夾（和上級資料夾）。

共用資料夾在虛擬資料夾中的各個目錄內組織。 您可以使用鎖定表徵圖識別這些虛擬資料夾。

請注意，虛擬資料夾的預設縮略圖是第一個共用資料夾的縮略圖。

### Dynamic Media視頻格式副本支援

AEM Author實例處於Dynamic Media混合模式的用戶除了可以預覽原始視頻檔案外，還可以預覽和下載動態媒體格式副本。

要允許預覽和下載特定租戶帳戶上的動態媒體格式副本，管理員需要從管理工具面板中在「視頻配置」中指定「Dynamic Media配置」(視頻服務URL(DM-Gateway URL)和註冊ID以獲取動態視頻)。

Dynamic Media視頻可以在以下位置預覽：

* 資產詳細資訊頁
* 資產的卡視圖
* 連結共用預覽頁

Dynamic Media視頻編碼可從以下網址下載：

* Brand Portal
* 共用連結

### 計畫發佈到Brand Portal

資產（和資料夾）發佈工作流 [AEM(6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) 可以安排到Brand Portal的作者實例的日期、時間。

同樣，可以在以後（時間）通過安排「從Brand Portal取消發佈」工作流從門戶中刪除已發佈的資產。

### URL中的可配置租戶別名

組織可以通過在URL中設定備用前置詞來自定義其門戶URL。 要獲取其現有門戶URL中租戶名稱的別名，組織需要與Adobe支援部門聯繫。

請注意，只能自定義Brand PortalURL的前置詞，而不能自定義整個URL。
例如，具有現有域的組織 `wknd.brand-portal.adobe.com` 能 `wkndinc.brand-portal.adobe.com` 已根據請求建立。

但是，AEM作者實例可以 [配置](https://helpx.adobe.com/tw/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) 僅具有租戶ID URL，而不具有租戶別名（備用）URL。

**用例** :組織可以通過自定義門戶URL來滿足其品牌需求，而不是堅持Adobe提供的URL。

## Brand Portal2018年12月功能和增強{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### 訪客存取

AEM品牌門戶允許來賓訪問門戶。 來賓用戶不需要憑據即可進入門戶，並且可以訪問和下載所有公共資料夾和集合。 來賓用戶可以將資產添加到其燈箱（私人收藏）中並下載相同的資產。 它們還可以查看管理員設定的智慧標籤搜索和搜索謂詞。 來賓會話不允許用戶建立集合和保存的搜索或進一步共用它們、訪問資料夾和集合設定以及將資產作為連結共用。

### 加速下載

Brand Portal用戶可以利用基於Aspera的快速下載，使下載速度提高25倍，無論他們在全球的位置如何，都能享受無縫的下載體驗。 要更快地從Brand Portal或共用連結下載資產，用戶需要在下載對話框中選擇「啟用下載加速」選項，前提是其組織上啟用了下載加速。

* [加速從Brand Portal下載的指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera連接Test伺服器](https://test-connect.asperasoft.com/)

### 用戶登錄報告

已引入一個新報告來跟蹤用戶登錄。 「用戶登錄」報告有助於使組織能夠審核和檢查委派的管理員和Brand Portal的其他用戶。

報告記錄每個用戶的顯示名稱、電子郵件ID、personas(admin、viewer、editor、guest)、組、上次登錄、活動狀態和登錄計數。

### 訪問原始格式副本

管理員可以限制用戶訪問原始影像檔案(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-rgb、x-xbitmap、x-xpixmap、x-icon、image/x-photoshop、psd、image/vnd.adobe.photoshop)，並允許訪問從Brand Portal或共用連結。 此訪問可以在管理員工具面板的「用戶角色」頁的「組」頁籤的用戶組級別進行控制。

### 新配置

為管理員添加了六種新配置，以在特定租戶上啟用/禁用以下功能：

* 允許訪客存取
* 允許用戶請求訪問Brand Portal
* 允許管理員從Brand Portal刪除資產
* 允許建立公共集合
* 允許建立公共智慧集合
* 允許下載加速

### 其他增強功能

* *卡和清單視圖上的資料夾層次結構路徑*  — 使用戶能夠知道儲存在Brand Portal實例中的資料夾的位置。 幫助用戶區分不同資料夾層次結構中具有相同名稱的資料夾。
* *概述選項*  — 通過選擇資產/資料夾，然後從工具欄中選擇概述選項，提供非管理員用戶關於資產/資料夾的元資料。 當前，顯示標題、建立日期和路徑

### Adobe I/O主機UI以配置身份驗證整合

Brand Portal使用Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) 介面，用於建立JWT應用程式，它允許配置oAuth整合，以允許AEM Assets與Brand Portal整合。 以前，用於配置OAuth整合的UI托管在 `https://marketing.adobe.com/developer/`。 要瞭解有關將AEM Assets與Brand Portal整合以向Brand Portal發佈資產和收藏的更多資訊，請參閱 [配置AEM Assets與Brand Portal的整合](https://helpx.adobe.com/tw/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)。

## Brand Portal2018年2月功能和增強{#brand-portal-features-and-enhancements-632}

新功能增強，旨在使Brand Portal與AEM一致。

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### 導航改進

* 已升級的用戶介面，它與AEM對齊並使用Coral3 UI。
* 通過新的Adobe徽標快速方便地訪問管理工具。
* 通過覆蓋的產品導航
* 從子資料夾快速導航到父資料夾。
* 導航到管理工具和內容的Omnisearch選項。
* 「卡視圖」、「清單」視圖和「列視圖」可幫助您輕鬆瀏覽嵌套的資料夾。
* 「清單視圖」中的資產排序不再限於螢幕上顯示的資產數。 資料夾中的所有資產都會排序。

### 搜索改進

* Omnisearch功能允許您快速搜索Brand Portal內的資產和檔案。
* Omnisearch還提供了一個選項，用於搜索特定資料夾或位置內的資產
* 自動關鍵字建議，使搜索更容易
* 使用其他篩選器改進Omnisearch。 將搜索結果保存到智慧收藏中，以便您稍後重新訪問搜索。
* 支援智慧標籤資產搜索
* 可AEM以將智慧標籤資AEM產從Brand Portal共用，並在Brand Portal內使用智慧標籤進行資產搜索。

### 檔案共用改進

* 用戶可以使用連結共用選項共用資產。
* 共用資產時，用戶將設定每個資產的到期日期。 為用戶提供對共用資產的更多控制。
* 具有資產共用連結的外部用戶可以下載映像並查看其屬性。
* 原始嵌套資料夾層次結構將保留用於下載的資產資料夾。

### 報告和管理能力

* 現在，可以從AEM Assets發佈元資料架構AEM到Brand Portal。
* 管理員可以建立和管理三種類型的報告 — 下載、過期和發佈的資產
* 能夠配置需要包括在報告中的列。
* 為Brand Portal內的資產建立影像預設。
* 能夠修改管理員搜索欄表單或搜索Forms以包括其他篩選選項。
* 更新和預覽品牌的自定義壁紙
* 使用情況報告，用於瞭解用戶數、已用儲存空間和總資產。

## 其他資源{#additional-resources}

* [Brand Portal有什麼新聞嗎](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM作者複製代理](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [加速下載指南](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand PortalAdobe文檔](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [AEM AssetsDynamic MediaAdobe](https://experienceleague.adobe.com/docs/)
* [下載Aspera連接](https://downloads.asperasoft.com/connect2/)
* [Aspera連接Test伺服器](https://test-connect.asperasoft.com/)
