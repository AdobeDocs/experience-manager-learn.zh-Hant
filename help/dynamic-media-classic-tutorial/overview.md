---
title: 歡迎使用Dynamic Media經典最佳做法教程
description: Dynamic Media經典是客戶建立、製作和提供多媒體內容的中心。 本最佳實務教學課程旨在協助目前和新來的Dynamic Media經典使用者更全面地瞭解他們如何運用Adobe提供的強大多媒體解決方案。 在本教學課程中，您將瞭解Dynamic Media經典之作，並簡要瞭解其核心功能和使用者介面。
sub-product: 動態媒體
doc-type: tutorial
topics: best-practices, development, authoring, configuring
audience: all
activity: develop, use
feature: Dynamic Media Classic
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 0%

---


# 歡迎使用Dynamic Media經典最佳做法教程

本指南旨在協助目前和新的Dynamic Media經典使用者，更全面地瞭解他們如何運用Adobe提供的強大多媒體解決方案。 我們將通過以下方式完成：

- 為您介紹Dynamic Media經典，介紹它的功能，並概述其核心功能和使用者介面(UI)。
- 說明在解決方案中使用資產時，將遵循的一般「建立」、「編寫」和「傳送」工作流程。
- 在跳入和使用解決方案之前，討論要設定的重要項目。
- 深入探討如何使用解決方案的多項核心功能。

在整個指南中，我們將提供範例、秘訣和最佳實務。 我們也會說明您在使用Dynamic Media經典時應熟悉的重要辭彙和概念。 當特定主題有提供時，我們會引導您參加相關的網路研討會、部落格文章和線上檔案。

我們希望本指南能為您提供所需的資訊，讓您從Dynamic Media經典解決方案中獲得巨大價值。 若要更輕鬆地導覽本指南的章節，請按一下指南左側的書籤圖示，以檢視其內容。

## Dynamic Media經典

Dynamic Media經典是客戶建立、製作和提供多媒體內容的中心。 Dynamic Media經典是整合的豐富式媒體管理、出版與服務環境。 您可將多媒體內容發佈至所有行銷和銷售通道，包括網頁、印刷資料、電子郵件宣傳、網頁應用程式、桌上型電腦和裝置。

影像伺服或許是Dynamic Media經典最常使用的功能。 事實上，大部分客戶都會使用Dynamic Media經典影像來提供網站上的所有影像，包括縮放影像或多媒體影像。 不過，它也可用於其他許多用途，包括視訊傳送和使用人工智慧來最佳化傳送的影像。

## Dynamic Media經典的核心能力

在本指南中，我們將討論以下Dynamic Media經典的核心功能。

- **Dynamic Imaging。** 即時編輯、格式化和調整大小，以及互動式縮放和平移的傘形詞語；顏色和紋理切換；360度旋轉；影像模板；和多媒體檢視器。
- **影片.** 上傳最終影片、發佈，並逐步將它們下載至可設定的影片檢視器。
- **智慧型影像.** 運用Adobe SenseiAI功能並與現有「影像預設集」搭配使用的技術，可根據用戶端瀏覽器功能自動最佳化影像格式、大小和品質，以增強影像傳送效能。

若要探索解決方案的其他功能，請造訪[Dynamic MediaClassic的檔案](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/intro/introduction.html)。

## Dynamic Media經典用戶介面(UI)

Dynamic Media經典主要UI包括三個主要領域：全域導覽列、資產庫和瀏覽面板／建置面板。

![影像](assets/overview/overview-dmc-ui-ew.png)

_Dynamic Media經典UI_

**全域導覽列。** 位於螢幕頂端，您將使用此列上的按鈕來存取解決方案的關鍵區域和功能。例如，您將使用它存取上傳功能、開啟各種資產建立區域（影像集、回轉集等）、執行重要工作，例如設定影像預設集和檢視器預設集，以及發佈您的資產。 您也可以從這裡監控工作、查看最近的活動，以及從各種說明選項中選擇。

**資產庫.** 畫面左側的是「資產庫」，此面板可用來組織您所建立之檔案夾和子檔案夾中的資產。在面板的頂端，您會找到搜尋和篩選器，以協助您找到資產。 「進階搜尋」可讓您指定多個選項作為搜尋標準，包括附加至該資產的隱藏中繼資料欄位，以進行搜尋。 在面板底部，按一下垃圾筒圖示即可看到已刪除的項目。 一開始，您不會從任何資料夾開始，但頂層資料夾的名稱與您的帳戶名稱相同。

>[!NOTE]
>
>垃圾筒中的資產將在放置七天後自動永久刪除，除非您復原。

**瀏覽／建立面板。** 這是UI的中心，您可在此處以「瀏覽」模式瀏覽資產，或在「建立」模式中，將它當做畫布，以在工作流程中建立資產。當您第一次登入時，將會看到「瀏覽面板」。 在畫面中央是格線檢視中影像的縮圖版本。 您可以變更為「清單」檢視，或選取資產，然後使用「詳細資料」檢視檢視其詳細資訊。

>[!IMPORTANT]
>
>每個資產ID旁邊是&#x200B;**標籤為發佈**&#x200B;開關。 當切換為開啟（綠色）時，表示資產已標示為發佈。

![影像](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>在「上傳」對話方塊中選取「上傳後發佈」核取方塊，以便在上傳時自動發佈資產。****

進一步瞭解[瀏覽Dynamic MediaClassic的UI](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/getting-started/navigation-basics.html)。
