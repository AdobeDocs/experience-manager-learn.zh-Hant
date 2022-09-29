---
title: 第4章 — 定義內容服務範本 — 內容服務
description: AEM Headless教學課程的第4章涵蓋AEM可編輯範本在AEM Content Services內容中的角色。 可編輯的範本可用來定義最終公開的JSON內容結構AEM Content Services。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 第4章 — 定義內容服務範本

AEM Headless教學課程的第4章涵蓋AEM可編輯範本在AEM Content Services內容中的角色。 可編輯的範本可用來定義AEM Content Services透過組成已啟用Content Services的AEM元件而公開給用戶端的JSON內容結構。

## 了解範本在AEM Content Services中的角色

AEM可編輯的範本可用來定義所存取的HTTP端點，以將事件內容公開為JSON。

傳統上，AEM可編輯範本是用來定義網頁，但此用途只是慣例。 可編輯的範本可用於撰寫 **any** 內容集；如何存取該內容：瀏覽器中的HTML，如同JavaScript(AEM SPA編輯器)或行動應用程式使用的JSON，則取決於該頁面的請求方式。

在AEM Content Services中，可編輯的範本可用來定義JSON資料公開的方式。

若 [!DNL WKND Mobile] 應用程式中，我們將建立單一可編輯範本，用於驅動單一API端點。 雖然此範例可簡單說明AEM Headless的概念，但您可以分別建立多個頁面（或端點），以顯示不同的內容集，以建立更複雜且組織更妥善的API。

## 了解API端點

若要了解如何撰寫API端點，並了解應向我們的 [!DNL WKND Mobile] 應用，讓我們重溫設計。

![事件API頁面分解](./assets/chapter-4/design-to-component-mapping.png)

如我們所見，我們有三組邏輯內容可提供給行動應用程式。

1. 此 **標誌**
2. 此 **標籤行**
3. 清單 **事件**

若要這麼做，我們可以將這些需求對應至AEM元件(在我們的案例中為AEM WCM核心元件)，以將必要的內容公開為JSON。

1. 此 **標誌** 透過 **影像元件**
2. 此 **標籤行** 透過 **文字元件**
3. 清單 **事件** 透過 **內容片段清單元件** 反之，會參考一組事件內容片段。

>[!NOTE]
>
>若要支援AEM內容服務的JSON匯出頁面和元件，頁面和元件必須 **衍生自AEM WCM核心元件**.
>
>[AEM WCM核心元件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 已內建功能，可支援已匯出頁面和元件的標準化JSON結構。 本教學課程中使用的所有WKND Mobile元件（頁面、影像、文字和內容片段清單）皆衍生自AEM WCM核心元件。

## 定義事件API範本

1. 導覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**.

1. 建立 **[!DNL Events API]** 範本：

   1. 點選 **[!UICONTROL 建立]** 在頂端動作列中
   1. 選取 **[!DNL WKND Mobile - Empty Page]** 範本
   1. 點選 **[!UICONTROL 下一個]** 在頂端動作列中
   1. 輸入 **[!DNL Events API]** 在 [!UICONTROL 範本標題] 欄位
   1. 點選 **[!UICONTROL 建立]** 在頂端動作列中
   1. 點選 **[!UICONTROL 開啟]** 開啟新範本以進行編輯

1. 首先，我們允許我們需要的三個已識別AEM元件，透過編輯 [!UICONTROL 原則] 根 [!UICONTROL 版面容器]. 確保 **[!UICONTROL 結構]** 模式為活動狀態，請選擇 **[!DNL Layout Container \[Root\]]**，然後點選 **[!UICONTROL 原則]** 按鈕。
1. 在 **[!UICONTROL 屬性] > [!UICONTROL 允許的元件]** 搜尋 **[!DNL WKND Mobile]**. 允許下列元件，從 [!DNL WKND Mobile] 元件群組，以便用於 [!DNL Events] API頁面。

   * **[!DNL WKND Mobile > Image]**

      * 應用程式的標誌
   * **[!DNL WKND Mobile > Text]**

      * 應用程式的介紹文本
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在應用程式中顯示的事件類別清單



1. 點選 **[!UICONTROL 完成]** 完成後，在右上角勾選「 」。
1. **重新整理** 可查看新 [!UICONTROL 允許的元件] 清單。
1. 從左側邊欄的「元件尋找器」中，拖曳下列AEM元件：
   1. **[!DNL Image]** 標誌
   2. **[!DNL Text]** 標籤行
   3. **[!DNL Content Fragment List]** 針對事件
1. **針對上述各元件**，選取並按下 **解除鎖定** 按鈕。
1. 不過，請確保 **配置容器** is **鎖定** 以防止新增其他元件，或移除這三個元件。
1. 點選 **[!UICONTROL 頁面資訊] > [!UICONTROL 在管理中檢視]** 返回 [!DNL WKND Mobile] 範本清單。 選取新建立的 **[!DNL Events API]** 範本和點選 **[!UICONTROL 啟用]** 在頂端動作列。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 請注意，用於呈現內容的元件將添加到「模板」(Template)本身並鎖定。 這可讓作者編輯預先定義的元件，但不能任意新增或移除元件，因為變更API本身可能會破壞JSON結構的假設，並破壞使用中的應用程式。 所有API都必須穩定。

## 後續步驟

（可選）安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的內容套件(透過 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含本教學課程及前幾章中概述的設定和內容。

* [第5章 — 編寫內容服務頁面](./chapter-5.md)
