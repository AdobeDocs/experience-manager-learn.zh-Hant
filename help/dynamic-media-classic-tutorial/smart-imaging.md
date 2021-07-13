---
title: 智慧型影像
description: Dynamic Media Classic中的智慧型影像處理根據用戶端瀏覽器功能自動最佳化影像格式和品質，增強影像傳送效能。 它透過運用Adobe Sensei AI功能並使用現有的影像預設集來完成此作業。 深入了解智慧型影像處理，以及如何透過更快的頁面載入提供更優質的客戶體驗。
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: 內容管理
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 3%

---


# 智慧型影像 {#smart-imaging}

客戶在您的網站、行動網站或應用程式上體驗最重要的方面之一，就是頁面載入時間。 如果頁面載入時間太長，客戶通常會放棄網站或應用程式。 影像是頁面載入時間的大部分。 Dynamic Media Classic中的智慧型影像處理根據用戶端瀏覽器功能自動最佳化影像格式和品質，增強影像傳送效能。 它透過運用Adobe Sensei AI功能並使用現有的影像預設集來完成此作業。 智慧型影像處理可將影像大小縮小30%或更多，進而加快頁面載入速度並提供更佳的客戶體驗。

智慧映像還可通過與Adobe中同級最佳的優質服務充分整合而增強效能而獲益。 此服務在伺服器、網路和互連點之間找到最佳的Internet路由，該路由的延遲和/或資料包丟失率比Internet上的預設路由低。

深入了解[智慧影像](https://docs.adobe.com/content/help/zh-Hant/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 智慧影像處理優勢

由於影像是頁面載入時間的大部分，智慧型影像處理的效能改善可能會對業務KPI產生深遠影響，例如較高的轉換率、網站逗留時間以及較低的網站跳出率。

![影像](assets/smart-imaging/smart-imaging-1.png)

## 智慧型影像處理如何運作

如前所述，智慧影像處理利用Adobe Sensei AI功能並與現有的影像預設集搭配使用，以自動將影像轉換為最佳的新一代影像格式，例如WebP，同時維持視覺逼真度。

深入了解[智慧型影像處理如何運作](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支援的影像格式（以及如果您不使用這些格式會發生什麼情況）等詳細資訊，及其對現有使用的影像預設集的影響。

## 智慧影像處理的影響

您可能擔心必須變更網站上的URL、影像預設集和程式碼，才能運用智慧型影像處理。 如果您符合使用智慧影像的先決條件，而且您只能使用支援的JPEG和PNG影像格式的影像，則無需進行任何變更。

智慧型影像可搭配透過HTTP、HTTPS和HTTP/2傳送的影像運作。

>[!NOTE]
>
>移至智慧型影像處理會清除CDN中的快取。 CDN中的快取通常會在一兩天內重新建立。

您現有的Dynamic Media Classic授權包含智慧型影像處理。 此功能不需額外付費。 若要利用此功能，您必須符合兩項要求：有Adobe套件的CDN和專用網域。 然後，您必須為您的帳戶啟用它，因為它不會自動啟用。

啟用智慧影像處理從向請求發送技術支援開始，由 |建立支援案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。 支援會與您合作，設定您將與智慧影像處理相關聯的自訂網域。 您會變更與快取相關的一個參數（存留時間或TTL），且支援會清除快取。 您也可以在推送至生產環境之前，視需要執行選用的測試步驟。 然後，當智慧影像處理開啟時，您將向客戶提供尺寸較小的影像，但質量與他們要求的相同。 這表示他們的頁面載入時間更快，而且所有這些都會自動完成，因為Adobe Sensei可協助選擇最有效的大小。

啟用智慧影像處理後，您將需要驗證其是否如預期般運作。

您可能有其他關於智慧影像處理的問題。 我們已整理常見問題集(FAQ)的清單，提供解答。 請參閱[常見問題集](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 其他資源

請觀看[Dynamic Media Classic最佳化頁面效能技能建立器](https://seminars.adobeconnect.com/pzc1gw0cihpv)隨選網路研討會，以深入了解智慧型影像處理。
