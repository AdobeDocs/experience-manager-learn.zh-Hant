---
title: 第4章 — 定義Content Services模板 — Content Services
description: 「無頭」教AEM程的第4章介紹AEM了Content Services上下文中可編輯模AEM板的角色。 可編輯模板用於定義JSON內容結構Content Services最AEM終公開的內容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 第4章 — 定義Content Services模板

「無頭」教AEM程的第4章介紹AEM了Content Services上下文中可編輯模AEM板的角色。 可編輯模板用於定義Content Services通過啟用Content Services的組AEM成向客戶端公開的JSON內容結AEM構。

## 瞭解模板在Content Services中AEM的角色

可編AEM輯模板用於定義訪問的HTTP端點，以將事件內容顯示為JSON。

傳統AEM上，可編輯模板用於定義網頁，但此用法只是慣例。 可編輯模板可用於合成 **任何** 內容集；訪問該內容的方式：作為瀏覽器中的HTML,JavaScript(AEMSPA Editor)或Mobile App使用的JSON是請求該頁的方式的函式。

在AEMContent Services中，可編輯模板用於定義JSON資料的顯示方式。

對於 [!DNL WKND Mobile] 應用程式，我們將建立一個用於驅動單個API終結點的可編輯模板。 雖然此示例簡單地說明了「無頭」的概念，但您可以建立多個頁面（或端點），每個頁面都會顯示不同的內容集，以建立更複雜、組織更好的API。

## 瞭解API端點

瞭解如何構成我們的API端點，並瞭解應向我們公開哪些內容 [!DNL WKND Mobile] 應用，讓我們重溫設計。

![事件API頁分解](./assets/chapter-4/design-to-component-mapping.png)

正如我們所看到的，我們有三組邏輯內容要提供給移動應用。

1. 的 **標識**
2. 的 **標籤行**
3. 清單 **事件**

為此，我們可以將這些要求映射AEM到元件(在我們的例子中為AEMWCM核心元件)，以便將必需內容顯示為JSON。

1. 的 **標識** 通過 **影像元件**
2. 的 **標籤行** 通過 **文本元件**
3. 清單 **事件** 通過 **內容片段清單元件** 反過來，引用一組事件內容片段。

>[!NOTE]
>
>要支AEM持Content Service的JSON頁面和元件導出，頁面和元件必須 **衍生自AEMWCM核心元件**。
>
>[AEMWCM核心元件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 具有內置功能，可支援導出的頁和元件的規範化JSON架構。 本教程中使用的所有WKND Mobile元件（頁面、影像、文本和內容片段清單）都源自AEMWCM核心元件。

## 定義事件API模板

1. 導航到 **[!UICONTROL 工具] > [!UICONTROL 常規] > [!UICONTROL 模板] >[!DNL WKND Mobile]**。

1. 建立 **[!DNL Events API]** 模板：

   1. 點擊 **[!UICONTROL 建立]** 的子菜單。
   1. 選擇 **[!DNL WKND Mobile - Empty Page]** 模板
   1. 點擊 **[!UICONTROL 下一個]** 的子菜單。
   1. 輸入 **[!DNL Events API]** 的 [!UICONTROL 模板標題] 場
   1. 點擊 **[!UICONTROL 建立]** 的子菜單。
   1. 點擊 **[!UICONTROL 開啟]** 開啟新模板進行編輯

1. 首先，我們允許需要的三AEM個已標識的元件通過編輯 [!UICONTROL 策略] 根 [!UICONTROL 佈局容器]。 確保 **[!UICONTROL 結構]** 模式處於活動狀態，選擇 **[!DNL Layout Container \[Root\]]**，然後按一下 **[!UICONTROL 策略]** 按鈕
1. 下 **[!UICONTROL 屬性] > [!UICONTROL 允許的元件]** 搜索 **[!DNL WKND Mobile]**。 允許以下元件 [!DNL WKND Mobile] 元件組，以便在 [!DNL Events] API頁。

   * **[!DNL WKND Mobile > Image]**

      * 應用的徽標
   * **[!DNL WKND Mobile > Text]**

      * 應用的介紹性文本
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在應用中顯示的事件類別清單



1. 點擊 **[!UICONTROL 完成]** 完成時在右上角選中標籤。
1. **刷新** 瀏覽器窗口，以查看新 [!UICONTROL 允許的元件] 清單。
1. 從左滑軌的「元件查找器」中，拖動以下「元件AEM」：
   1. **[!DNL Image]** 標識
   2. **[!DNL Text]** 標籤行
   3. **[!DNL Content Fragment List]** 活動
1. **對於上述每個元件**，選擇它們，然後按 **解鎖** 按鈕
1. 但是，確保 **佈局容器** 是 **鎖定** 以防止添加其他元件或刪除這三個元件。
1. 點擊 **[!UICONTROL 頁面資訊] > [!UICONTROL 在管理中查看]** 返回 [!DNL WKND Mobile] 模板清單。 選擇新建立的 **[!DNL Events API]** 模板和點擊 **[!UICONTROL 啟用]** 按鈕。

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> 請注意，用於曲面內容的元件將添加到「模板」(Template)本身並被鎖定。 這允許作者編輯預定義的元件，但不能任意添加或刪除元件，因為更改API本身可能會打破JSON結構的假設並破壞使用的應用。 所有API都需要穩定。

## 後續步驟

（可選）安裝 [com.adobe.aem.guides.wknd-mobile content-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 通過 [包管AEM理器](http://localhost:4502/crx/packmgr/index.jsp)。 此軟體包包含本教程及前面各章中概述的配置和內容。

* [第5章 — 創作Content Services頁](./chapter-5.md)
