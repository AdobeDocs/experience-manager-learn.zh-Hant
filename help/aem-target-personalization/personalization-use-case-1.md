---
title: 使用體驗片段AEM和Adobe Target的個人化
seo-title: 使用Adobe Experience Manager(AEM)體驗片段和Adobe Target的個人化
description: 端對端教學課程，說明如何使用Adobe Experience Manager體驗片段和Adobe Target來建立和提供個人化體驗。
seo-description: 端對端教學課程，說明如何使用Adobe Experience Manager體驗片段和Adobe Target來建立和提供個人化體驗。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1734'
ht-degree: 1%

---


# 使用體驗片段AEM和Adobe Target的個人化

透過將AEMExperience片段匯出為HTML提供的功能，您可以結合Target中的易用性和強大功能AEM，以及強大的自動化智慧(AI)和機器學習(ML)功能，以大規模測試和個人化體驗。

將您AEM的所有內容和資產整合在一個中心位置，以推動您的個人化策略。 讓您AEM在單一位置輕鬆建立適用於桌上型電腦、平板電腦和行動裝置的內容，毋需編寫程式碼。 不需要為每個裝置建立頁面，而是使用您的內AEM容自動調整每個體驗。

Target可讓您根據結合行為、情境和離線變數的基於規則和人工智慧驅動機器學習方法的組合，大規模地提供個人化體驗。  有了Target，您可以輕鬆設定並執行A/B和多變數(MVT)活動，以決定最佳選件、內容和體驗。

體驗片段代表了將內容建立者與行銷人員連結在一起的一大步，這些行銷人員使用Target推動業務成果。

## 藍本概觀

WKND網站計畫透過其網站在全美推出&#x200B;**SkateFest競賽**，並希望讓其網站使用者註冊每個州的試用。 身為行銷人員，您已獲指派在WKND網站首頁上執行促銷活動的工作，其中包含與使用者位置相關的橫幅訊息，以及事件詳細資料頁面的連結。 讓我們探索WKND網站首頁，並瞭解如何根據使用者目前的位置，為使用者建立並提供個人化體驗。

### 相關使用者

在本練習中，需要有下列用戶參與，並執行一些可能需要管理訪問權限的任務。

* **內容製作者／內容編輯者** (Adobe Experience Manager)
* **行銷人員** (Adobe Target/最佳化團隊)

### 必備條件

* **AEM**
   * [分AEM別在](./implementation.md#getting-aem) localhost 4502和4503上製作和發佈例項。
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解決方案
      * [Adobe Target](https://experiencecloud.adobe.com)

### WKND網站首頁

![目AEM標方案1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. 行銷人員會與內容編輯器開始WKND SkateFestAEM促銷活動討論，並詳細說明需求。
   * ***要求***:在WKND網站首頁上針對美國各州的訪客推廣WKND SkateFest促銷活動，提供個人化內容。在「首頁轉盤」下方新增內容區塊，其中包含背景影像、文字和按鈕。
      * **背景影像**:影像應與使用者瀏覽WKND網站頁面的狀態相關。
      * **文字**:&quot;註冊Audition&quot;
      * **按鈕**:指向WKND SkateFest頁面的「活動詳細資訊」
      * **WKND SkateFest頁面**:包含活動詳細資訊的新頁面，包括試鏡地點、日期和時間。
1. 內容編輯器會根據AEM需求為內容區塊建立體驗片段，並將它匯出為選件至Adobe Target。 為了為美國所有州提供個人化內容，內容作者可以建立一個體驗片段主變數，然後建立50個其他變數，每個州各一個。 然後，您可以手動編輯每個狀態變化與相關影像和文字的內容。 製作體驗片段時，內容編輯人員可使用「資產搜尋器」選項，快速存取AEM Assets地區的所有可用資產。 當體驗片段匯出至Adobe Target時，其所有變數也會以選件的形式推送至Adobe Target。

1. 將體驗片段從匯出AEM為選件後，行銷人員可以使用這些選件在Target中建立活動。 根據WKND網站SkateFest促銷活動，行銷人員需要建立個人化的體驗，並從每個州向WKND網站訪客提供。 若要建立「體驗定位」活動，行銷人員需要識別受眾。 我們的WKND SkateFest活動需要根據觀眾造訪WKND網站的位置，建立50個不同的觀眾。
   * [對](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) 像會定義您活動的目標，並用於任何有目標的地方。目標對象是一組定義的訪客條件。 選件可定位至特定對象（或區段）。 只有屬於該對象的訪客才會看到其目標體驗。  例如，您可以將選件傳送給由使用特定瀏覽器或來自特定地理位置的訪客所組成的觀眾。
   * [選件](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9)是促銷活動或活動期間在您的網頁上顯示的內容。 當您測試網頁時，會測量每個體驗在您所在位置使用不同選件時的成功程度。 選件可包含不同類型的內容，包括：
      * 影像
      * 文字
      * **HTML**
         * *HTML選件將用於此藍本的活動*
      * 連結
      * 按鈕

## 內容編輯器活動

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>將體驗片段匯出至Adobe Target之前先發佈。

## 行銷人員活動

### 建立具有地理定位{#marketer-audience}的對象

1. 導覽至您的組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/)(<https://>`<yourcompany>`.experiencecloud.adobe.com)
1. 使用您的Adobe ID登入，並確定您所在的組織正確。
1. 在解決方案切換器中，按一下&#x200B;**Target**，然後按一下&#x200B;**launch** Adobe Target。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. 導覽至&#x200B;**選件**&#x200B;標籤，並搜尋「WKND」選件。 您應該可以查看從HTML選件匯出的「體驗片段」變AEM數清單。 每個選件都對應一個狀態。 例如，*WKND SkateFest California*&#x200B;是提供給來自加州的WKND網站訪客的選件。

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. 在主導覽中，按一下「觀眾」**。**

   行銷人員需要為來自美國每個州的WKND網站訪客建立50個不同的觀眾。

1. 若要建立對象，請按一下「建立對象」按鈕，並為對象提供名稱。****

   **對象名稱格式：WKND-\&lt;>state *\>***

   ![Experience Cloud-Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. 按一下「新增規則>地理&#x200B;**」。**
1. 按一下&#x200B;**選擇** ，然後選擇以下選項之一：
   * 國家/地區
   * **狀態** *（WKND Site SkateFest促銷活動的選取狀態）*
   * 城市
   * 郵遞區號
   * 緯度
   * 經度
   * DMA
   * 行動電信業者

   **地理** -使用對象根據其地理位置來定位使用者，包括其國家、州／省、城市、郵遞區號、DMA或行動電信業者。地理位置參數可讓您根據訪客的地理位置來定位活動和體驗。 此資料會隨每個Target請求傳送，並以訪客的IP位址為基礎。 選取這些參數，就像任何定位值一樣。

   >[!NOTE]
   >訪客的IP位址會隨mbox請求傳遞，每次瀏覽（作業）一次，以解決該訪客的地理定位參數。

1. 選取運算元為&#x200B;**matches**，提供適當的值(例如：California)和&#x200B;**儲存您的變更。**&#x200B;在本例中，請提供州名。

   ![Adobe Target-地理規則](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >您可以為對象指派多個規則。

1. 重複步驟6-9以建立其他狀態的觀眾。

   ![Adobe Target- WKND Audiences](assets/personalization-use-case-1/adobe-target-audiences-50.png)

目前，我們已成功為美國不同州的所有WKND網站訪客建立對象，並為每個州提供對應的HTML選件。 現在，讓我們建立「體驗定位」活動，以針對WKND網站首頁的對應選件來定位對象。

### 建立具有地理定位的活動

1. 從您的Adobe Target窗口，導航至&#x200B;**活動**&#x200B;頁籤。
1. 按一下「建立活動」**，然後選取「體驗定位」**&#x200B;活動類型。****
1. 選擇&#x200B;**Web**&#x200B;頻道，然後選擇&#x200B;**Visual Experience Composer**。
1. 輸入&#x200B;**活動URL**，然後按一下&#x200B;**下一步**&#x200B;以開啟Visual Experience Composer。

   WKND網站首頁發佈URL:http://localhost:4503/content/wknd/en.html

   ![體驗定位活動](assets/personalization-use-case-1/target-activity.png)

1. 若要載入&#x200B;**Visual Experience Composer**，請啟用瀏覽器上的「允許載入不安全指令碼&#x200B;**」並重新載入頁面。**

   ![體驗定位活動](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. 請注意，WKND網站首頁在Visual Experience Composer編輯器中開啟。

   ![VEC](assets/personalization-use-case-1/vec.png)

1. 若要新增對象至VEC，請按一下「對象」下方的「新增體驗定位」**，然後選取「WKND-California對象」，然後按一下「下一步」**「下一步」。****

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. 按一下VEC中的WKND網站頁面，選取HTML元素以新增WKND-California觀眾的選件，然後選擇「取代為&#x200B;**」選項，然後選取「** HTML選件&#x200B;**」。**

   ![體驗定位活動](assets/personalization-use-case-1/vec-selecting-div.png)

1. 從選件選擇的UI中，為&#x200B;**WKND-California**&#x200B;觀眾選取&#x200B;**WKND SkateFest California** HTML選件，然後按一下&#x200B;**Done**。
1. 您現在應該可以看到WKND-California觀眾的WKND網站頁面中已新增&#x200B;**WKND SkateFest California** HTML選件。
1. 重複步驟7-10，為其他狀態新增「體驗定位」，並選擇對應的HTML選件。
1. 按一下「**Next**」繼續，您會看到「觀眾」與「體驗」的對應。
1. 按一下「**Next**」移至「目標與設定」。
1. 選擇您的報表來源並識別活動的主要目標。 對於我們的藍本，我們選取「報表來源」作為「**Adobe Target**」、將活動測量為「**轉換**」、將動作視為頁面，以及指向「WKND SkateFest詳細資料」頁面的URL。

   ![目標與目標——目標](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >您也可以選擇Adobe Analytics做為您的報表來源。

1. 將滑鼠指標暫留在目前的活動名稱上，您可將它重新命名為&#x200B;**WKND SkateFest - USA**，然後再將變更重新命名為&#x200B;**儲存並關閉**。
1. 在「活動詳細資訊」螢幕中，確保&#x200B;**激活**&#x200B;您的活動。

   ![啟動活動](assets/personalization-use-case-1/activate-activity.png)

1. 您的WKND SkateFest促銷活動現在可供所有WKND網站訪客使用。
1. 導覽至[WKND網站首頁](http://localhost:4503/content/wknd/en.html)，您就可以根據您的地理位置(*狀態：California*)。

   ![活動QA](assets/personalization-use-case-1/wknd-california.png)

### 目標活動QA

1. 在「**活動詳細資料>概述**」標籤下，按一下「活動QA」按鈕，即可取得所有體驗的直接QA連結。****

   ![活動QA](assets/personalization-use-case-1/activity-qa.png)

1. 導覽至[WKND網站首頁](http://localhost:4503/content/wknd/en.html)，您就可以根據您的地理位置（州）查看WKND SkateFest優惠。
1. 觀看以下影片，瞭解選件如何傳送至您的頁面、如何自訂回應Token，以及執行品質檢查。

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## 摘要

在本章中，內容編輯器可以建立所有內容，以支援Adobe Experience Manager的WKND SkateFest促銷活動，並將它匯出為HTML選件，以建立以使用者地理位置為基礎的體驗定位。
