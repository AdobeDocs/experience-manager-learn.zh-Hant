---
title: Dynamic Media Classic最佳做法教程
description: Dynamic Media Classic是客戶建立、創作和交付富媒體內容的中心。 本最佳實踐教程旨在幫助Dynamic Media Classic的當前和新用戶更全面地瞭解他們可以利用Adobe提供的這一功能強大的富媒體解決方案做些什麼。 在本教程的這一部分，您將瞭解Dynamic Media Classic是什麼，並簡要瞭解其核心功能和用戶介面。
doc-type: tutorial
audience: all
activity: develop, use
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
exl-id: 975b85af-ca6a-419e-ab2a-6e1781bfee4a
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '885'
ht-degree: 0%

---

# Dynamic Media Classic最佳做法教程

本指南旨在幫助Dynamic Media Classic的當前和新用戶更全面地瞭解他們可以利用Adobe的強大富媒體解決方案做些什麼。 我們將通過以下方式完成：

- 向您介紹Dynamic Media Classic，介紹其內容，並概述其核心功能和用戶介面(UI)。
- 在解決方案中使用資產時，將遵循的常規「建立」、「建立」和「提交」工作流。
- 討論在跳入和使用解決方案之前要設定的重要項目。
- 深入探討如何使用解決方案的幾項核心功能。

在整個指南中，我們將提供示例、提示和最佳實踐。 我們還將解釋您在與Dynamic Media Classic合作時應熟悉的重要術語和概念。 如果您可以參與某個特定主題，我們將向您介紹相關的網路研討會、部落格和線上文檔。

我們希望本指南為您提供您從您的Dynamic Media Classic解決方案中獲得巨大價值所需的資訊。 要更輕鬆地瀏覽本指南的各章，請按一下指南左側的書籤表徵圖以查看其內容。

## Dynamic Media Classic概述

Dynamic Media Classic是客戶建立、創作和交付富媒體內容的中心。 Dynamic Media Classic是一個整合的富媒體管理、出版和服務環境。 富媒體可以提供到所有營銷和銷售渠道，包括Web 、打印材料、電子郵件活動、Web應用程式、案頭和設備。

影像服務也許是Dynamic Media Classic最常用的功能。 事實上，大多數客戶都使用Dynamic Media Classic來提供其網站上的所有影像，包括縮放影像或富媒體影像。 但是，它也可用於許多其他目的，包括視頻的傳送和使用人工智慧來優化傳送的影像。

## Dynamic Media Classic的核心能力

在本指南中，我們將討論以下Dynamic Media Classic的核心功能。

- **Dynamic Imaging。** 即時編輯、格式化和尺寸調整、互動式縮放和平移的傘形術語；顏色和紋理切換；360度旋轉；影像模板；多媒體觀眾。
- **影片.** 上載最終視頻、發佈它們，並逐步將它們下載到可配置的視頻查看器中。
- **智慧型影像.** 利用Adobe SenseiAI功能並與現有的&quot;影像預設&quot;協作的技術，通過基於客戶瀏覽器功能自動優化影像格式、大小和質量來增強影像傳送效能。

要瞭解解決方案的其他功能，請訪問 [Dynamic Media Classic檔案](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/intro/introduction.html)。

## Dynamic Media Classic用戶介面(UI)

Dynamic Media Classic主要用戶介面包括三個主要領域：全局導航欄、資產庫和瀏覽面板/生成面板。

![影像](assets/overview/overview-dmc-ui-ew.png)

_Dynamic Media ClassicUI_

**全局導航欄。** 位於螢幕頂部，您將使用此欄上的按鈕訪問解決方案的關鍵區域和功能。 例如，您將使用它訪問上載功能、開啟各種資產構建區域（影像集、旋轉集等）、執行重要任務，如設定影像預設和查看器預設，以及發佈您的資產。 在此，您還可以監視您的工作、查看最近的活動以及從各種幫助選項中進行選擇。

**資產庫.** 螢幕左側是「資產庫」，此面板用於組織您建立的資料夾和子資料夾中的資產。 在面板頂部，您將找到搜索和篩選器以幫助您查找資產。 「高級搜索」允許您通過指定多個選項作為搜索的條件進行搜索，包括附加到該資產的隱藏元資料欄位。 在面板底部，按一下「資源回收筒」表徵圖可以看到已刪除的項目。 最初，您不會從任何資料夾開始，但頂級資料夾與帳戶名同名。

>[!NOTE]
>
>垃圾箱中的資產將在放置七天後自動被永久刪除，除非您還原。

**瀏覽/生成面板。** 這是UI的中心，在該中，您將在「瀏覽」模式下瀏覽資產，或者，如果在「生成」模式下，將其用作作為工作流的一部分構建資產的畫布。 首次登錄時，將看到「瀏覽面板」。 在螢幕的中心是「網格」視圖中影像的縮略圖版本。 您可以更改為「清單」視圖，或選擇資產並使用「詳細資訊」視圖查看資產的詳細資訊。

>[!IMPORTANT]
>
>每個資產ID旁邊是 **標籤為發佈** 按鈕 當切換開啟（綠色）時，表示資產被標籤為發佈。

![影像](assets/overview/overview-mark-for-publish.png)

>[!TIP]
>
>選擇 **上載後發佈** 複選框以在上載時自動發佈資產。

瞭解有關 [瀏覽Dynamic Media Classic的用戶介面](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/getting-started/navigation-basics.html)。
