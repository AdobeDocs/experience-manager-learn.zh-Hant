---
title: AEM 和 Adobe Target 快速入門
description: 端對端教學課程，說明如何使用 Adobe Experience Manager 和 Adobe Target 建立及提供個人化體驗。在本教學課程中，您還會了解端對端流程中所涉及的不同角色，以及他們如何協作
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '846'
ht-degree: 100%

---

# 整合 AEM Sites 與 Adobe Target {#getting-started-with-aem-target}

AEM 和 Target 兩者都是強大的解決方案，且功能大致重疊。客戶有時不易了解要用什麼方式以及在什麼時機，搭配使用這兩種產品來提供個人化體驗。為了向每位一般使用者提供最佳化體驗，組織內不同的團隊應密切合作，並定義各自的職責。

在本教學課程中，我們會介紹 AEM 和 Target 的三種不同情境，幫助您了解最適合您組織的方法，以及不同團隊間如何協作。

* 情境 1：使用 AEM Experience Fragments 進行個人化
* 情境 2：使用 Visual Experience Composer 進行個人化
* 情境 3：完整網頁體驗的個人化

## 使用 AEM Experience Fragments 進行個人化 {#personalization-using-aem-experience-fragment}

針對此情境，我們會使用 AEM 和 Target。顯然地，這兩種產品都有其各自的優勢，當需要為網站的使用者提供個人化體驗時，您需要&#x200B;**個人化的內容 (來自 AEM 的內容)** 以及&#x200B;**智慧方式 (Target)**，以便根據特定使用者提供這些內容。

AEM 協助您建立個人化內容，將您的所有內容和資產集中在中心位置，支援您實現個人化策略。您可以使用 AEM 在一個位置輕鬆地為桌上型電腦、平板電腦和行動裝置建立內容，不需要編寫程式碼。不需要為每個裝置建立頁面，AEM 會自動根據您的內容來調整每一次體驗。您也可以按下按鈕，將內容從 AEM 匯出到 Adobe Target 作為選件。

我們現在於 Target 中具有由 AEM 提供並以選件形式存在的個人化內容。Target 讓您能夠根據一組包含行為、情境和離線變數的規則型和 AI 驅動型機器學習方法，大規模傳遞這些選件。透過 Target，您可以輕鬆地設定和執行 A/B 測試及多變數 (MVT) 活動，藉此決定最佳選件、內容和體驗。

**體驗片段**&#x200B;代表內容/體驗建立者與使用 Target 來推動業務成果的個人化專家之間互相合作的長足進展。

* AEM 內容編輯器將個人化內容製作為體驗片段及其變化版本
* AEM 將體驗片段 HTML 匯出至 Target
* Target 使用 AEM Experience Fragment 標記作為活動中的選件
* Target 傳遞體驗片段 HTML，AEM 提供參照影像

  ![使用體驗片段圖表進行個人化](assets/personalization-use-case-1/use-case-1-diagram.png)

**若要實施此情境，您需要：**

* [使用標記和 Adobe I/O 整合 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)
* [使用舊版雲端服務的 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)

***實施上述整合後，便能[詳細探索該情境](./personalization-use-case-1.md)。***

## 使用 Visual Experience Composer 進行個人化

行銷人員可以在其網站上進行快速變更，無需變更任何程式碼即可使用 Adobe Target Visual Experience Composer (VEC) 執行測試。VEC 是 WYSIWYG (所見即所得) 使用者介面，讓您能夠輕鬆建立及測試網站內容中的個人化體驗和選件。您可以拖放、切換及修改網頁 (或選件) 或行動網頁的版面與內容，以便建立 Target 活動的體驗和選件。

VEC 是 Adobe Target 的其中一項主要功能。VEC 讓行銷人員和設計人員能夠使用視覺化介面來建立和變更內容。您可以進行許多設計選擇，而不需直接編輯程式碼。您也可以使用撰寫器中提供的編輯選項來編輯 HTML 和 JavaScript。

* 內容會駐留在 AEM 中，並由內容編輯者建立和管理網站頁面
* Target 使用 AEM 託管的網站頁面來執行測試和個人化
* Target 傳遞個人化內容
* 使用 Adobe Target VEC 建立全新內容
* AEM 託管網站和非 AEM 託管網站皆適用

  ![使用 Visual Experience Composer 進行個人化的圖表](assets/personalization-use-case-3/use-case-diagram-3.png)

**若要實施此情境，您需要：**

* [使用標記和 Adobe I/O 整合 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)

***實施上述整合後，便能[詳細探索該情境。](./personalization-use-case-3.md)***

## 完整網頁體驗的個人化

將 Adobe Experience Manager 與 Adobe Target 整合，有助於您為網站使用者提供個人化體驗。此外，還可以幫助您更加了解在指定的測試期間，哪些版本的網站內容最能提高您的轉換率。例如，A/B 測試會將您的兩個或多個網站內容版本進行比較，以找出提升轉換率、銷售或您指定之其他量度成效最好的版本。行銷人員可以在 Adobe Target 中建立活動來了解使用者如何與您網站的內容互動，以及這些互動如何影響您的網站量度。

* 內容會駐留在 AEM 中，並由內容編輯者建立和管理網站頁面
* Target 使用 AEM 託管的網站頁面來執行測試和個人化
* Target 傳遞個人化內容
* 不會建立任何新內容
* AEM 和非 AEM 網站皆適用

  ![圖表](assets/personalization-use-case-2/use-case-2-diagram.png)

**若要實施此情境，您需要：**

* [使用標記和 Adobe I/O 整合 AEM 和 Adobe Target](./implementation.md#integrating-aem-target-options)

***實施上述整合後，便能[詳細探索該情境。](./personalization-use-case-2.md)***
