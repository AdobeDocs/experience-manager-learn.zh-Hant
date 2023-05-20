---
title: 第5章 — 創作Content Services頁 — Content Services
description: 「無頭」教AEM程的第5章涉及從第4章定義的模板建立頁面。 這些頁將充當JSON HTTP端點。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# 第5章 — 創作Content Services頁

「無頭」教程AEM的第5章涉及從第4章中定義的模板建立頁面。 本章中建立的頁面將充當Mobile App的JSON HTTP端點。

>[!NOTE]
>
> 頁面內容體系結構 `/content/wknd-mobile/en/api` 已經預建好了。 基頁 `en` 和 `api` 服務於架構和組織目的，但並不嚴格要求。 如果API內容可以本地化，則最好遵循通常的「語言複製」和「多站點管理器」頁面組織最佳實踐，因為API頁面可以像任何AEM Sites頁面一樣本地化。

## 「建立事件API」頁

1. 導航到 **[!UICONTROL AEM] > [!UICONTROL 站點] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**。
1. **點擊API頁上的標籤**，然後按一下 **建立** 按鈕，在API頁下面新建一個「事件API」頁。
   1. 點擊 **建立** 的子菜單。
   1. 選擇 **事件API** 模板
   1. 在 **名稱** 欄位輸入 **事件**
   1. 在 **標題** 欄位輸入 **事件API**
   1. 點擊 **建立** 的子菜單。
   1. 點擊 **完成** 返回AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## 「創作事件API」頁

>[!NOTE]
>
> 項目提供了CSS，以便為作者提供一些基本樣式。

1. 編輯 **事件API** 導航至頁面 **>站AEM點> WKND Mobile >英語> API**，選擇 **事件API** 頁面和點擊 **編輯** 按鈕。
1. 添加 **標誌影像** 將應用程式從Asset Finder拖放到Image元件佔位符上，以在應用程式中顯示。
   * 使用在以下位置找到的提供的徽標 `/content/dam/wknd-mobile/images/wknd-logo.png`。

1. 添加 **標籤行** 顯示在事件上方。
   1. 編輯 **文本** 元件
   1. 輸入文本：
      * `The WKND is here.`

1. 選擇 **事件** 要顯示：
   1. 在 **屬性** 頁籤：
      * 型號： **事件**
      * 父路徑： **/content/dam/wknd mobile/en/events**
      * 標籤： **&lt;leave blank=&quot;&quot;>**
   1. 在 **元素** 頁籤：
      * 刪除所有列出的元素，以確保事件內容片段的所有元素都已公開。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## 查看API頁的JSON輸出

可以通過以下方式請求頁面來查看JSON輸出及其格式： `.model.json` 選擇器。

此JSON結構（或架構）必須由此API的使用者充分理解。 關鍵的API使用者要瞭解結構的哪些方面是固定的(即 事件API的徽標（影像）和標籤活動（文本），它們是流體的(即 「內容片段清單」元件下列出的事件)。

在已發佈的API上違反此合同，可能導致使用的應用程式中的行為不正確。

1. 在新瀏覽器頁籤中，使用 `.model.json` selector ，它調AEM用Content Services的JSON導出器，並將頁和元件序列化為標準化且定義良好的JSON結構。

   這些頁生成的JSON結構是使用應用必須對齊的結構。

1. 請求 **事件API** 頁 **JSON**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果應與以下內容類似：

![Content Services AEM JSON輸出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可以在 **整齊** （格式化）使用 `.tidy` 選擇器：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

（可選）安裝 [com.adobe.aem.guides.wknd-mobile content-chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 通過 [包管AEM理器](http://localhost:4502/crx/packmgr/index.jsp)。 此軟體包包含本教程及前面各章中概述的配置和內容。

* [第6章 — 將AEM發佈上的內容公開為JSON](./chapter-6.md)
