---
title: AEM虛名URL功能
description: 了解AEM如何運用重寫規則將內容對應至更接近傳送邊緣處，處理虛名URL和其他技巧。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---


# Dispatcher虛名URL

[目錄](./overview.md)

[&lt; — 上一個：調度程式排清](./disp-flushing.md)

## 概觀

本檔案將協助您了解AEM如何處理虛名url以及使用重寫規則將內容對應至更接近傳送邊緣的部分其他技巧

## 虛名URL是什麼

當您的資料夾結構中有內容且內容合理時，不一定會存在於易於參照的URL中。  虛名URL就像捷徑。  參考實際內容存留位置的較短或唯一URL。

範例： `/aboutus` 指向 `/content/we-retail/us/en/about-us.html`

AEM作者可以選擇在AEM中的內容上設定虛名URL屬性並發佈。

若要使用此功能，您必須調整Dispatcher篩選器，以允許虛名。  這對於以作者設定這些虛名頁面項目所需的速率調整Dispatcher設定檔案而言已不合理。

因此，Dispatcher模組具有可自動允許內容樹狀結構中列為虛名的任何項目的功能。


## 運作方式

### 編寫虛名URL

作者造訪AEM中的頁面，並造訪頁面屬性，並在虛名url區段中新增項目。

他們儲存變更並啟動頁面後，虛名現在會指派給此頁面。

#### 觸控式 UI:

![網站編輯器畫面上AEM製作UI的下拉式對話方塊功能表](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties — 下拉式清單")

![aem頁面屬性對話頁面](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 傳統內容尋找器：

![AEM siteadmin classic ui sidekick頁面屬性](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![傳統UI頁面屬性對話方塊](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
請了解，這很容易出現空間問題。

虛榮條目對所有頁面而言都是全球性的，這只是您必須為解決辦法規劃的不足之處之一，我們稍後會解釋其中的一些。
</div>

## 資源解析/映射

每個虛名項目都是內部重新導向的Sling對應項目。

瀏覽AEM例項Felix主控台即可檢視這些地圖( `/system/console/jcrresolver` )

以下是虛名項目所建立之地圖項目的螢幕擷取畫面：
![資源解析規則中虛名項目的主控台螢幕擷取畫面](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

在上述範例中，當我們要求AEM例項瀏覽 `/aboutus` 會決心 `/content/we-retail/us/en/about-us.html`

## Dispatcher自動允許篩選器

處於安全狀態的Dispatcher會篩除路徑上的請求 `/` 透過Dispatcher，因為這是JCR樹的根。

請務必確認發佈者僅允許 `/content` 和其他安全路徑等。  而不是 `/system` 等。

以下是即時於 `/` 那麼，我們如何讓他們在保持安全的同時聯繫出版商呢？

簡單Dispatcher具有自動篩選允許機制，您必須安裝AEM套件，然後設定Dispatcher以指向該套件頁面。

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher的伺服器陣列檔案中有設定區段：

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

此設定會告知Dispatcher從其AEM例項擷取此URL，每300秒前擷取一次，以擷取我們要允許的項目清單。

它會將回應的快取儲存在 `/file` 此示例中的參數 `/tmp/vanity_urls`

因此，如果您在URI造訪AEM例項，則會看到擷取的內容：
![從/libs/granite/dispatcher/content/vanityUrls.html呈現的內容螢幕截圖](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

這真的是一份清單，非常簡單

## 將規則重寫為虛名規則

我們為何要提及使用重寫規則，而非如上所述內建至AEM的預設機制？

簡單說明命名空間問題、效能，以及可更妥善處理的較高層級邏輯。

讓我們來看看虛名項目的範例 `/aboutus` 內容 `/content/we-retail/us/en/about-us.html` 使用Apache的 `mod_rewrite` 模組完成此操作。

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

此規則會尋找虛名 `/aboutus` 並使用PT標幟（傳遞）從轉譯器擷取完整路徑。

它也會停止處理所有其他規則L標幟（最後一個），這表示它不必周遊大量的規則清單，例如JCR解析必須執行的作業。

除了不必代理請求並等待AEM發佈者回應此方法的這兩個元素，讓其效能更佳。

然後，蛋糕上的糖霜是NC標誌（不區分大小寫），這表示如果客戶用 `/AboutUs` 而非 `/aboutus` 仍可運作，且可讓正確的頁面擷取。

若要建立重寫規則以執行此動作，請在Dispatcher上建立設定檔案(範例： `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`)，並將其納入 `.vhost` 處理需要套用虛名url之網域的檔案。

以下是內含的范常式式碼片段 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

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

## 哪種方法和位置

使用AEM控制虛名項目具有以下優點
- 作者可即時建立
- 他們與內容一起生活，並可與內容一起打包

使用 `mod_rewrite` 若要控制虛名項目，有下列優點
- 更快解析內容
- 更接近一般使用者內容要求的邊緣
- 更具擴充性和選項，可控制如何將內容對應至其他條件
- 可不區分大小寫

請同時使用兩種方法，但以下是建議和條件，您可在下列情況下使用：
- 如果虛名為暫時性，且規劃的流量水準低，請使用AEM內建功能
- 如果虛名是經常不變且經常使用的主要端點，請使用 `mod_rewrite` 規則。
- 如果虛名命名空間(例如： `/aboutus`)必須重複用於相同AEM例項上的許多品牌，然後使用重寫規則。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

如果您想使用AEM虛名功能並避免命名空間，可以制定命名慣例。  使用巢狀內嵌的虛名url，例如 `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Next ->常見記錄](./common-logs.md)