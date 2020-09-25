---
title: AEM和Adobe Target快速入門
seo-title: AEM和Adobe Target快速入門
description: 教學課程的端對端說明如何使用Adobe Experience Manager和Adobe Target建立和傳遞個人化體驗。 在本教學課程中，您也將瞭解端對端流程中不同角色的相關資訊，以及他們如何彼此協作
seo-description: 教學課程的端對端說明如何使用Adobe Experience Manager和Adobe Target建立和提供個人化體驗。 在本教學課程中，您也將瞭解端對端流程中不同角色的相關資訊，以及他們如何彼此協作
translation-type: tm+mt
source-git-commit: c4ddafe392f74be8401f3ef6e07fc9d463d7620a
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---


# AEM和Adobe Target快速入門 {#getting-started-with-aem-target}

AEM和Target都是功能強大且功能看似重疊的解決方案。 客戶有時很難瞭解如何及何時搭配使用這些產品來提供個人化體驗。 為了為每位使用者提供最佳化的體驗，您組織內不同的團隊應密切合作，並定義誰負責。

在本教學課程中，我們針對AEM和Target提供三種不同的案例，協助您瞭解哪些方案對您的組織最有用，以及不同團隊如何協作。

* 方案1:使用AEM體驗片段進行個人化
* 方案2:使用Visual Experience Composer進行個人化
* 方案3:完整網頁體驗的個人化

## 使用AEM體驗片段進行個人化 {#personalization-using-aem-experience-fragment}

針對此案例，我們將使用AEM和Target。 顯然，這兩種產品各有其優勢，在為您網站的使用者提供個人化體驗時，您需要個人化內容（來自AEM的內容） **** ，以及智慧方式(Target) **** ，以根據特定使用者提供這些內容。

AEM可協助您建立個人化內容，將您的所有內容和資產整合在一個中心位置，以推動您的個人化策略。 AEM可讓您在單一位置輕鬆建立適用於桌上型電腦、平板電腦和行動裝置的內容，毋需編寫程式碼。 您不需要為每個裝置建立頁面- AEM會使用您的內容自動調整每個體驗。 您也可以透過按鈕，將內容從AEM匯出至Adobe Target，成為選件。

我們現在擁有Target中AEM提供的優惠形式的個人化內容。 Target可讓您根據結合行為、情境和離線變數的規則導向和人工智慧導向機器學習方法的組合，大規模提供這些選件。  有了Target，您可以輕鬆設定並執行A/B和多變數(MVT)活動，以決定最佳選件、內容和體驗。

**體驗片段** ，代表了將內容／體驗建立者連結至個人化專業人員的一大步，這些專業人員使用Target推動業務成果。

* AEM內容編輯器作者個人化內容為「體驗片段」及其變體
* AEM將體驗片段HTML匯出至Target &#x200B;
* Target將&#x200B;AEM體驗片段標籤用作活動中的選件
* Target提供Experience Fragment HTML,AEM提供參考影像

   ![使用體驗片段圖表進行個人化](assets/personalization-use-case-1/use-case-1-diagram.png)

**若要實作此藍本，您必須：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)
* [使用舊版雲端服務的AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實作上述整合後，請讓您詳細[瞭解藍本](./personalization-use-case-1.md)。***

## 使用Visual Experience Composer進行個人化

行銷人員可在其網站上快速變更，而不需變更任何程式碼，即可使用Adobe Target Visual Experience Composer(VEC)執行測試。 VEC是WYSIWYG（所見即所得）使用者介面，可讓您輕鬆建立和測試網站內容中的個人化體驗和選件。 您可以拖放、交換及修改網頁（或選件）或行動網頁的版面配置和內容，以建立Target活動的體驗和選件。

VEC是Adobe Target的主要功能之一。 VEC可讓行銷人員和設計人員使用視覺化介面來建立和變更內容。 您可進行許多設計選擇，毋需直接編輯程式碼。 您也可以使用撰寫器中提供的編輯選項來編輯HTML和JavaScript。

* 內容駐留在AEM中，內容編輯人員可建立和管理網站頁面
* Target使用AEM代管的網站頁面執行測試和個人化
* Target提供個人化內容
* 使用Adobe Target VEC建立新內容
* 同時套用至AEM代管網站和非AEM代管網站

   ![使用Visual Experience Composer圖表進行個人化](assets/personalization-use-case-3/use-case-diagram-3.png)

**若要實作此藍本，您必須：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實作上述整合後，讓我們詳細探[討藍本。](./personalization-use-case-3.md)***

## 完整網頁體驗的個人化

將Adobe Experience Manager與Adobe Target整合，可協助您為網站使用者提供個人化體驗。 此外，它也有助於您進一步瞭解哪些版本的網站內容在指定的測試期間最能提升轉換率。 例如，A/B測試會比較您的兩個或多個網站內容版本，以找出哪個版本最能提升轉換、銷售或您識別的其他量度。 行銷人員可在Adobe Target中建立活動，以瞭解使用者如何與您網站的內容互動，以及它對您網站量度的影響。

* 內容駐留在AEM中，內容編輯人員可建立和管理網站頁面
* Target使用AEM代管的網站頁面執行測試和個人化
* Target提供個人化內容
* 此處未建立新內容
* 同時套用至AEM和非AEM網站

   ![圖表](assets/personalization-use-case-2/use-case-2-diagram.png)

**若要實作此藍本，您必須：**

* [使用Launch和Adobe I/O整合AEM和Adobe Target](./implementation.md#integrating-aem-target-options)

***實作上述整合後，讓我們詳細探[討藍本。](./personalization-use-case-2.md)***
