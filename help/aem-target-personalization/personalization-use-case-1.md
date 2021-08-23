---
title: 使用AEM體驗片段和Adobe Target進行個人化
seo-title: 使用Adobe Experience Manager(AEM)體驗片段和Adobe Target進行個人化
description: 端對端教學課程，說明如何使用Adobe Experience Manager體驗片段和Adobe Target來建立和提供個人化體驗。
seo-description: 端對端教學課程，說明如何使用Adobe Experience Manager體驗片段和Adobe Target來建立和提供個人化體驗。
feature: 體驗片段
topic: 個性化
role: Developer
level: Intermediate
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1728'
ht-degree: 1%

---


# 使用AEM體驗片段和Adobe Target進行個人化

透過將AEM體驗片段匯出為HTML選件的功能，您可以將AEM的易用性和威力，結合Target中強大的自動化智慧(AI)和機器學習(ML)功能，以大規模測試並個人化體驗。

AEM將您所有的內容和資產整合在集中位置，助長您的個人化策略。 AEM可讓您輕鬆在單一位置建立案頭、平板電腦和行動裝置的內容，而無需編寫程式碼。 不需要為每個裝置建立頁面，AEM會使用您的內容自動調整每個體驗。

Target可讓您根據結合了行為、情境和離線變數之規則型和AI導向機器學習方法的組合，大規模提供個人化體驗。  透過Target，您可以輕鬆設定和執行A/B和多變數(MVT)活動，以決定最佳選件、內容和體驗。

體驗片段代表將內容建立者與行銷人員連結的一大步，這些行銷人員使用Target推動業務成果。

## 藍本概述

WKND網站計畫通過其網站在全美宣佈&#x200B;**SkateFest挑戰**，並希望其網站用戶註冊參加在每個州進行的試鏡。 身為行銷人員，您已獲指派在WKND網站首頁上執行促銷活動的工作，其中包含與使用者位置相關的橫幅訊息以及事件詳細資料頁面的連結。 讓我們探索WKND網站首頁，了解如何根據使用者目前的位置，為使用者建立並提供個人化體驗。

### 相關使用者

在本練習中，需要涉及以下用戶，並執行一些可能需要管理訪問權限的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **行銷人員** (Adobe Target/最佳化團隊)

### 必備條件

* **AEM**
   * [AEM作者和](./implementation.md#getting-aem) 發佈instancerunning分別在localhost 4502和4503上。
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud已布建以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND站點首頁

![AEM Target案例1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. 行銷人員與AEM內容編輯器起始WKND SkateFest行銷活動討論，並詳細說明需求。
   * ***要求***:在WKND網站首頁上提升WKND SkateFest促銷活動，為來自美國每個州的訪客提供個人化內容。在首頁輪播下方新增內容區塊，其中包含背景影像、文字及按鈕。
      * **背景影像**:影像應與使用者造訪WKND網站頁面的狀態相關。
      * **文字**:&quot;註冊Audition&quot;
      * **按鈕**:指向WKND SkateFest頁面的「活動詳細資訊」
      * **WKND SkateFest頁面**:包含活動詳細資料的新頁面，包括試鏡地點、日期和時間。
1. AEM內容編輯器會根據需求為內容區塊建立體驗片段，並將其以選件形式匯出至Adobe Target。 若要為美國的所有州提供個人化內容，內容作者可以建立一個體驗片段主變數，然後建立50個其他變數，每個州各一個。 然後，可手動編輯每個狀態變異的內容與相關影像和文字。 編寫體驗片段時，內容編輯器可以使用「資產尋找器」選項，快速存取AEM Assets中可用的所有資產。 當體驗片段匯出至Adobe Target時，其所有變數也會以選件形式推送至Adobe Target。

1. 從AEM將體驗片段匯出為Adobe Target作為選件之後，行銷人員可以使用這些選件在Target中建立活動。 根據WKND網站SkateFest行銷活動，行銷人員需要建立並提供個人化體驗給來自每個州的WKND網站訪客。 若要建立體驗鎖定目標活動，行銷人員需要識別對象。 為了進行WKND SkateFest活動，我們需要根據他們造訪WKND網站的所在位置，建立50個不同的受眾。
   * [](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) 對象會定義活動的目標，並用於任何可使用目標的位置。Target對象是一組已定義的訪客條件。 選件可以鎖定在特定對象（或區段）。 只有屬於該對象的訪客才會看見鎖定他們的體驗。  例如，您可以將優惠方案傳送給由使用特定瀏覽器或來自特定地理位置的訪客所組成的對象。
   * [選件](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)是促銷活動或活動期間顯示在網頁上的內容。 當您測試網頁時，會測量每個體驗在您位置中使用不同選件的成功程度。 選件可以包含不同類型的內容，包括：
      * 影像
      * 文字
      * **HTML**
         * *HTML選件將用於此情境的活動*
      * 連結
      * 按鈕

## 內容編輯器活動

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>先發佈體驗片段，再將其匯出至Adobe Target。

## 行銷人員活動

### 使用地理定位建立對象 {#marketer-audience}

1. 導覽至您的組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)
1. 使用Adobe ID登入，並確定您所在的組織正確無誤。
1. 在解決方案切換器中，按一下&#x200B;**Target**，然後按一下&#x200B;**launch** Adobe Target。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 導覽至&#x200B;**Offers**&#x200B;標籤並搜尋「WKND」選件。 您應該可以看到從AEM匯出為HTML選件的體驗片段變體清單。 每個選件都對應至一個狀態。 例如，*WKND SkateFest California*&#x200B;是提供給來自加州的WKND網站訪客的選件。

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. 在主導覽列中，按一下&#x200B;**對象**。

   行銷人員需要為來自美國每個州的WKND網站訪客建立50個不同的受眾。

1. 若要建立對象，請按一下&#x200B;**建立對象**&#x200B;按鈕，並提供對象的名稱。

   **對象名稱格式：WKND-\&lt;>state *\>***

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. 按一下「**新增規則>地理**」。
1. 按一下「**選擇**」，然後選擇以下選項之一：
   * 國家/地區
   * **狀態** *（WKND Site SkateFest促銷活動的選取狀態）*
   * 城市
   * 郵遞區號
   * 緯度
   * 經度
   * DMA
   * 行動電信業者

   **地理**  — 根據使用者的地理位置，包括其國家/地區、州/省、城市、郵遞區號、DMA或行動電信業者，使用對象來鎖定使用者。地理位置參數可讓您根據訪客的地理位置來定位活動和體驗。 此資料會根據訪客的IP位址，與每個Target請求一併傳送。 選取這些參數就像任何定位值一樣。

   >[!NOTE]
   >訪客的IP位址會隨mbox請求而傳遞，每次造訪（工作階段）傳遞一次，以解析該訪客的地理定位參數。

1. 選取運算子作為&#x200B;**符合**，提供適當的值(例如：加州)和&#x200B;**儲存**&#x200B;您的變更。 在本例中，請提供州名。

   ![Adobe Target — 地理規則](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >您可以為對象指派多個規則。

1. 重複步驟6-9以建立其他狀態的對象。

   ![Adobe Target - WKND Audiences](assets/personalization-use-case-1/adobe-target-audiences-50.png)

此時，我們已成功為美國不同州間的所有WKND網站訪客建立對象，並且每個州都有對應的HTML選件。 因此，現在來建立體驗鎖定目標活動，使用WKND網站首頁的對應選件來鎖定對象。

### 建立具有地理定位的活動

1. 從Adobe Target視窗，導覽至&#x200B;**Activities**&#x200B;標籤。
1. 按一下「**建立活動**」 ，然後選取「**體驗鎖定目標**」活動類型。
1. 選取&#x200B;**Web**&#x200B;通道，然後選擇&#x200B;**可視化體驗撰寫器**。
1. 輸入&#x200B;**活動URL**，然後按一下&#x200B;**下一步**&#x200B;以開啟可視化體驗撰寫器。

   WKND網站首頁發佈URL:http://localhost:4503/content/wknd/en.html

   ![體驗鎖定目標活動](assets/personalization-use-case-1/target-activity.png)

1. 若要載入&#x200B;**可視化體驗撰寫器**，請啟用在瀏覽器上允許載入不安全指令碼&#x200B;**並重新載入您的頁面。**

   ![體驗鎖定目標活動](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. 請注意WKND網站首頁已在可視化體驗撰寫器編輯器中開啟。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. 若要新增對象至您的VEC，請按一下「對象」下方的&#x200B;**新增體驗鎖定目標**，然後選取WKND-California對象，然後按一下&#x200B;**下一步**。

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. 按一下VEC內的WKND網站頁面，選取HTML元素以新增WKND-California對象的選件，然後選擇&#x200B;**取代為**&#x200B;選項，然後選取&#x200B;**HTML選件**。

   ![體驗鎖定目標活動](assets/personalization-use-case-1/vec-selecting-div.png)

1. 從選件選取UI中，為&#x200B;**WKND-California**&#x200B;對象選取&#x200B;**WKND SkateFest California** HTML選件，然後按一下&#x200B;**Done**。
1. 您現在應該會看到WKND-California觀眾的WKND網站頁面新增了&#x200B;**WKND SkateFest California** HTML選件。
1. 重複步驟7-10，將體驗鎖定目標新增至其他狀態，然後選擇對應的HTML選件。
1. 按一下&#x200B;**Next**&#x200B;以繼續，您會看到對象與體驗的對應。
1. 按一下&#x200B;**Next**&#x200B;以移至「目標與設定」。
1. 選擇您的報表來源並識別活動的主要目標。 對於我們的案例，我們會選取「報表來源」作為&#x200B;**Adobe Target**、將活動測量為&#x200B;**轉換**、將動作視為頁面，以及指向WKND SkateFest詳細資料頁面的URL。

   ![目標與目標 — 目標](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >您也可以選擇Adobe Analytics作為報表來源。

1. 暫留在目前活動名稱上，您可以將其重新命名為&#x200B;**WKND SkateFest - USA**，然後&#x200B;**儲存並關閉**&#x200B;您的變更。
1. 在「活動詳細資訊」畫面中，確定&#x200B;**啟動**&#x200B;您的活動。

   ![啟動活動](assets/personalization-use-case-1/activate-activity.png)

1. 您的WKND SkateFest促銷活動現已對所有WKND網站訪客啟用。
1. 導覽至[WKND網站首頁](http://localhost:4503/content/wknd/en.html)，您應該能夠根據您的地理位置(*狀態：加州*)。

   ![活動QA](assets/personalization-use-case-1/wknd-california.png)

### Target活動QA

1. 在「**活動詳細資料>概觀**」標籤下，按一下「**活動QA**」按鈕，即可取得連結至所有體驗的直接QA連結。

   ![活動QA](assets/personalization-use-case-1/activity-qa.png)

1. 導覽至[WKND網站首頁](http://localhost:4503/content/wknd/en.html)，您應該能根據您的地理位置（狀態）看到WKND SkateFest優惠方案。
1. 請觀看以下影片，了解優惠方案如何傳送至您的頁面、如何自訂回應Token，以及如何執行品質檢查。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 摘要

在本章中，內容編輯器能建立所有內容以支援Adobe Experience Manager中的WKND SkateFest促銷活動，並將其匯出為HTML選件，以根據使用者地理位置建立體驗鎖定目標。
