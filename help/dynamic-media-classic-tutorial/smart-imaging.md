---
title: 智慧型影像
description: Dynamic Media Classic中的Smart Imaging可根據用戶端瀏覽器功能自動最佳化影像格式和品質，以增強影像傳送效能。 它可運用Adobe Sensei AI功能並使用現有的影像預設集來達成此目的。 進一步瞭解智慧型影像功能，以及如何運用它透過更快的頁面載入提供更佳的客戶體驗。
sub-product: 動態媒體
feature: smart-crop
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 317fb625e7af57b7ad0079014c341eab9adda376
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 1%

---


# 智慧型影像 {#smart-imaging}

在您的網站或行動網站或應用程式上，客戶體驗最重要的方面之一，就是頁面載入時間。 如果頁面載入時間太長，客戶通常會放棄網站或應用程式。 影像是頁面載入時間的大部分。 Dynamic Media Classic中的Smart Imaging可根據用戶端瀏覽器功能自動最佳化影像格式和品質，以增強影像傳送效能。 它可運用Adobe Sensei AI功能並使用現有的影像預設集來達成此目的。 Smart Imaging將影像大小減少30%以上— 可加快頁面載入速度並改善客戶體驗。

Smart Imaging也受益於與Adobe同級最佳優質服務完全整合的額外效能提升。 此服務在伺服器、網路和互連點之間找到最佳的Internet路由，該路由的延遲和／或資料包丟失率比Internet上的預設路由低。

進一步瞭解 [Smart Imaging](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 智慧影像的優點

由於影像是頁面載入時間的大部份，因此Smart Imaging的效能提升會對商業KPI產生深遠影響，例如較高的轉換率、網站逗留時間以及較低的網站反彈率。

![影像](assets/smart-imaging/smart-imaging-1.png)

## Smart Imaging的運作方式 {#smart-imaging-1}

如前所述，智慧型影像運用Adobe Sensei AI功能，並搭配現有的影像預設集來自動將影像轉換為最佳的新一代影像格式，例如WebP，同時仍能維持視覺完整性。

進一步了 [解Smart Imaging的運作方式](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支援的影像格式（以及不使用這些格式時會發生什麼情況）等詳細資訊，以及它對現有使用的影像預設集的影響。

## Smart Imaging的影響 {#smart-imaging-1-1}

您可能擔心您必須變更網站上的URL、影像預設集和程式碼，才能運用智慧型影像。 如果您符合使用智慧型影像的先決條件，而且您只使用支援的JPEG和PNG影像格式來處理影像，則不必進行任何變更。

智慧型影像可處理透過HTTP、HTTPS和HTTP/2傳送的影像。

>[!NOTE]
>
>移至智慧型影像會清除CDN中的快取。 CDN中的快取通常會在一、兩天內重新建立。

Smart Imaging隨附於您現有的Dynamic Media Classic授權中。 此功能不需額外付費。 若要善用它，您必須符合兩項要求：擁有Adobe搭售的CDN和專屬網域。 然後，您必須為您的帳戶啟用它，因為它不會自動啟用。

啟用Smart Imaging從您發送技術支援請求開始， 建立支援案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。 支援將與您一起設定將與Smart Imaging關聯的自定義域。 您將變更與快取相關的一個參數（存留時間或TTL），並且支援會清除快取。 您也可以視需要在推送至生產環境之前，先執行選用的接移步驟。 接著，當智慧型影像開啟時，您會向客戶傳遞尺寸較小的影像，但品質與他們要求的相同。 這表示他們體驗的頁面載入時間更快— 所有這些都會自動完成，因為Adobe Sensei可協助您選擇最有效的尺寸。

啟用Smart Imaging後，您將需要驗證它是否如預期般工作。

您可能還有其他有關智慧型影像的問題。 我們已整理出一份常見問答集(FAQ)清單，提供解答。 閱讀常見 [問答](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html)。

## 其他資源

觀看 [Dynamic Media Classic最佳化頁面效能技能產生器隨選網路研討會](https://seminars.adobeconnect.com/pzc1gw0cihpv) ，進一步瞭解智慧型影像。
