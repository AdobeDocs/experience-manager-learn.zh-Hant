---
title: 第5章 — 編寫內容服務頁面 — 內容服務
description: AEM Headless教學課程的第5章涵蓋從第4章中定義的範本建立頁面。 這些頁面將作為JSON HTTP端點。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 0%

---


# 第5章 — 編寫內容服務頁面

AEM Headless教學課程的第5章涵蓋從第4章中定義的範本建立頁面。 本章中建立的頁面會成為行動應用程式的JSON HTTP端點。

>[!NOTE]
>
> `/content/wknd-mobile/en/api`的頁面內容架構已預先建置。 `en`和`api`的基頁具有架構和組織目的，但並不嚴格要求。 如果API內容可能已本地化，最佳作法是遵循一般的「語言復本」和「多網站管理員」頁面組織最佳實務，因為API頁面可像任何AEM Sites頁面一樣本地化。

## 建立事件API頁面

1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**。
1. **點選「API」頁面的標籤**，然後點選頂端 **** 動作列中的「建立」按鈕，並在「API」頁面下方建立新的「事件API」頁面。
   1. 點選頂端動作列中的&#x200B;**建立**
   1. 選擇&#x200B;**事件API**&#x200B;模板
   1. 在&#x200B;**Name**&#x200B;欄位中，輸入&#x200B;**events**
   1. 在&#x200B;**Title**&#x200B;欄位中，輸入&#x200B;**Events API**
   1. 點選頂端動作列中的&#x200B;**建立**&#x200B;以建立頁面
   1. 點選&#x200B;**完成**&#x200B;以返回AEM Sites管理員

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## 編寫「事件API」頁面

>[!NOTE]
>
> 專案提供CSS，以提供一些製作體驗的基本樣式。

1. 導覽至「**事件API**」頁面，方法是導覽至「**AEM >網站> WKND行動>英文> API**」，選取「**事件API**」頁面，然後點選頂端動作列中的「**編輯**」。
1. 將&#x200B;**標誌影像**&#x200B;從「資產尋找器」拖放至「影像」元件預留位置，以在應用程式中顯示。
   * 使用`/content/dam/wknd-mobile/images/wknd-logo.png`處提供的徽標。

1. 新增&#x200B;**標籤行**&#x200B;以顯示在事件上方。
   1. 編輯&#x200B;**Text**&#x200B;元件
   1. 輸入文本：
      * `The WKND is here.`

1. 選取要顯示的&#x200B;**events**:
   1. 在&#x200B;**Properties**&#x200B;標籤上設定以下配置：
      * 模型：**事件**
      * 父路徑：**/content/dam/wknd-mobile/en/events**
      * 標籤：**&lt;留空>**
   1. 在&#x200B;**Elements**&#x200B;標籤上設定以下配置：
      * 移除所有列出的元素，以確保事件內容片段的所有元素都公開。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## 檢閱API頁面的JSON輸出

使用`.model.json`選取器要求頁面，即可檢閱JSON輸出及其格式。

此API的使用者必須充分了解此JSON結構（或結構）。 重要的API使用者必須了解結構的哪些方面已修正(即 事件API的標誌（影像）和標籤即時（文字），且流暢(即 「內容片段清單」元件下列出的事件)。

在已發佈的API上違反此合約，可能會在使用的應用程式中導致錯誤行為。

1. 在新的瀏覽器標籤中，使用`.model.json`選取器來要求事件API頁面(會叫用AEM Content Services的JSON匯出工具)，並將頁面和元件序列化為已標準化且定義良好的JSON結構。

   這些頁面產生的JSON結構是使用應用程式必須符合的結構。

1. 將「**事件API**」頁面要求為&#x200B;**JSON**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果應該會顯示類似：

![AEM內容服務JSON輸出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可使用`.tidy`選取器以&#x200B;**tidy**（格式化）方式輸出，以利人類閱讀：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

選擇性地，透過[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 此套件包含本教學課程及前幾章中概述的設定和內容。

* [第6章 — 將AEM發佈上的內容公開為JSON](./chapter-6.md)
