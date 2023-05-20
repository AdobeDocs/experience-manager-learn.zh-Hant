---
title: 使用經驗AEM片段和Adobe Target
seo-title: Personalization using Adobe Experience Manager (AEM) Experience Fragments and Adobe Target
description: 一個端到端教程，介紹如何使用Adobe Experience Manager體驗片段和Adobe Target建立和提供個性化體驗。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Experience Manager Experience Fragments and Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1691'
ht-degree: 1%

---

# 使用經驗AEM片段和Adobe Target

通過將體驗AEM片段按HTML提供的方式導出到Adobe Target，您可以將其易用性和強大功能AEM與Target中強大的自動智慧(AI)和機器學習(ML)功能相結合，以在規模上test和個性化體驗。

將您AEM的所有內容和資產集中到一個中心位置，以推動您的個性化策略。 AEM讓您在一個位置輕鬆建立台式機、平板電腦和移動設備的內容，而無需編寫代碼。 無需為每個設備建立頁面 — 可自AEM動使用內容調整每個體驗。

Target允許您基於基於規則和人工智慧驅動的機器學習方法的組合，在規模上提供個性化體驗，這些方法結合了行為、上下文和離線變數。  使用「目標」，您可以輕鬆設定和運行A/B和多變數(MVT)活動，以確定最佳服務、內容和體驗。

經驗片段代表了向內容建立者與營銷者之間連接邁出的一大步，營銷者正在使用Target推動業務成果。

## 方案概述

WKND網站計畫宣佈 **SkateFest挑戰賽** 希望讓網站用戶報名參加各州的試鏡。 作為營銷員，您已分配了在WKND網站首頁上運行市場活動的任務，其中包含與用戶位置相關的橫幅消息和指向事件詳細資訊頁面的連結。 讓我們瀏覽WKND網站首頁並瞭解如何根據用戶當前的位置為用戶建立和提供個性化體驗。

### 涉及的用戶

在本練習中，需要讓以下用戶參與並執行一些可能需要管理訪問權限的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **營銷商** (Adobe Target/優化團隊)

### 必備條件

* **AEM**
   * [AEM作者和發佈實例](./implementation.md#getting-aem) 分別運行在localhost 4502和4503上。
* **Experience Cloud**
   * 訪問您的組織Adobe Experience Cloud。 `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND網站首頁

![目標AEM方案1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. Marketer與內容編輯器啟動WKND SkateFest活動AEM討論並詳細說明要求。
   * ***要求***:在WKND網站首頁上為來自美國每個州的訪問者宣傳WKND SkateFest活動，並提供個性化內容。 在首頁旋轉盤下添加新內容塊，其中包含背景影像、文本和按鈕。
      * **背景影像**:映像應與用戶訪問WKND網站頁的狀態相關。
      * **文本**:&quot;註冊Audition&quot;
      * **按鈕**:指向WKND SkateFest頁面的「活動詳細資訊」
      * **WKND SkateFest頁**:包含活動詳細資訊的新頁面，包括試鏡地點、日期和時間。
1. 根據這些要求，內AEM容編輯器為內容塊建立一個「體驗」片段，並將其作為優惠導出到Adobe Target。 要為美國所有州提供個性化內容，內容作者可以建立一個體驗片段主變體，然後建立50個其他變體，每個州建立一個。 然後可以手動編輯每種狀態變化與相關影像和文本的內容。 創作體驗片段時，內容編輯器可以使用「資產查找器」選項快速訪問AEM Assets內的所有可用資產。 當一個體驗片段被輸出到Adobe Target時，它的所有變體也作為優惠被推到Adobe Target。

1. 在將體驗片段作為AEM優惠從導出到Adobe Target後，營銷人員可以使用這些優惠在目標中建立活動。 根據WKND網站SkateFest活動，營銷人員需要建立並向來自每個州的WKND網站訪問者提供個性化的體驗。 要建立「體驗目標」活動，商家需要確定受眾。 為了開展WKND SkateFest活動，我們需要根據觀眾訪問WKND網站的地點，建立50個不同的觀眾。
   * [觀眾](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) 為活動定義目標，並在目標可用的任何位置使用。 目標受眾是已定義的訪問者標準集。 可以針對特定受眾（或段）。 只有屬於該受眾的訪問者才能看到針對他們的體驗。  例如，您可以向由使用特定瀏覽器或特定地理位置的訪問者組成的受眾提供優惠。
   * 安 [優惠](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) 是在市場活動或活動期間在網頁上顯示的內容。 test網頁時，您可以衡量每個體驗在您所在位置的成功程度。 優惠可包含不同類型的內容，包括：
      * 影像
      * 文字
      * **HTML**
         * *HTML優惠用於此方案的活動*
      * 連結
      * 按鈕

## 內容編輯器活動

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>在將體驗片段導出到Adobe Target之前發佈它。

## 營銷商活動

### 建立具有地域目標的受眾 {#marketer-audience}

1. 導航到您的組織 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. 使用您的Adobe ID登錄，並確保您所在的組織正確。
1. 在解決方案切換器中，按一下 **目標** 然後 **啟動** Adobe Target。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 導航到 **優惠** 並搜索「WKND」選項。 您應該能夠查看從HTML優惠導出的體驗片段變體AEM清單。 每個優惠都對應一個狀態。 比如說， *加利福尼亞WKND冰鞋節* 是給來自加利福尼亞的WKND Site訪客的優惠。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. 在主導航中，按一下 **觀眾**。

   Marketer需要為來自美利堅合眾國每個州的WKND網站訪問者建立50個不同的受眾。

1. 要建立受眾，請按一下 **建立受眾** 按鈕，並為受眾提供名稱。

   **受眾名稱格式：WKND-\&lt;*狀態*\>**

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. 按一下 **添加規則>地理位置**。
1. 按一下 **選擇**，然後選擇以下選項之一：
   * 國家/地區
   * **州** *（為WKND站點SkateFest活動選擇狀態）*
   * 城市
   * 郵遞區號
   * 緯度
   * 經度
   * DMA
   * 移動運營商

   **吉奧**  — 根據用戶的地理位置（包括其國家/省、市、郵遞區號、DMA或移動運營商），使用受眾瞄準用戶。 地理位置參數允許您根據訪問者的地理位置確定活動和體驗的目標。 此資料隨每個目標請求一起發送，並基於訪問者的IP地址。 選擇這些參數與任何目標值一樣。

   >[!NOTE]
   >訪問者的IP地址會通過一個mbox請求(每次訪問（會話）一次)來解決該訪問者的地理目標參數。

1. 選擇運算子為 **匹配**，提供適當的值(例如：加州) **保存** 您的更改。 在我們的情況下，提供州名。

   ![Adobe Target — 地理規則](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >您可以為受眾分配多個規則。

1. 重複步驟6-9為其他州建立觀眾。

   ![Adobe Target- WKND觀眾](assets/personalization-use-case-1/adobe-target-audiences-50.png)

目前，我們已經成功地為美國不同州的所有WKND網站訪問者建立了觀眾群，並為每個州提供了相應的HTML。 現在，讓我們建立一個「體驗目標」活動，以針對WKND網站首頁提供相應的優惠。

### 建立具有地理目標的活動

1. 從您的Adobe Target窗口，導航到 **活動** 頁籤。
1. 按一下 **建立活動** 的 **體驗目標** 活動類型。
1. 選擇 **Web** 選擇 **視覺體驗作曲家**。
1. 輸入 **活動URL** 按一下 **下一個** 開啟視覺體驗作曲家。

   WKND網站首頁發佈URL:http://localhost:4503/content/wknd/en.html

   ![體驗目標活動](assets/personalization-use-case-1/target-activity.png)

1. 對於 **視覺體驗作曲家** 載入，啟用 **允許載入不安全指令碼** 重新載入頁面。

   ![體驗目標活動](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. 請注意在Visual Experience Composer編輯器中開啟的WKND網站首頁。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. 要向VEC添加受眾，請按一下 **添加體驗目標** 在「觀眾」下，選擇WKND-California觀眾，然後按一下 **下一個**。

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. 按一下VEC中的WKND站點頁面，選擇HTML元素以添加WKND-California受眾的優惠，然後選擇 **替換為** 選項，然後選擇 **HTML優惠**。

   ![體驗目標活動](assets/personalization-use-case-1/vec-selecting-div.png)

1. 選擇 **加利福尼亞WKND冰鞋節** HTML **加利福尼亞州** 來自優惠的受眾選擇UI，然後按一下 **完成**。
1. 你現在應該能看到 **加利福尼亞WKND冰鞋節** HTML優惠已添加到WKND-California觀眾的WKND網站頁面。
1. 重複步驟7-10為其他狀態添加「體驗目標」並選擇相應的HTML服務。
1. 按一下 **下一個** 繼續，您可以看到「觀眾」到「體驗」的映射。
1. 按一下 **下一個** 按鈕。
1. 選擇報告來源並確定活動的主要目標。 對於我們的方案，我們選擇報告源作為 **Adobe Target**，測量活動 **轉換**，操作作為查看的頁面，以及指向WKND SkateFest詳細資訊頁面的URL。

   ![目標和目標 — 目標](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >您還可以選擇Adobe Analytics作為報告來源。

1. 將滑鼠懸停在當前活動名稱上，可將其更名為 **WKND滑冰節 — 美國**，然後 **保存並關閉** 您的更改。
1. 在「活動詳細資訊」螢幕中，確保 **激活** 您的活動。

   ![激活活動](assets/personalization-use-case-1/activate-activity.png)

1. 您的WKND SkateFest活動現在對所有WKND Site訪問者進行現場直播。
1. 導航到 [WKND網站首頁](http://localhost:4503/content/wknd/en.html)，您應該能夠根據您的地理位置(*狀態：加利福尼亞*)。

   ![活動QA](assets/personalization-use-case-1/wknd-california.png)

### 目標活動QA

1. 下 **活動詳細資訊>概覽** 的 **活動QA** 按鈕，您可以獲得所有體驗的直接QA連結。

   ![活動QA](assets/personalization-use-case-1/activity-qa.png)

1. 導航到 [WKND網站首頁](http://localhost:4503/content/wknd/en.html)，您應該能夠根據您的地理位置（州）查看WKND SkateFest服務。
1. 觀看下面的視頻，瞭解如何將優惠送達您的頁面、如何自定義響應令牌以及如何執行質量檢查。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 摘要

在本章中，內容編輯器能夠建立所有內容以支援Adobe Experience Manager的WKND SkateFest活動，並將其作為HTML產品導出到Adobe Target，以建立基於用戶地理位置的體驗目標。
