---
title: 智慧型影像處理
description: Dynamic Media Classic中的「智慧型影像」可根據使用者端瀏覽器功能自動最佳化影像格式和品質，藉以強化影像傳送效能。 它透過使用現有的影像預設集來達成此目的。 進一步瞭解智慧型影像，以及如何透過更快速的頁面載入來使用智慧型影像提供更好的客戶體驗。
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
duration: 130
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 1%

---

# 智慧型影像處理 {#smart-imaging}

客戶在您網站或行動網站或應用程式上的體驗中，最重要的一個方面是頁面載入時間。 如果頁面載入時間過長，客戶通常會捨棄網站或應用程式。 影像構成了頁面載入時間的大部分。 Dynamic Media Classic中的「智慧型影像」可根據使用者端瀏覽器功能自動最佳化影像格式和品質，藉以強化影像傳送效能。 智慧型影像處理可將影像大小減少30%以上，進而加快頁面載入速度，並提供更佳的客戶體驗。

智慧型影像還能與Adobe同級最佳的優質服務充分整合，進一步提升效能。 此服務會尋找伺服器、網路和對等點之間的最佳網際網路路由，其延遲和/或封包遺失率比網際網路上的預設路由為最低。

深入瞭解[智慧型影像](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)。

## 智慧型影像的優點

由於影像佔據了頁面載入時間的大部分，因此智慧型影像的效能提升可對業務KPI產生深遠影響，例如提高轉換率、網站逗留時間以及降低網站跳出率。

![影像](assets/smart-imaging/smart-imaging-1.png)

## 智慧型影像的運作方式

如先前所述，智慧型影像處理可搭配現有的影像預設集使用，自動將影像轉換為最佳的新一代影像格式（例如WebP），同時維持視覺逼真度。

深入瞭解[智慧型影像如何運作](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支援的影像格式（以及不使用這些格式時會發生什麼情況）等詳細資訊，及其對使用中的現有影像預設集的影響。

## 智慧型影像的影響

您可能擔心您必須變更網站上的URL、影像預設集和程式碼，才能使用智慧型影像處理。 如果您符合使用智慧型影像的先決條件，而且您只處理支援的JPEG和PNG影像格式的影像，則不需要進行任何變更。

智慧型影像處理透過HTTP、HTTPS和HTTP/2傳送的影像。

>[!NOTE]
>
>移到智慧型影像可清除CDN的快取。 CDN中的快取通常會在一或兩天內再次建立。

智慧型影像包含於您現有的Dynamic Media Classic授權中。 此功能不需額外付費。 若要充分運用此功能，您必須符合兩項需求：具備Adobe隨附的CDN和專用網域。 然後您必須為帳戶啟用，因為它未自動啟用。

若要啟用智慧型影像，您必須傳送技術支援請求，請求者為 |建立支援案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/tw/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。 支援人員將與您合作，設定您將與智慧型影像建立關聯的自訂網域。 您將變更一個與快取相關的引數（存留時間或TTL），支援將清除快取。 您也可以選擇在推送至生產環境前執行選擇性預備步驟。 接著，當智慧型影像開啟時，您會以客戶要求的相同品質，提供客戶較小大小的影像。 這表示訪客的頁面載入速度更快 — 而且所有這一切都是自動完成的。

啟用智慧型影像之後，您需要確認其是否如預期般運作。

您可能有其他關於智慧型影像的問題。 我們已整理好常見問題集(FAQ)的清單，並提供解答。 閱讀[常見問題集](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)。

## 其他資源

觀看[Dynamic Media Classic最佳化頁面效能技能建立工具](https://seminars.adobeconnect.com/pzc1gw0cihpv)隨選網路研討會，進一步瞭解智慧型影像處理。
