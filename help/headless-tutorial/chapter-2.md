---
title: 第2章——定義事件內容片段模型
seo-title: AEM Content Services快速入門——第2章——定義事件內容片段模型
description: AEM無頭教學課程的第2章涵蓋啟用和定義內容片段模型，用於定義標準化的資料結構和製作介面以建立事件。
seo-description: AEM無頭教學課程的第2章涵蓋啟用和定義內容片段模型，用於定義標準化的資料結構和製作介面以建立事件。
translation-type: tm+mt
source-git-commit: 1faf22f2e664b775c11e16cb1dfa18b363a7316b
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 6%

---


# 第2章——使用內容片段模型

AEM內容片段模型可定義內容結構描述，這些結構描述可用於範本化AEM作者建立原始內容。 此方法類似於腳手架或表單製作。 「內容片段」的主要概念是製作的內容不受簡報限制，這表示其適用於多頻道使用，而消費應用程式（不論是AEM、單一頁面應用程式或行動應用程式）會控制內容對使用者的顯示方式。

「內容片段」的主要顧慮是：

1. 從作者收集正確的內容
2. 內容可以以結構化、易於理解的格式公開給消費性應用程式。

本章介紹啟用和定義內容片段模型，用於定義標準化的資料結構和編寫介面，以建模和建立「事件」。

## 啟用內容片段模型

內容片段模 **型必須** 透過 **[AEM的「設定瀏 [!UICONTROL 覽器」啟用]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**。

如果未針對設定啟 **用「內容片段模型** 」，則相關AEM設定將不會顯示「建立 **[!UICONTROL >]** 內容片段」按鈕。

>[!NOTE]
>
>AEM的設定代表儲存於下 [方的一組內容感知租用戶](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 設定 `/conf`。 通常，AEM設定會與AEM Sites中管理的特定網站或負責子集內容（資產、頁面等）的業務單位產生關聯 在AEM中。
>
>為了讓配置影響內容分層結構，必須通過該內容分層結構上 `cq:conf` 的屬性引用該配置。 (這是在以下步 [!DNL WKND Mobile] 驟 **5中完成** )。
>
>使用 `global` 配置時，配置將應用於所有內容， `cq:conf` 無需設定。
>
>See the [[!UICONTROL Configuration Browser] documentation](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html) for more information.

1. 以具有適當權限的使用者身分登入AEM Author，以修改相關設定。
   * 在本教學課程中， **可使用** admin使用者。
1. 導覽至「工 **[!UICONTROL 具] >一般 [!UICONTROL >設定瀏][!UICONTROL 覽器」]**
1. 點選 **旁的資料夾圖示** , **[!DNL WKND Mobile]** 然後點選左上角的「 **[!UICONTROL Edit] （編輯）** 」按鈕。
1. 選取 **[!UICONTROL 內容片段模型]**，然後點選右 **[!UICONTROL 上方的「儲存並關閉]** 」。

   如此可啟用已套用設定之資產資料夾內容樹上的內容片段 [!DNL WKND Mobile] 模型。

   >[!NOTE]
   >
   >此組態變更無法從 [!UICONTROL AEM Configuration] Web UI中反轉。 要撤消此配置：
   >    
   >    1. 開啟 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 導航到 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除節 `models` 點

   >    
   >在此配置下建立的任何現有內容片段模型都將被刪除，其定義將儲存在下 `/conf/wknd-mobile/settings/dam/cfm/models`。

1. 將設定 **[!DNL WKND Mobile]** 套用至「資產」檔 **[!DNL WKND Mobile]案夾** ，以允許在「資產」檔案夾階層中建立「內容片段模型」的內容片段：

   1. 導覽至「 **[!UICONTROL AEM] >資 [!UICONTROL 產] >檔 [!UICONTROL 案」]**
   1. 選擇「 **[!UICONTROL WKND Mobile」檔案夾]**
   1. 點選頂端 **[!UICONTROL 動作列中的]** 「屬性」按鈕，以開啟「資料 [!UICONTROL 夾屬性」]
   1. 在資 [!UICONTROL 料夾屬性]，點選「雲端 **[!UICONTROL 服務」標籤]**
   1. 驗證「 **[!UICONTROL 雲端設定]** 」欄位是否設 **為/conf/wknd-mobile**
   1. 點選 **[!UICONTROL 右上方的]** 「儲存並關閉」以持續變更

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## 瞭解要建立的內容片段模型

在定義內容片段模型之前，讓我們先回顧一下我們將帶來的體驗，以確保我們擷取了所有必要的資料點。 為此，我們將檢閱行動應用程式設計，並將設計元素對應至要收集的內容。

我們可依下列方式劃分定義事件的資料點：

![建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

有了對應，我們可以定義內容片段，用來收集並最終公開事件資料。

## 建立內容片段模型

1. 導覽至「 **[!UICONTROL 工具] >資 [!UICONTROL 產] >內 [!UICONTROL 容片段模型」]**。
1. 點選資料 **[!DNL WKND Mobile]** 夾以開啟。
1. 點選「 **[!UICONTROL 建立]** 」以開啟「內容片段模型建立精靈」。
1. 輸入 **[!DNL Event]** 為「模 **[!UICONTROL 型標題]** 」(說明 *為可選)* ，然後點選「 **[!UICONTROL 建立]** 」以儲存。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## 定義內容片段模型的結構

1. 導覽至「 **[!UICONTROL 工具] >資 [!UICONTROL 產] >內 [!UICONTROL 容片段模型] >[!DNL WKND]**」。
1. 選取「內 **[!DNL Event]** 容片段模型」，然後點選 **[!UICONTROL 頂端動作列中的]** 「編輯」。
1. 從右側 **[!UICONTROL 的「資料類型] 」標籤，將「單** 行文字輸入 **[!UICONTROL 」拖曳至左側的下拉區域以定義欄]****[!DNL Question]** 位。
1. 請確定左 **[!UICONTROL 側已選取新的「單行文字]** 」輸入，右側已選取「屬 **[!UICONTROL 性] 」索引標籤** 。 按如下方式填入「屬性」欄位：

   * [!UICONTROL 呈現為] : `textfield`
   * [!UICONTROL 欄位標籤] : `Event Title`
   * [!UICONTROL 屬性名稱] : `eventTitle`
   * [!UICONTROL 最大長度] :25
   * [!UICONTROL 必要] : `Yes`

使用下面定義的輸入定義重複這些步驟，以建立「事件內容片段模型」的其餘部分。

>[!NOTE]
>
> 「屬 **性名稱** 」欄位必須完全相符，因為Android應用程式會設定為鍵出這些名稱。

### 事件說明

* [!UICONTROL 資料類型] : `Multi-line text`
* [!UICONTROL 欄位標籤] : `Event Description`
* [!UICONTROL 屬性名稱] : `eventDescription`
* [!UICONTROL 預設類型] : `Rich text`

### 事件日期和時間

* [!UICONTROL 資料類型] : `Date and time`
* [!UICONTROL 欄位標籤] : `Event Date and Time`
* [!UICONTROL 屬性名稱] : `eventDateAndTime`
* [!UICONTROL 必要] : `Yes`

### 事件類型

* [!UICONTROL 資料類型] : `Enumeration`
* [!UICONTROL 欄位標籤] : `Event Type`
* [!UICONTROL 屬性名稱] : `eventType`
* [!UICONTROL 選項] : `Art,Music,Performance,Photography`

### 票價

* [!UICONTROL 資料類型] : `Number`
* [!UICONTROL 呈現為] : `numberfield`
* [!UICONTROL 欄位標籤] : `Ticket Price`
* [!UICONTROL 屬性名稱] : `eventPrice`
* [!UICONTROL 類型] : `Integer`
* [!UICONTROL 必要] : `Yes`

### 事件影像

* [!UICONTROL 資料類型] : `Content Reference`
* [!UICONTROL 呈現為] : `contentreference`
* [!UICONTROL 欄位標籤] : `Event Image`
* [!UICONTROL 屬性名稱] : `eventImage`
* [!UICONTROL 根路徑] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必要] : `Yes`

### 場地名稱

* [!UICONTROL 資料類型] : `Single-line text`
* [!UICONTROL 呈現為] : `textfield`
* [!UICONTROL 欄位標籤] : `Venue Name`
* [!UICONTROL 屬性名稱] : `venueName`
* [!UICONTROL 最大長度] :20
* [!UICONTROL 必要] : `Yes`

### Venue City

* [!UICONTROL 資料類型] : `Enumeration`
* [!UICONTROL 欄位標籤] : `Venue City`
* [!UICONTROL 屬性名稱] : `venueCity`
* [!UICONTROL 選項] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>屬 **[!UICONTROL 性名稱]** ：表示將 **** 儲存此值的JCR屬性名稱，以及JSON檔案中的金鑰。 這應該是一個語義名稱，在內容片段模型的生命週期中不會改變。

完成內容片段模型的建立後，您最後應該會得到如下定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

（可選）透過 [AEM的Package Manager，在AEM Author上安裝](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip [content套件](http://localhost:4502/crx/packmgr/index.jsp)。 此套件包含教學課程本部分中概述的設定和內容。

* [第3章——製作事件內容片段](./chapter-3.md)
