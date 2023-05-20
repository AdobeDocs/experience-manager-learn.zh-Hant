---
title: 智慧型影像
description: Dynamic Media Classic的智慧映像通過基於客戶端瀏覽器功能自動優化映像格式和質量來增強映像交付效能。 它通過利用Adobe SenseiAI功能和使用現有的影像預設來做到這一點。 瞭解有關智慧映像的更多資訊，以及如何使用它通過更快的頁面載入為客戶提供更好的體驗。
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 2%

---

# 智慧型影像 {#smart-imaging}

在您的網站或移動站點或應用程式上，客戶體驗最重要的方面之一是頁面載入時間。 如果頁面載入時間過長，客戶通常會放棄站點或應用。 影像佔頁面載入時間的大部分。 Dynamic Media Classic的智慧映像通過基於客戶端瀏覽器功能自動優化映像格式和質量來增強映像交付效能。 它通過利用Adobe SenseiAI功能和使用現有的影像預設來做到這一點。 Smart Imaging將影像大小減少30%或更多，從而加快頁面載入和改善客戶體驗。

智慧映像還得益於與Adobe中同類最佳的優質服務充分整合而帶來的額外效能提升。 此服務可在伺服器、網路和對等點之間找到最佳的Internet路由，該路由的延遲和/或資料包丟失率均低於Internet上的預設路由。

瞭解有關 [智慧映像](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)。

## 智慧映像優勢

因為影像是頁面載入時間的大部分，所以智慧映像的效能改進會對業務KPI產生深刻影響，如更高的轉換率、在站點上花費的時間以及更低的站點彈跳率。

![影像](assets/smart-imaging/smart-imaging-1.png)

## 智慧映像的工作原理

如前所述，智慧成像公司利用Adobe SenseiAI功能，並與現有的影像預設軟體配合工作，自動將影像轉換為最佳的下一代影像格式，如WebP，同時保持視覺保真度。

瞭解有關 [智慧映像的工作原理](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支援的影像格式等詳細資訊（以及如果不使用這些格式會發生什麼），以及它對正在使用的現有影像預設的影響。

## 智慧映像的影響

您可能擔心您必須對站點上的URL、影像預設和代碼進行更改，以利用智慧成像。 如果您滿足使用智慧成像的先決條件，並且您只使用受支援的JPEG和PNG影像格式的影像，則無需進行任何更改。

智慧映像可與通過HTTP、HTTPS和HTTP/2傳送的映像配合使用。

>[!NOTE]
>
>切換到智慧映像將清除CDN上的快取。 CDN中的快取通常在一兩天內重新建立。

您現有的Dynamic Media Classic許可證中包含智慧映像。 此功能沒有額外成本。 要利用它，您必須滿足以下兩個要求：具有Adobe捆綁的CDN和專用域。 然後，您必須為帳戶啟用它，因為它未自動啟用。

啟用智慧映像從向技術支援發送請求開始，方法是 |建立支援案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/tw/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。 支援將與您協作來設定您將與智慧映像關聯的自定義域。 您將更改一個與快取（生存時間或TTL）相關的參數，支援將清除快取。 如果您願意，也可以在推入生產之前執行可選的轉移步驟。 然後，啟用智慧映像後，您將向客戶交付尺寸較小的映像，但質量與他們要求的相同。 這意味著他們的頁面載入時間更快 — 所有這些都是自動完成的，因為Adobe Sensei幫助選擇最高效的大小。

啟用智慧映像後，您將希望驗證它是否按預期工作。

您可能還有其他有關智慧映像的問題。 我們已編製了常見問題(FAQ)清單，並提供了答案。 閱讀 [常見問題](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html)。

## 其他資源

觀看 [Dynamic Media Classic優化頁面效能技能生成器](https://seminars.adobeconnect.com/pzc1gw0cihpv) 按需網路研討會，瞭解有關智慧映像的更多資訊。
