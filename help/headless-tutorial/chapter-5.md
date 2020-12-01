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
> `/content/wknd-mobile/en/api`的頁面內容架構已預先建立。 `en`和`api`的基本頁面有建築和組織用途，但不嚴格要求。 如果API內容可本地化，則最好遵循「語言復本」和「多網站管理員」頁面組織的一般最佳實務，因為API頁面可像任何AEM Sites頁面一樣本地化。

## 「建立事件API」頁

1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**。
1. **點選API頁面的標籤** **** ，然後點選頂端動作列中的「建立」按鈕，並在API頁面下方建立新的「事件API」頁面。
   1. 點選頂端動作列中的「建立」****
   1. 選擇&#x200B;**事件API**&#x200B;範本
   1. 在&#x200B;**名稱**&#x200B;欄位中，輸入&#x200B;**events**
   1. 在&#x200B;**Title**&#x200B;欄位中，輸入&#x200B;**Events API**
   1. 點選頂端動作列中的「建立」**以建立頁面**
   1. 點選&#x200B;**Done**&#x200B;返回AEM Sites管理員

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## 「編寫事件API」頁

>[!NOTE]
>
> 本專案提供CSS，以提供一些基本的樣式來提供作者體驗。

1. 導覽至&#x200B;**AEM > Sites > WKND Mobile > English > API**，選擇&#x200B;**Events API**&#x200B;頁面，然後點選頂端動作列中的&#x200B;**Edit**，編輯&#x200B;**頁面。**
1. 將&#x200B;**標誌影像**&#x200B;從「資產搜尋器」拖放至「影像」元件預留位置，以在應用程式中顯示。
   * 使用`/content/dam/wknd-mobile/images/wknd-logo.png`中提供的標誌。

1. 新增&#x200B;**標籤行**&#x200B;以顯示在事件上方。
   1. 編輯&#x200B;**Text**&#x200B;元件
   1. 輸入文字：
      * `The WKND is here.`

1. 選擇要顯示的&#x200B;**events**:
   1. 在&#x200B;**Properties**&#x200B;頁籤上設定以下配置：
      * 型號：**Event**
      * 父路徑：**/content/dam/wknd-mobile/tw/events**
      * 標籤：**&lt;留空>**
   1. 在&#x200B;**Elements**&#x200B;標籤上設定下列組態：
      * 移除任何列出的元素，以確保「事件內容片段」的「所有元素」已公開。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## 檢視API頁面的JSON輸出

使用`.model.json`選取器來要求頁面，即可檢視JSON輸出及其格式。

此API的使用者必須充分瞭解此JSON結構（或結構）。 關鍵的API消費者瞭解結構的哪些方面是固定的(即 事件API的標誌（影像）和即時標籤（文字），而且流暢(即 內容片段清單元件下列出的事件)。

在已發佈的API上違反此合約，可能會導致使用應用程式的行為不正確。

1. 在新的瀏覽器標籤中，使用`.model.json`選擇器來請求「事件API」頁面，該選擇器會叫用AEM Content Services的JSON匯出器，並將「頁面」和「元件」序列化為已標準化、已清楚定義的JSON結構。

   這些頁面產生的JSON結構是使用應用程式必須對齊的結構。

1. 請求&#x200B;**事件API**&#x200B;頁面為&#x200B;**JSON**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果應類似：

![AEM Content Services JSON輸出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可使用`.tidy`選擇器，以&#x200B;**tidy**（格式化）方式輸出，以利人為閱讀：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

（可選）透過[AEM的Package Manager](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝[com.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 本套件包含教學課程本章及前幾章中概述的設定和內容。

* [第6章——將AEM Publish上的內容公開為JSON](./chapter-6.md)
