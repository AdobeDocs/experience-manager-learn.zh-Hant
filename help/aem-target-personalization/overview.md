---
title: AEM 和 Adobe Target 快速入門
description: 一個端到端教程，介紹如何使用Adobe Experience Manager和Adobe Target建立和提供個性化體驗。 在本教程中，您還將瞭解端到端流程中涉及的不同角色以及它們如何相互協作
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 1%

---

# AEM 和 Adobe Target 快速入門 {#getting-started-with-aem-target}

而AEMTarget都是功能看似重疊的強大解決方案。 客戶有時難以理解如何和何時結合使用這些產品來提供個性化體驗。 為了為每個最終用戶提供優化的體驗，您組織內的不同團隊應密切協作並定義誰負責什麼。

在本教程中，我們介紹了三種不同的AEM「目標」方案，這有助於您瞭解哪些方案最適合您的組織以及不同團隊的協作方式。

* 方案1 :使用體驗AEM片段個性化
* 方案2 :使用Visual Experience Composer實現個性化
* 方案3 :全網頁體驗的個性化

## 使用體驗AEM片段個性化 {#personalization-using-aem-experience-fragment}

對於此方案，我們將使用AEM和Target。 顯然，這兩種產品都有各自的優勢，在向您的站點用戶提供個性化體驗時，您需要 **個性化內容(內容AEM)** 和 **智慧路徑（目標）** 根據特定用戶提供這些內容。

幫AEM助您建立個性化內容，將您的所有內容和資產集中到一個中心位置，以推動您的個性化策略。 AEM讓您在不編寫代碼的情況下，在一個位置輕鬆建立台式機、平板電腦和移動設備的內容。 無需為每個設備建立頁面 — 可自AEM動使用內容調整每個體驗。 您還可以通過按鈕將內AEM容作為優惠導出到Adobe Target。

我們現在有個性化內容，形式為來自目標AEM的優惠。 Target允許您基於基於規則和人工智慧驅動的機器學習方法的組合在規模上提供這些服務，這些方法結合了行為、上下文和離線變數。  使用「目標」，您可以輕鬆設定和運行A/B和多變數(MVT)活動，以確定最佳服務、內容和體驗。

**體驗片段** 代表了將內容/體驗建立者與使用Target推動業務成果的個性化專業人員聯繫起來的巨大進步。

* 內AEM容編輯器作者個性化內容Experience Fragments及其變體
* 將AEM經驗片段HTML導出到目&#x200B;標
* 目標&#x200B;將AEM體驗片段標籤用作活動中的優惠
* 目標提供體驗片段HTML,AEM提供引用的映像

   ![使用體驗片段圖實現個性化](assets/personalization-use-case-1/use-case-1-diagram.png)

**要實施此方案，您需要：**

* [使用AEM發射和Adobe I/O整合和Adobe Target](./implementation.md#integrating-aem-target-options)
* [和AEMAdobe Target使用傳統Cloud Services](./implementation.md#integrating-aem-target-options)

***實施上述整合後，讓我們瞭解 [方案詳細資訊](./personalization-use-case-1.md)。***

## 使用Visual Experience Composer實現個性化

營銷人員可以在其網站上快速更改test，而無需更改任何代碼以使用Adobe TargetVisual Experience Composer(VEC)運行操作。 VEC是WYSIWYG（您看到的是您獲得的）用戶介面，它使您能夠輕鬆建立和test站點上下文中的個性化體驗和優惠。 您可以通過拖放、交換和修改網頁（或優惠）或移動網頁的佈局和內容來為目標活動建立體驗和優惠。

VEC是Adobe Target的主要特徵之一。 VEC允許營銷人員和設計人員使用可視介面建立和更改內容。 無需直接編輯代碼即可做出許多設計選擇。 也可以使用HTML器中提供的編輯選項來編輯JavaScript。

* 內容駐留AEM在中，內容編輯器建立和管理網站頁
* 目標使AEM用托管網站頁運行test和個性化
* 目標提供個性化內容
* 使用Adobe TargetVEC建立網路新內容
* 適用於托AEM管站點和非托AEM管站點

   ![使用Visual Experience Composer圖實現個性化](assets/personalization-use-case-3/use-case-diagram-3.png)

**要實施此方案，您需要：**

* [使用AEM發射和Adobe I/O整合和Adobe Target](./implementation.md#integrating-aem-target-options)

***在實施上述整合後，讓我們 [方案。](./personalization-use-case-3.md)***

## 全網頁體驗的個性化

將Adobe Experience Manager與Adobe Target整合，可幫助您為網站用戶提供個性化體驗。 此外，它還有助於您更好地瞭解哪些版本的網站內容在指定的test期內最能改進轉換。 例如，A/Btest會比較網站內容的兩個或兩個以上版本，以查看哪個版本能最有效地提升您所識別的轉換、銷售或其他指標。 營銷人員可以在Adobe Target內建立活動，以瞭解用戶如何與您的站點內容交互以及它如何影響您的站點指標。

* 內容駐留AEM在中，內容編輯器建立和管理網站頁
* 目標使AEM用托管網站頁運行test和個性化
* 目標提供個性化內容
* 此處未建立任何新內容
* 同時應AEM用於非站AEM點

   ![圖](assets/personalization-use-case-2/use-case-2-diagram.png)

**要實施此方案，您需要：**

* [使用AEM發射和Adobe I/O整合和Adobe Target](./implementation.md#integrating-aem-target-options)

***在實施上述整合後，讓我們 [方案。](./personalization-use-case-2.md)***
