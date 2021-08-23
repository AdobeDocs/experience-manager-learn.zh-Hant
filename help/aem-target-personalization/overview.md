---
title: AEM 和 Adobe Target 快速入門
seo-title: AEM 和 Adobe Target 快速入門
description: 端對端教學課程，說明如何使用Adobe Experience Manager和Adobe Target建立和提供個人化體驗。 在本教學課程中，您還將了解端到端流程中涉及的不同角色，以及它們如何相互協作
seo-description: 端對端教學課程，說明如何使用Adobe Experience Manager和Adobe Target建立和提供個人化體驗。 在本教學課程中，您還將了解端到端流程中涉及的不同角色，以及它們如何相互協作
feature: 體驗片段
topic: 個性化
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 2%

---


# AEM 和 Adobe Target 快速入門 {#getting-started-with-aem-target}

AEM和Target都是功能強大的解決方案，且功能大致重疊。 客戶有時難以了解搭配使用這些產品來提供個人化體驗的方式與時機。 為了為每位使用者提供最佳化的體驗，您組織內的不同團隊應密切合作，並定義由誰負責。

在本教學課程中，我們會說明AEM和Target的三種不同案例，協助您了解哪些項目最適合您的組織，以及不同團隊的協作方式。

* 案例1 :使用AEM體驗片段進行個人化
* 案例2 :使用可視化體驗撰寫器進行個人化
* 案例3 :個人化完整網頁體驗

## 使用AEM體驗片段進行個人化 {#personalization-using-aem-experience-fragment}

對於此案例，我們將使用AEM和Target。 顯然，這兩種產品都有各自的優點，而若要提供個人化體驗給網站的使用者，您需要&#x200B;**個人化內容(來自AEM的內容)**&#x200B;和&#x200B;**智慧方式(Target)**，才能根據特定使用者提供這些內容。

AEM可協助您建立個人化內容，將所有內容和資產整合在集中位置，助您推行個人化策略。 AEM可讓您輕鬆在單一位置建立案頭、平板電腦和行動裝置的內容，而無需編寫程式碼。 不需要為每個裝置建立頁面，AEM會使用您的內容自動調整每個體驗。 您也可以按一下按鈕，將內容從AEM匯出至Adobe Target作為選件。

我們現在以Target中AEM優惠方案的形式提供個人化內容。 Target可讓您根據結合了行為、情境和離線變數之規則型和AI導向機器學習方法的組合，大規模提供這些選件。  透過Target，您可以輕鬆設定和執行A/B和多變數(MVT)活動，以決定最佳選件、內容和體驗。

**體驗** 片段代表將內容/體驗建立者與使用Target推動業務成果的個人化專業人員連結向前邁進的一大步。

* AEM內容編輯器作者將個人化內容視為體驗片段及其變數
* AEM將體驗片段HTML匯出至Target &#x200B;
* Target&#x200B;使用AEM體驗片段標籤作為活動中的選件
* Target提供體驗片段HTML, AEM提供參考的影像

   ![使用體驗片段圖表進行個人化](assets/personalization-use-case-1/use-case-1-diagram.png)

**若要實作此案例，您必須：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)
* [AEM和Adobe Target使用舊版Cloud Services](./implementation.md#integrating-aem-target-options)

***實作上述整合後，可讓您詳細 [探索案例](./personalization-use-case-1.md)。***

## 使用可視化體驗撰寫器進行個人化

行銷人員可以在其網站上快速變更，而不需變更任何程式碼，即可使用Adobe Target可視化體驗撰寫器(VEC)執行測試。 VEC是WYSIWYG（您看到的是您獲得的）使用者介面，可讓您輕鬆建立和測試網站內容中的個人化體驗和選件。 您可以拖放、切換及修改網頁（或選件）或行動網頁的版面與內容，借此建立Target活動的體驗和選件。

VEC是Adobe Target的其中一項主要功能。 VEC可讓行銷人員和設計人員使用視覺化介面來建立和變更內容。 無需直接編輯程式碼，便可做出許多設計選擇。 您也可以使用撰寫器中可用的編輯選項來編輯HTML和JavaScript。

* 內容駐留在AEM中，而內容編輯器會建立和管理網站頁面
* Target使用AEM托管的網站頁面執行測試和個人化
* Target提供個人化內容
* 使用Adobe Target VEC建立新內容
* 同時套用至AEM托管網站和非AEM托管網站

   ![使用可視化體驗撰寫器圖表進行個人化](assets/personalization-use-case-3/use-case-diagram-3.png)

**若要實作此案例，您必須：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實作上述整合後，可讓您詳細探 [索案例。](./personalization-use-case-3.md)***

## 個人化完整網頁體驗

將Adobe Experience Manager與Adobe Target整合可協助您為網站使用者提供個人化體驗。 此外，它也可協助您更清楚了解哪些版本的網站內容在指定的測試期間最能改善您的轉換。 例如，A/B測試會比較兩個或更多版本的網站內容，以找出最能提升轉換率、銷售額或您識別的其他量度的版本。 行銷人員可在Adobe Target中建立活動，以了解使用者與您網站內容的互動方式，以及其對您網站量度的影響。

* 內容駐留在AEM中，而內容編輯器會建立和管理網站頁面
* Target使用AEM托管的網站頁面執行測試和個人化
* Target提供個人化內容
* 此處未建立新內容
* 同時套用至AEM和非AEM網站

   ![圖表](assets/personalization-use-case-2/use-case-2-diagram.png)

**若要實作此案例，您必須：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實作上述整合後，可讓您詳細探 [索案例。](./personalization-use-case-2.md)***
