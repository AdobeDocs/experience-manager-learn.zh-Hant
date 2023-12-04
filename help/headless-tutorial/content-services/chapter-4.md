---
title: 第4章 — 定義內容服務範本 — 內容服務
description: AEM Headless教學課程的第4章涵蓋AEM Content Services內容中AEM可編輯範本的角色。 可編輯的範本是用來定義AEM Content Services最終公開的JSON內容結構。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 301
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# 第4章 — 定義內容服務範本

AEM Headless教學課程的第4章涵蓋AEM Content Services內容中AEM可編輯範本的角色。 可編輯的範本是用來定義AEM Content Services透過構成啟用AEM元件的方式向使用者端公開的JSON內容結構。

## 瞭解AEM Content Services中範本的角色

AEM可編輯範本用於定義存取的HTTP端點，以將事件內容公開為JSON。

傳統上，使用AEM可編輯範本來定義網頁，不過這僅是慣例。 可編輯的範本可用於撰寫 **任何** 內容集；內容的存取方式：作為瀏覽器中的HTML、JavaScript (AEM SPA Editor)或行動應用程式使用的JSON，是該頁面請求方式的函式。

在AEM Content Services中，可編輯的範本可用來定義JSON資料的公開方式。

對於 [!DNL WKND Mobile] 應用程式，我們將建立單一可編輯範本，用於驅動單一API端點。 雖然此範例只是簡單說明AEM Headless的概念，但您可以建立多個頁面（或端點），每個頁面都會公開不同的內容集，以建立更複雜、更有條理的API。

## 瞭解API端點

瞭解如何組成API端點，以及應該向我們的展示哪些內容 [!DNL WKND Mobile] 應用程式，讓我們重新造訪設計。

![事件API頁面分解](./assets/chapter-4/design-to-component-mapping.png)

如我們所見，我們有三個邏輯內容集可提供給行動應用程式。

1. 此 **標誌**
2. 此 **標籤行**
3. 清單 **活動**

為此，我們可以將這些需求對應至AEM元件(在此例中為AEM WCM核心元件)，以將必要內容公開為JSON。

1. 此 **標誌** 是透過 **影像元件**
2. 此 **標籤行** 是透過 **文字元件**
3. 清單 **活動** 是透過 **內容片段清單元件** 這進而會參考一組事件內容片段。

>[!NOTE]
>
>若要支援AEM Content Service的頁面和元件JSON匯出，頁面和元件必須 **衍生自AEM WCM Core Components**.
>
>[AEM WCM核心元件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 具有內建功能，可支援匯出頁面和元件的標準化JSON架構。 本教學課程使用的所有WKND Mobile元件（頁面、影像、文字和內容片段清單）都衍生自AEM WCM核心元件。

## 定義事件API範本

1. 瀏覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**.

1. 建立 **[!DNL Events API]** 範本：

   1. 點選 **[!UICONTROL 建立]** 在頂端動作列中
   1. 選取 **[!DNL WKND Mobile - Empty Page]** 範本
   1. 點選 **[!UICONTROL 下一個]** 在頂端動作列中
   1. 輸入 **[!DNL Events API]** 在 [!UICONTROL 範本標題] 欄位
   1. 點選 **[!UICONTROL 建立]** 在頂端動作列中
   1. 點選 **[!UICONTROL 開啟]** 開啟新範本以進行編輯

1. 首先，我們允許三個已識別的AEM元件，以透過編輯 [!UICONTROL 原則] 根目錄的 [!UICONTROL 配置容器]. 確保 **[!UICONTROL 結構]** 模式為作用中，選取 **[!DNL Layout Container \[Root\]]**，然後點選 **[!UICONTROL 原則]** 按鈕。
1. 在 **[!UICONTROL 屬性] > [!UICONTROL 允許的元件]** 搜尋 **[!DNL WKND Mobile]**. 允許下列元件來自 [!DNL WKND Mobile] 元件群組，以便用於 [!DNL Events] API頁面。

   * **[!DNL WKND Mobile > Image]**

      * 應用程式的標誌

   * **[!DNL WKND Mobile > Text]**

      * 應用程式的簡介文字

   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在應用程式中顯示的事件類別清單

1. 點選 **[!UICONTROL 完成]** 完成後，右上角勾選記號。
1. **重新整理** 要檢視的瀏覽器視窗 [!UICONTROL 允許的元件] 清單。
1. 從左側欄的「元件」尋找器中，拖曳下列AEM元件：
   1. **[!DNL Image]** 代表標誌
   2. **[!DNL Text]** （標籤行）
   3. **[!DNL Content Fragment List]** 適用於事件
1. **針對上述每個元件**，選取它們並按 **解除鎖定** 按鈕。
1. 不過，請確保 **配置容器** 是 **已鎖定** 以防止新增其他元件，或移除這三個元件。
1. 點選 **[!UICONTROL 頁面資訊] > [!UICONTROL 在管理員中檢視]** 以返回 [!DNL WKND Mobile] 範本清單。 選取新建立的 **[!DNL Events API]** 範本並點選 **[!UICONTROL 啟用]** 在頂端動作列中。

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> 請注意，用於曲面化內容的元件會加入範本本身，並加以鎖定。 這是為了讓作者可編輯預先定義的元件，但不可任意新增或移除元件，因為變更API本身可能會破壞JSON結構的假設，並破壞耗用應用程式。 所有API都必須穩定。

## 後續步驟

或者安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM Author上的內容套件，透過 [AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含本教學課程及先前章節中概述的設定和內容。

* [第5章 — 編寫Content Services頁面](./chapter-5.md)
