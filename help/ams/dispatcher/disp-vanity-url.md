---
title: AEM Dispatcher虛名URL功能
description: 瞭解AEM如何處理虛名URL，以及使用重寫規則將內容對應到更接近傳送邊緣的其他技術。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 244
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 0%

---

# Dispatcher虛名URL

[目錄](./overview.md)

[&lt; — 上一步：Dispatcher排清](./disp-flushing.md)

## 概觀

本檔案可協助您瞭解AEM如何處理虛名URL，以及使用重寫規則將內容對應到更接近傳送邊緣的其他技術

## 什麼是虛名URL

當您的內容位於合理的資料夾結構中時，它並不總是位於易於參照的URL中。 虛名URL就像捷徑。 參照實際內容所在位置的較短或唯一URL。

範例： `/aboutus` 指向 `/content/we-retail/us/en/about-us.html`

AEM作者可選擇在AEM中為內容設定虛名URL屬性並將其發佈。

若要使用此功能，您必須調整Dispatcher篩選器，以允許虛名通過。 以作者必須設定這些虛名頁面專案的速率來調整Dispatcher設定檔案，會變得不合理。

因此，Dispatcher模組具備的功能可自動允許內容樹狀結構中列出的任何虛名。


## 運作方式

### 製作虛名URL

作者在AEM中造訪頁面、按一下頁面屬性，然後在 _虛名URL_ 區段。 儲存變更並啟動頁面後，會將虛名指派給頁面。

作者也可以選取 _重新導向虛名URL_ 新增時的核取方塊 _虛名URL_ 登入點，會導致虛名url的行為如302重新導向。 這表示瀏覽器被告知前往新URL (透過 `Location` 回應標頭)，且瀏覽器會向新URL提出新要求。

#### 觸控式UI：

![網站編輯器畫面上AEM編寫UI的下拉式對話方塊選單](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![aem頁面屬性對話方塊頁面](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 傳統內容尋找器：

![AEM網站管理員傳統ui sidekick頁面屬性](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![傳統UI頁面屬性對話方塊](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>瞭解這可能會導致名稱空間問題。 虛專案是所有頁面的全域專案，這只是您必須規劃變通辦法的簡短說明之一，稍後我們將加以說明。


## 資源解析/對應

每個虛名專案都是用於內部重新導向的sling對應專案。

造訪AEM例項Felix主控台即可看到地圖( `/system/console/jcrresolver` )

以下是虛專案建立的地圖專案的熒幕擷圖：
![資源解析規則中虛專案的主控台熒幕擷圖](assets/disp-vanity-url/vanity-resource-resolver-entry.png "虛名 — 資源 — 解析器 — 專案")

在上述範例中，當我們要求AEM執行個體造訪時 `/aboutus` 它解析為 `/content/we-retail/us/en/about-us.html`

## Dispatcher自動允許篩選器

處於安全狀態的Dispatcher會篩選掉路徑上的請求 `/` 透過Dispatcher，因為這是JCR樹的根目錄。

請務必確保發佈者僅允許來自 `/content` 和其他安全路徑，等等，而不是像這樣的路徑 `/system`.

以下是基本資料夾中的問題和虛名URL `/` 那麼，我們如何允許他們在保持安全的同時聯絡發佈者？

簡單Dispatcher具有自動篩選允許機制，您必須安裝AEM套件，然後將Dispatcher設定為指向該套件頁面。

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher的伺服器陣列檔案中有設定區段：

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

此 `/delay` 引數（以秒為單位）並非以固定間隔為基礎來運作，而是以條件為基礎來檢查。 Dispatcher會評估 `/file` （會儲存已辨識的虛名URL清單）收到未列出URL的請求時。 此 `/file` 如果目前時間與之前時間之間的時間差，則不會重新整理 `/file`的上次修改小於 `/delay` 持續時間。 重新整理 `/file` 會在兩個條件下發生：

1. 傳入請求針對的URL未快取或未列於 `/file`.
1. 至少 `/delay` 秒數已經過了從 `/file` 上次更新時間。

此機制旨在避免阻斷服務(DoS)攻擊，否則可能會利用虛名URL功能讓Dispatcher不堪重負。

更簡單地說， `/file` 只有當要求到達的URL尚未在 `/file` 如果 `/file`的上次修改早於 `/delay` 句點。

若要明確觸發 `/file`，您可在確認下列要求後，要求不存在的URL `/delay` 自上次更新以來已過去時間。 此用途的範例URL包括：

- `https://dispatcher-host-name.com/this-vanity-url-does-not-exist`
- `https://dispatcher-host-name.com/please-hand-me-that-planet-maestro`
- `https://dispatcher-host-name.com/random-vanity-url`

此方法會強制Dispatcher更新 `/file`，已提供指定的 `/delay` 間隔自其上次修改後已過。

它會將其回應的快取儲存在 `/file` 引數，因此在此範例中 `/tmp/vanity_urls`

因此，如果您透過URI造訪AEM執行個體，便可看到其擷取內容：

![從/libs/granite/dispatcher/content/vanityUrls.html轉譯的內容熒幕擷圖](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

其實就是一份清單，非常簡單

## 將規則重寫為虛規則

為什麼要提到使用重寫規則，而不使用如上所述內建在AEM中的預設機制？

簡單來說，名稱空間問題、效能，以及可以更好處理的更高層級邏輯。

讓我們再來看看虛專案範例 `/aboutus` 至其內容 `/content/we-retail/us/en/about-us.html` 使用Apache的 `mod_rewrite` 完成此任務的模組。

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

此規則會尋找虛名 `/aboutus` 並從具有PT旗標(Pass Through)的轉譯器中擷取完整路徑。

它也會停止處理所有其他規則L標幟（最後），這表示它不必周遊大型規則清單，例如JCR解析必須執行的規則。

同時不必代理要求，並等待AEM發佈者回應此方法的這兩個元素，使其效能大幅提升。

此處錦上添花的是NC標幟（不區分大小寫），代表如果客戶輸入URI有 `/AboutUs` 而非 `/aboutus` 仍然有效。

若要建立重寫規則以執行此操作，您會在Dispatcher上建立設定檔(範例： `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`)並包含在 `.vhost` 處理需要套用這些虛名url之網域的檔案。

以下是包含於中的範常式式碼片段 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## 哪一種方法及何處

使用AEM控制虛名專案具有以下優點

- 作者可以即時建立這些檔案
- 它們會與內容共存，並可與內容一起封裝

使用 `mod_rewrite` 若要控制虛名專案，具有以下優點

- 解析內容的速度更快
- 更接近使用者內容請求的邊緣
- 更多擴充性和選項，可控制如何將內容對應到其他條件
- 可不區分大小寫

請同時使用這兩種方法，以下是建議與准則，以瞭解何時應使用哪一種方法：

- 如果虛值是暫時的，並且規劃了低層流量，則使用AEM內建功能
- 如果虛值是不經常變更且頻繁使用的固定端點，則使用 `mod_rewrite` 規則。
- 如果虛名名稱空間(例如： `/aboutus`)必須在同一個AEM執行個體上重複使用多個品牌，然後使用重寫規則。

>[!NOTE]
>
>如果您想使用AEM虛名功能並避免名稱空間，您可以制定命名慣例。 使用類似巢狀的虛名URL `/brand1/aboutus`， `brand2/aboutus`， `brand3/aboutus`.

[下一個 — >一般記錄](./common-logs.md)
