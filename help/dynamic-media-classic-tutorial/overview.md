---
title: 歡迎使用Dynamic Media Classic最佳作法教學課程
description: Dynamic Media Classic是客戶建立、撰寫和傳送多媒體內容的中心。 本最佳實務教學課程旨在協助Dynamic Media Classic的目前和新使用者，更全面了解他們可以透過Adobe運用這項功能強大的多媒體解決方案做些什麼。 在本教學課程中，您將了解Dynamic Media Classic是什麼，並簡要了解其核心功能和使用者介面。
sub-product: dynamic-media
doc-type: tutorial
topics: best-practices, development, authoring, configuring
audience: all
activity: develop, use
feature: Dynamic Media Classic
topic: 內容管理
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 0%

---


# 歡迎使用Dynamic Media Classic最佳作法教學課程

本指南旨在協助Dynamic Media Classic的目前和新使用者，更全面了解他們可以透過Adobe運用其強大的多媒體解決方案做些什麼。 我們將通過以下方式完成：

- 為您介紹Dynamic Media Classic，說明其內容，並概略說明其核心功能和使用者介面(UI)。
- 說明在解決方案中使用資產時，您將遵循的一般「建立」、「撰寫」和「傳送」工作流程。
- 在跳入和使用解決方案之前討論要設定的重要項目。
- 深入探討如何使用數個解決方案的核心功能。

在本指南中，我們將提供範例、秘訣和最佳實務。 我們也會說明使用Dynamic Media Classic時應該熟悉的重要術語和概念。 當特定主題可用時，我們會將您引導至相關的網路研討會、部落格文章和線上檔案。

我們希望本指南能提供您從Dynamic Media Classic解決方案中充分發揮巨大價值所需的資訊。 若要更輕鬆導覽本指南的章節，請按一下指南左側的書籤圖示，以查看其內容。

## Dynamic Media Classic概觀

Dynamic Media Classic是客戶建立、撰寫和傳送多媒體內容的中心。 Dynamic Media Classic是整合式多媒體管理、發佈和服務環境。 多媒體可以傳遞到所有營銷和銷售渠道，包括Web 、打印材料、電子郵件宣傳、Web應用程式、案頭和設備。

影像提供也許是Dynamic Media Classic最常使用的功能。 事實上，大部分的客戶都是使用Dynamic Media Classic來提供其網站上的所有影像，包括影像以供縮放或多媒體。 不過，它也可用於其他許多用途，包括視訊傳送和使用AI來最佳化傳送的影像。

## Dynamic Media Classic的核心功能

在本指南中，我們將討論下列Dynamic Media Classic的核心功能。

- **Dynamic Imaging。** 即時編輯、格式化和尺寸調整、互動式縮放和平移的傘形術語；顏色和紋理切換；旋轉360度；影像模板；和多媒體觀眾。
- **影片.** 上傳最終影片、發佈，並逐步將它們下載至可設定的影片檢視器中。
- **智慧型影像.** 運用Adobe Sensei AI功能並與現有的「影像預設集」搭配使用的技術，可根據用戶端瀏覽器功能自動最佳化影像格式、大小和品質，以增強影像傳送效能。

若要探索解決方案的其他功能，請造訪[Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/intro/introduction.html)的檔案。

## Dynamic Media Classic使用者介面(UI)

Dynamic Media Classic主要UI包含三個主要區域：全域導覽列、資產庫和瀏覽面板/建置面板。

![影像](assets/overview/overview-dmc-ui-ew.png)

_Dynamic Media傳統UI_

**全域導覽列。** 位於螢幕頂部，您將使用此欄上的按鈕來訪問解決方案的關鍵區域和功能。例如，您將可使用它來存取上傳功能、開啟各種資產建置區域（影像集、回轉集等）、執行重要工作，例如設定影像預設集和檢視器預設集，以及發佈您的資產。 您也可以在此監控工作、查看最近的活動，以及從各種說明選項中選擇。

**資產庫.** 畫面左側的是「資產資料庫」，此面板可用來在您建立的資料夾和子資料夾中組織資產。面板頂端會提供搜尋和篩選器，協助您找出資產。 「進階搜尋」可讓您指定多個選項作為搜尋的條件，包括附加至該資產的隱藏中繼資料欄位，以利搜尋。 在面板底部，按一下垃圾桶圖示即可查看已刪除的項目。 起初，您不會從任何資料夾開始操作，但頂層資料夾的名稱與帳戶名稱相同。

>[!NOTE]
>
>垃圾筒中的資產放置七天後，除非您還原，否則會自動永久刪除這些資產。

**瀏覽/建置面板。** 這是UI的中心，您會在「瀏覽」模式中瀏覽資產，或在「建置」模式中，您會將其用作畫布，以在工作流程中建置資產。第一次登入時，您會看到「瀏覽」面板。 畫面中央是格線檢視中影像的縮圖版本。 您可以變更為「清單」檢視，或選取資產，並使用「詳細資料」檢視檢視其詳細資訊。

>[!IMPORTANT]
>
>每個資產ID旁邊都是&#x200B;**Mark for Publish**&#x200B;開關。 切換為開啟（綠色）時，表示資產已標示為要發佈。

![影像](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>在「上傳」對話方塊中選取「**上傳後發佈」核取方塊，以便在上傳時自動發佈資產。**

深入了解[導覽Dynamic Media Classic的UI](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/getting-started/navigation-basics.html)。
