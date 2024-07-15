---
title: 第4章 — 定義內容服務範本 — 內容服務
description: AEM Headless教學課程的第4章涵蓋AEM Content Services內容中AEM可編輯範本的角色。 可編輯的範本是用來定義AEM Content Services最終公開的JSON內容結構。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 245
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# 第4章 — 定義內容服務範本

AEM Headless教學課程的第4章涵蓋AEM Content Services內容中AEM可編輯範本的角色。 可編輯的範本是用來定義AEM Content Services透過構成啟用AEM元件的方式向使用者端公開的JSON內容結構。

## 瞭解AEM Content Services中範本的角色

AEM可編輯範本用於定義存取的HTTP端點，以將事件內容公開為JSON。

傳統上，AEM會使用可編輯的範本來定義網頁，不過這僅是慣例。 可編輯的範本可用來構成&#x200B;**任何**&#x200B;內容集；內容的存取方式：在瀏覽器中作為HTML、作為JavaScript (AEM SPA Editor)使用的JSON或行動應用程式，是該頁面請求方式的函式。

在AEM Content Services中，可編輯的範本可用來定義JSON資料的公開方式。

我們將針對[!DNL WKND Mobile]應用程式建立單一可編輯的範本，用來驅動單一API端點。 雖然此範例只是簡單說明AEM Headless的概念，但您可以建立多個頁面（或端點），每個頁面都會公開不同的內容集，以建立更複雜、更有條理的API。

## 瞭解API端點

若要瞭解如何撰寫我們的API端點，以及應該向我們的[!DNL WKND Mobile]應用程式公開哪些內容，請讓我們重新造訪設計。

![事件API頁面分解](./assets/chapter-4/design-to-component-mapping.png)

如我們所見，我們有三個邏輯內容集可提供給行動應用程式。

1. **標誌**
2. **標籤行**
3. **事件**&#x200B;的清單

為此，我們可以將這些需求對應至AEM元件(在此例中為AEM WCM核心元件)，以將必要內容公開為JSON。

1. **標誌**&#x200B;是透過&#x200B;**影像元件**&#x200B;顯示
2. **標籤行**&#x200B;是透過&#x200B;**文字元件**&#x200B;顯示
3. **事件**&#x200B;的清單透過&#x200B;**內容片段清單元件**&#x200B;顯示，而該元件又參考一組事件內容片段。

>[!NOTE]
>
>若要支援AEM Content Service的頁面和元件JSON匯出，頁面和元件必須&#x200B;**衍生自AEM WCM核心元件**。
>
>[AEM WCM核心元件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components)已內建功能，可支援已匯出頁面和元件的標準化JSON結構描述。 本教學課程使用的所有WKND Mobile元件（頁面、影像、文字和內容片段清單）都衍生自AEM WCM核心元件。

## 定義事件API範本

1. 導覽至&#x200B;**[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**。

1. 建立&#x200B;**[!DNL Events API]**&#x200B;範本：

   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 建立]**
   1. 選取&#x200B;**[!DNL WKND Mobile - Empty Page]**&#x200B;範本
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 下一步]**
   1. 在[!UICONTROL 範本標題]欄位中輸入&#x200B;**[!DNL Events API]**
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 建立]**
   1. 點選&#x200B;**[!UICONTROL 開啟]**&#x200B;開啟新範本以進行編輯

1. 首先，我們允許三個已識別的AEM元件，以編輯根[!UICONTROL 配置容器]的[!UICONTROL 原則]，為內容建模。 請確定&#x200B;**[!UICONTROL 結構]**&#x200B;模式為作用中，選取&#x200B;**[!DNL Layout Container \[Root\]]**，然後點選&#x200B;**[!UICONTROL 原則]**&#x200B;按鈕。
1. 在&#x200B;**[!UICONTROL 屬性] > [!UICONTROL 允許的元件]**&#x200B;下，搜尋&#x200B;**[!DNL WKND Mobile]**。 允許[!DNL WKND Mobile]元件群組中的下列元件，以便在[!DNL Events] API頁面上使用。

   * **[!DNL WKND Mobile > Image]**

      * 應用程式的標誌

   * **[!DNL WKND Mobile > Text]**

      * 應用程式的簡介文字

   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在應用程式中顯示的事件類別清單

1. 完成時，點選右上角的&#x200B;**[!UICONTROL 完成]**&#x200B;核取記號。
1. **重新整理**&#x200B;瀏覽器視窗，以在左側邊欄中檢視新的[!UICONTROL 允許的元件]清單。
1. 從左側欄的「元件」尋找器中，拖曳下列AEM元件：
   1. 徽標的&#x200B;**[!DNL Image]**
   2. 標籤行的&#x200B;**[!DNL Text]**
   3. 事件的&#x200B;**[!DNL Content Fragment List]**
1. **針對上述每個元件**，選取它們並按下&#x200B;**解除鎖定**&#x200B;按鈕。
1. 不過，請確定&#x200B;**配置容器**&#x200B;為&#x200B;**鎖定**，以防止新增其他元件，或移除這三個元件。
1. 點選「**[!UICONTROL 頁面資訊] > [!UICONTROL 以管理員檢視]**」以返回[!DNL WKND Mobile]範本清單。 選取新建立的&#x200B;**[!DNL Events API]**&#x200B;範本，然後點選頂端動作列中的&#x200B;**[!UICONTROL 啟用]**。

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> 請注意，用於曲面化內容的元件會加入範本本身，並加以鎖定。 這是為了讓作者可編輯預先定義的元件，但不可任意新增或移除元件，因為變更API本身可能會破壞JSON結構的假設，並破壞耗用應用程式。 所有API都必須穩定。

## 後續步驟

可選擇透過[AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)在AEM Author上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容封裝。 此套件包含本教學課程及先前章節中概述的設定和內容。

* [第5章 — 編寫Content Services頁面](./chapter-5.md)
