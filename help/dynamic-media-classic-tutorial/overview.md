---
title: Dynamic Media Classic最佳實務教學課程
description: Dynamic Media Classic是客戶建立、編寫及提供多媒體內容的中樞。 建立此最佳實務教學課程，是為了協助Dynamic Media Classic的現有和新使用者更完全瞭解他們可以使用Adobe的這個強大的多媒體解決方案做什麼。 在本教學課程的這個部分，您將瞭解Dynamic Media Classic是什麼，並取得其核心功能和使用者介面的簡短介紹。
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 975b85af-ca6a-419e-ab2a-6e1781bfee4a
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '885'
ht-degree: 0%

---

# Dynamic Media Classic最佳實務教學課程

本指南旨在協助Dynamic Media Classic的現有和新使用者更完全瞭解他們可以使用Adobe提供的強大多媒體解決方案做什麼。 我們將透過以下方式達成此目的：

- 向您介紹Dynamic Media Classic，說明其內容，並提供其核心功能和使用者介面(UI)的概觀。
- 說明在解決方案中使用資產時，您會遵循的一般建立、編寫和傳遞工作流程。
- 討論重要專案以在跳入並使用解決方案之前進行設定。
- 深入瞭解如何使用解決方案的幾項核心功能。

在本指南中，我們將提供範例、提示和最佳實務。 我們也會說明您在使用Dynamic Media Classic時應該熟悉的重要辭彙和概念。 如果有特定主題的相關資訊，我們會將您導向相關的網路研討會、部落格和線上檔案。

我們希望本指南能提供您所需的資訊，協助您從Dynamic Media Classic解決方案中釋放巨大價值。 若要更輕鬆地瀏覽本指南的章節，請按一下指南左側的書籤圖示以檢視其內容。

## Dynamic Media Classic概觀

Dynamic Media Classic是客戶建立、編寫及提供多媒體內容的中樞。 Dynamic Media Classic是一個整合式多媒體管理、發佈和服務環境。 豐富型媒體可傳送至所有行銷和銷售管道，包括網頁、列印資料、電子郵件行銷活動、網頁應用程式、桌上型電腦和裝置。

影像伺服可能是Dynamic Media Classic最常使用的功能。 事實上，大部分客戶都使用Dynamic Media Classic來提供其網站上的所有影像，包括縮放或多媒體影像。 不過，它也可用於許多其他用途，包括傳送視訊和使用AI最佳化傳送的影像。

## Dynamic Media Classic的核心功能

在本指南中，我們將討論Dynamic Media Classic的下列核心功能。

- **Dynamic Imaging。** 此傘狀辭彙用於即時編輯、格式化及調整大小，以及互動式縮放和平移；顏色和紋理色票；360度旋轉；影像範本；以及多媒體檢視器。
- **影片.** 上傳最終影片、發佈影片，並逐步下載至可設定的影片檢視器。
- **智慧型影像.** 這項技術運用Adobe Sensei AI功能，並與現有的「影像預設集」搭配使用，根據使用者端瀏覽器功能自動最佳化影像格式、大小和品質，進而增強影像傳送效能。

若要瞭解解決方案的其他功能，請造訪 [Dynamic Media Classic檔案](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/intro/introduction.html).

## Dynamic Media Classic使用者介面(UI)

Dynamic Media Classic主要UI包含三個主要區域：全域導覽列、資產庫和瀏覽面板/組建面板。

![影像](assets/overview/overview-dmc-ui-ew.png)

_DYNAMIC MEDIA CLASSIC UI_

**全域導覽列。** 位於熒幕頂端，您將使用此列上的按鈕來存取解決方案的關鍵區域和功能。 例如，您可用它來存取上傳功能、開啟各種資產建置區域（影像集、迴轉集等）、執行重要工作，例如設定影像預設集和檢視器預設集，以及發佈您的資產。 您也可以在此處監視作業、檢視最近的活動，以及選擇各種說明選項。

**資產庫.** 「資產庫」位於畫面左側，該面板可用來將資產整理在您建立的資料夾和子資料夾中。 在面板頂端，您可以找到搜尋和篩選器，協助您尋找資產。 進階搜尋可讓您指定多個選項作為搜尋條件，包括附加至該資產的隱藏中繼資料欄位，以進行搜尋。 在面板底部，您可以按一下垃圾桶圖示來檢視已刪除的專案。 最初，您不會從任何資料夾開始，但頂層資料夾除外，其名稱與您的帳戶名稱相同。

>[!NOTE]
>
>除非您還原，否則系統會在資產放入垃圾桶七天後，自動將其永久刪除。

**瀏覽/建立面板。** 這是UI的中央，您可在此在「瀏覽」模式下瀏覽資產，或在「建置」模式中，您可將其用作工作流程中建置資產的畫布。 第一次登入時，您會看到「瀏覽」面板。 在熒幕中央是格線檢視中影像的縮圖版本。 您可以變更為「清單」檢視，或選取資產，並使用「詳細資訊」檢視來檢視其詳細資訊。

>[!IMPORTANT]
>
>在每個資產ID旁邊， **標籤為發佈** 開關。 當切換開啟（綠色）時，表示資產已標示為發佈。

![影像](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>選取 **上傳後發佈** 「上傳」對話方塊中的核取方塊，可在上傳時自動發佈資產。

進一步瞭解 [導覽Dynamic Media Classic的UI](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/navigation-basics.html).
