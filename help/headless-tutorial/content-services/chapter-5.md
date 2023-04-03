---
title: 第5章 — 編寫內容服務頁面 — 內容服務
description: AEM Headless教學課程的第5章涵蓋從第4章中定義的範本建立頁面。 這些頁面將作為JSON HTTP端點。
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

# 第5章 — 編寫內容服務頁面

AEM Headless教學課程的第5章涵蓋從第4章中定義的範本建立頁面。 本章中建立的頁面會成為行動應用程式的JSON HTTP端點。

>[!NOTE]
>
> 的頁面內容架構 `/content/wknd-mobile/en/api` 已預先建置。 的基礎頁面 `en` 和 `api` 服務於架構和組織目的，但並非嚴格要求。 如果API內容可能已本地化，最佳作法是遵循一般的「語言復本」和「多網站管理員」頁面組織最佳實務，因為API頁面可像任何AEM Sites頁面一樣本地化。

## 建立事件API頁面

1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 網站] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **點選「API」頁面的標籤**，然後點選 **建立** 按鈕，並在「API」頁面下建立新的「事件API」頁面。
   1. 點選 **建立** 在頂端動作列中
   1. 選擇 **事件API** 範本
   1. 在 **名稱** 欄位輸入 **事件**
   1. 在 **標題** 欄位輸入 **事件API**
   1. 點選 **建立** ，建立頁面
   1. 點選 **完成** 返回AEM Sites管理員

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## 編寫「事件API」頁面

>[!NOTE]
>
> 專案提供CSS，以提供一些製作體驗的基本樣式。

1. 編輯 **事件API** 頁面 **AEM > Sites > WKND Mobile >英文> API**，選取 **事件API** 頁面，點選 **編輯** 在頂端動作列。
1. 新增 **標誌影像** 從「資產尋找器」將其拖放至「影像」元件預留位置，以顯示在應用程式中。
   * 使用提供的徽標(位於 `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. 新增 **標籤行** 以顯示在事件上方。
   1. 編輯 **文字** 元件
   1. 輸入文本：
      * `The WKND is here.`

1. 選取 **事件** 顯示：
   1. 在 **屬性** 標籤：
      * 模型： **事件**
      * 父路徑： **/content/dam/wknd-mobile/en/events**
      * 標籤： **&lt;leave blank=&quot;&quot;>**
   1. 在 **元素** 標籤：
      * 移除所有列出的元素，以確保事件內容片段的所有元素都公開。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## 檢閱API頁面的JSON輸出

請使用來檢閱JSON輸出及其格式的頁面 `.model.json` 選取器。

此API的使用者必須充分了解此JSON結構（或結構）。 重要的API使用者必須了解結構的哪些方面已修正(即 事件API的標誌（影像）和標籤即時（文字），且流暢(即 「內容片段清單」元件下列出的事件)。

在已發佈的API上違反此合約，可能會在使用的應用程式中導致錯誤行為。

1. 在新的瀏覽器分頁中，使用 `.model.json` 選取器會叫用AEM Content Services的JSON匯出工具，並將頁面和元件序列化為已標準化且定義的JSON結構。

   這些頁面產生的JSON結構是使用應用程式必須符合的結構。

1. 要求 **事件API** 頁面 **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果應該會顯示類似：

![AEM內容服務JSON輸出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可在 **整齊** （格式化）使用 `.tidy` 選取器：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

（可選）安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的內容套件(透過 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含本教學課程及前幾章中概述的設定和內容。

* [第6章 — 將AEM發佈上的內容公開為JSON](./chapter-6.md)
