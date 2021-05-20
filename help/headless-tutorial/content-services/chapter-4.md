---
title: 第4章 — 定義內容服務範本 — 內容服務
seo-title: AEM Headless快速入門 — 第4章 — 定義內容服務範本
description: AEM Headless教學課程的第4章涵蓋AEM可編輯範本在AEM Content Services內容中的角色。 可編輯的範本可用來定義JSON內容結構AEM Content Services最終將公開。
seo-description: AEM Headless教學課程的第4章涵蓋AEM可編輯範本在AEM Content Services內容中的角色。 可編輯的範本可用來定義JSON內容結構AEM Content Services最終將公開。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 0%

---


# 第4章 — 定義內容服務範本

AEM Headless教學課程的第4章涵蓋AEM可編輯範本在AEM Content Services內容中的角色。 可編輯的範本可用來定義AEM Content Services透過組成已啟用Content Services的AEM元件而公開給用戶端的JSON內容結構。

## 了解範本在AEM Content Services中的角色

AEM可編輯的範本可用來定義將被存取以將事件內容公開為JSON的HTTP端點。

傳統上，AEM可編輯範本是用來定義網頁，但此用途只是慣例。 可編輯的範本可用來撰寫&#x200B;**任何**&#x200B;內容集；如何存取該內容：瀏覽器中的HTML、JavaScript(AEM SPA編輯器)使用的JSON或行動應用程式是請求該頁面的方式的函式。

在AEM Content Services中，可編輯的範本可用來定義JSON資料公開的方式。

對於[!DNL WKND Mobile]應用程式，我們將建立單一可編輯範本，以用於驅動單一API端點。 雖然此範例可簡單說明AEM Headless的概念，但您可以分別建立多個頁面（或端點），以顯示不同的內容集，以建立更複雜且組織更妥善的API。

## 了解API端點

若要了解如何組成API端點，並了解應向[!DNL WKND Mobile]應用程式公開哪些內容，請讓我們重新審視設計。

![事件API頁面分解](./assets/chapter-4/design-to-component-mapping.png)

如我們所見，我們有三組邏輯內容可提供給行動應用程式。

1. **標誌**
2. **標籤行**
3. **Events**&#x200B;的清單

若要這麼做，我們可以將這些需求對應至AEM元件(在我們的案例中為AEM WCM核心元件)，以將必要的內容公開為JSON。

1. **標誌**&#x200B;將通過&#x200B;**影像元件**&#x200B;呈現
2. **標籤行**&#x200B;將通過&#x200B;**文本元件**&#x200B;呈現
3. **事件**&#x200B;的清單將通過&#x200B;**內容片段清單元件**&#x200B;呈現，該元件又引用一組事件內容片段。

>[!NOTE]
>
>若要支援AEM內容服務的JSON匯出頁面和元件，頁面和元件必須&#x200B;**衍生自AEM WCM核心元件**。
>
>[AEM WCM核心元](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 件提供內建功能，支援匯出頁面和元件的標準化JSON結構描述。本教學課程中使用的所有WKND Mobile元件（頁面、影像、文字和內容片段清單）皆衍生自AEM WCM核心元件。

## 定義事件API範本

1. 導覽至&#x200B;**[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**。

1. 建立&#x200B;**[!DNL Events API]**&#x200B;範本：

   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 建立]**
   1. 選擇&#x200B;**[!DNL WKND Mobile - Empty Page]**&#x200B;模板
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL Next]**
   1. 在[!UICONTROL 範本標題]欄位中輸入&#x200B;**[!DNL Events API]**
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 建立]**
   1. 點選&#x200B;**[!UICONTROL 開啟]**&#x200B;開啟新範本以進行編輯

1. 首先，我們允許我們需要的三個已識別的AEM元件通過編輯根[!UICONTROL 配置容器]的[!UICONTROL Policy]來模型內容。 確保&#x200B;**[!UICONTROL Structure]**&#x200B;模式處於活動狀態，選擇&#x200B;**[!DNL Layout Container \[Root\]]**，然後點選&#x200B;**[!UICONTROL Policy]**&#x200B;按鈕。
1. 在「**[!UICONTROL 屬性] > [!UICONTROL 允許的元件]**」下搜索&#x200B;**[!DNL WKND Mobile]**。 允許從[!DNL WKND Mobile]元件組中使用以下元件，以便在[!DNL Events] API頁上使用。

   * **[!DNL WKND Mobile > Image]**

      * 應用程式的標誌
   * **[!DNL WKND Mobile > Text]**

      * 應用程式的介紹文本
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在應用程式中顯示的事件類別清單



1. 完成後，點選右上角的&#x200B;**[!UICONTROL Done]**&#x200B;勾選記號。
1. **** 重新整理瀏覽器視窗，以在左 [!UICONTROL 側] 邊欄中查看新的允許元件清單。
1. 從左側邊欄的「元件尋找器」中，拖曳下列AEM元件：
   1. **[!DNL Image]** 標誌
   2. **[!DNL Text]** 標籤行
   3. **[!DNL Content Fragment List]** 針對事件
1. **對於上述各元件**，選取它們並按解鎖 **** 按鈕。
1. 不過，請確定&#x200B;**版面容器**&#x200B;為&#x200B;**已鎖定**，以防止新增其他元件或移除這三個元件。
1. 點選&#x200B;**[!UICONTROL 頁面資訊] > [!UICONTROL 在管理員]**&#x200B;中檢視，以返回[!DNL WKND Mobile]範本清單。 選取新建立的&#x200B;**[!DNL Events API]**&#x200B;範本，然後點選頂端動作列中的&#x200B;**[!UICONTROL 啟用]**。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 請注意，用於呈現內容的元件將添加到「模板」(Template)本身並鎖定。 這可讓作者編輯預先定義的元件，但不能任意新增或移除元件，因為變更API本身可能會破壞JSON結構的假設，並破壞使用中的應用程式。 所有API都必須穩定。

## 後續步驟

選擇性地，透過[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 此套件包含本教學課程及前幾章中概述的設定和內容。

* [第5章 — 編寫內容服務頁面](./chapter-5.md)
