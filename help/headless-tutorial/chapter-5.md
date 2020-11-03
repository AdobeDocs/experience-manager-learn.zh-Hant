---
title: 第5章——編寫內容服務頁面
description: AEM無頭教學課程的第5章涵蓋從第4章中定義的範本建立頁面。 這些頁面將充當JSON HTTP端點。
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---


# 第5章——編寫內容服務頁面

AEM無頭教學課程的第5章涵蓋從第4章中定義的範本建立頁面。 本章中建立的頁面將做為行動應用程式的JSON HTTP端點。

>[!NOTE]
>
> 的頁面內容 `/content/wknd-mobile/en/api` 架構已預先建立。 基本頁面 `en` 和服 `api` 務於架構和組織用途，但並非嚴格要求。 如果API內容可本地化，則最好遵循「語言復本」和「多網站管理員」頁面組織的一般最佳實務，因為API頁面可像任何AEM Sites頁面一樣本地化。

## 「建立事件API」頁

1. 導覽至 **[!UICONTROL AEM] >網 [!UICONTROL 站] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**。
1. **點選API頁面的標籤**，然後點選頂端動作列中的「建立 **** 」按鈕，並在API頁面下建立新的事件API頁面。
   1. 點選 **頂端動作列中的** 「建立」
   1. 選擇 **事件API範本**
   1. 在「名稱 **」欄位中** ，輸入 **事件**
   1. 在「標 **題** 」欄位中輸 **入事件API**
   1. 點選 **頂端動作列中的** 「建立」以建立頁面
   1. 點選 **「完成** 」以返回AEM Sites管理員

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## 「編寫事件API」頁

>[!NOTE]
>
> 本專案提供CSS，以提供一些基本的樣式來提供作者體驗。

1. 導覽至 **AEM > Sites > WKND Mobile > English > API** ，選取「 **Events API**」頁面，然後點選「 ******** EditEdit」（在頂層動作列中編輯），即可編輯「事件API」頁面。
1. 從「資 **產搜尋器** 」拖放標誌影像至「影像」元件預留位置，以新增標誌影像以顯示在應用程式中。
   * 使用位於的提供標誌 `/content/dam/wknd-mobile/images/wknd-logo.png`。

1. 新增 **標籤行** ，以顯示在事件上方。
   1. 編輯 **Text元件**
   1. 輸入文字：
      * `The WKND is here.`

1. 選取要 **顯示的事** 件：
   1. 在「屬性」( **Properties)頁籤上設定以** 下配置：
      * 型號： **事件**
      * 父路徑： **/content/dam/wknd-mobile/tw/events**
      * 標籤： **&lt;留空>**
   1. 在「元素」( **Elements** )頁籤上設定以下配置：
      * 移除任何列出的元素，以確保「事件內容片段」的「所有元素」已公開。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## 檢視API頁面的JSON輸出

JSON輸出及其格式可透過使用選取器請求頁面來 `.model.json` 檢視。

此API的使用者必須充分瞭解此JSON結構（或結構）。 關鍵的API消費者瞭解結構的哪些方面是固定的(即 事件API的標誌（影像）和即時標籤（文字），而且流暢(即 內容片段清單元件下列出的事件)。

在已發佈的API上違反此合約，可能會導致使用應用程式的行為不正確。

1. 在新的瀏覽器標籤中，使用選取器來請求「事件API」頁面(此選取器會叫用 `.model.json` AEM Content Services的「JSON匯出器」)，並將「頁面」和「元件」序列化為標準化、定義良好的JSON結構。

   這些頁面產生的JSON結構是使用應用程式必須對齊的結構。

1. 以 **JSON的形** 式請求 **「事件API**」頁面。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果應類似：

![AEM Content Services JSON輸出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可使用選取器 **** ，以精簡(格式化 `.tidy` )的方式輸出，以方便人類閱讀：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

（可選）透過 [AEM的Package Manager，在AEM Author上安裝](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip [content套件](http://localhost:4502/crx/packmgr/index.jsp)。 本套件包含教學課程本章及前幾章中概述的設定和內容。

* [第6章——將AEM Publish上的內容公開為JSON](./chapter-6.md)
