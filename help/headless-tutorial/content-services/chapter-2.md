---
title: 第2章——定義事件內容片段模型——內容服務
seo-title: 內容服AEM務入門——第2章——定義事件內容片段模型
description: 無頭教學課程的第AEM2章涵蓋啟用和定義內容片段模型，用於定義標準化的資料結構和製作介面以建立事件。
seo-description: 無頭教學課程的第AEM2章涵蓋啟用和定義內容片段模型，用於定義標準化的資料結構和製作介面以建立事件。
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 6%

---


# 第2章——使用內容片段模型

內AEM容片段模型定義內容結構描述，可用來範本化作者建立原始內容的AEM方式。 此方法類似於腳手架或表單製作。 「內容片段」的主要概念是製作的內容不受簡報限制，這表示其適用於多頻道使用，而消費應用程式、單頁應用程式或行動應用程式皆可控制內容對使用者的顯示方式。

「內容片段」的主要顧慮是：

1. 從作者收集正確的內容
2. 內容可以以結構化、易於理解的格式公開給消費性應用程式。

本章介紹啟用和定義內容片段模型，用於定義標準化的資料結構和編寫介面，以建模和建立「事件」。

## 啟用內容片段模型

內容片段模型&#x200B;**必須透過**[ AEM[!UICONTROL 組態瀏覽器]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**啟用。**

如果內容片段模型未為配置啟用&#x200B;****，則相關配置將不會顯示&#x200B;**[!UICONTROL 建立] > [!UICONTROL 內容片段]**&#x200B;按AEM鈕。

>[!NOTE]
>
>配AEM置表示儲存在`/conf`下的一組[上下文感知租用戶配置](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html)。 通常AEM配置與AEM Sites管理的特定網站或負責子集內容（資產、頁面等）的業務單位相關 中AEM。
>
>為了讓配置影響內容分層結構，必須通過該內容分層結構上的`cq:conf`屬性引用該配置。 （這是在&#x200B;**下面步驟5**&#x200B;的[!DNL WKND Mobile]配置中實現的）。
>
>使用`global`配置時，該配置將應用於所有內容，而且無需設定`cq:conf`。
>
>如需詳細資訊，請參閱[[!UICONTROL 組態瀏覽器]檔案](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)。

1. 以具有適當權限的使用者身分登入AEM Author，以修改相關設定。
   * 在本教學課程中，可使用&#x200B;**admin**&#x200B;使用者。
1. 導覽至「**[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 組態瀏覽器]**」
1. 點選&#x200B;**[!DNL WKND Mobile]**&#x200B;旁的&#x200B;**資料夾圖示**&#x200B;以進行選取，然後點選左上角的&#x200B;**[!UICONTROL 編輯]按鈕**。
1. 選擇&#x200B;**[!UICONTROL 內容片段模型]**，然後點選右上角的&#x200B;**[!UICONTROL 儲存並關閉]**。

   如此可啟用已套用[!DNL WKND Mobile]組態之資產資料夾內容樹狀結構的內容片段模型。

   >[!NOTE]
   >
   >此配置更改無法從[!UICONTROL AEM Configuration] Web UI中撤消。 要撤消此配置：
   >    
   >    1. 開啟[CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 導航到 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除`models`節點

   >    
   >在此配置下建立的任何現有內容片段模型都將被刪除，其定義將儲存在`/conf/wknd-mobile/settings/dam/cfm/models`下。

1. 將&#x200B;**[!DNL WKND Mobile]**&#x200B;組態套用至&#x200B;**[!DNL WKND Mobile]Assets資料夾**，以允許在該Assets資料夾階層中建立來自內容片段模型的內容片段：

   1. 導覽至「**[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案]**」
   1. 選擇&#x200B;**[!UICONTROL WKND Mobile]資料夾**
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 屬性]**&#x200B;按鈕，以開啟[!UICONTROL 資料夾屬性]
   1. 在[!UICONTROL 資料夾屬性]中，按一下&#x200B;**[!UICONTROL Cloud Services]**&#x200B;頁籤
   1. 驗證「**[!UICONTROL 雲配置]**」欄位是否設定為&#x200B;**/conf/wknd-mobile**
   1. 點選右上角的&#x200B;**[!UICONTROL 儲存並關閉]**&#x200B;以持續變更

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## 瞭解要建立的內容片段模型

在定義內容片段模型之前，讓我們先回顧一下我們將帶來的體驗，以確保我們擷取了所有必要的資料點。 為此，我們將檢閱行動應用程式設計，並將設計元素對應至要收集的內容。

我們可依下列方式劃分定義事件的資料點：

![建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

有了對應，我們可以定義內容片段，用來收集並最終公開事件資料。

## 建立內容片段模型

1. 導覽至「**[!UICONTROL 工具] > [!UICONTROL 資產] > [!UICONTROL 內容片段模型]**」。
1. 點選&#x200B;**[!DNL WKND Mobile]**&#x200B;資料夾以開啟。
1. 點選「**[!UICONTROL 建立]**」以開啟「內容片段模型建立」精靈。
1. 輸入&#x200B;**[!DNL Event]**&#x200B;作為&#x200B;**[!UICONTROL Model Title]** *(description is optional)*，然後點選&#x200B;**[!UICONTROL Create]**&#x200B;以儲存。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## 定義內容片段模型的結構

1. 導覽至「**[!UICONTROL 工具] > [!UICONTROL 資產] > [!UICONTROL 內容片段模型] >[!DNL WKND]**」。
1. 選取「**[!DNL Event]**&#x200B;內容片段模型」，然後點選頂端動作列中的「編輯&#x200B;****」。
1. 從右側的&#x200B;**[!UICONTROL 資料類型]頁籤**&#x200B;將&#x200B;**[!UICONTROL 單行文本輸入]**&#x200B;拖動到左下拉區中以定義&#x200B;**[!DNL Question]**&#x200B;欄位。
1. 確保左側選擇了新的&#x200B;**[!UICONTROL 單行文本輸入]**，右側選擇了&#x200B;**[!UICONTROL 屬性]頁籤**。 按如下方式填入「屬性」欄位：

   * [!UICONTROL 呈現為] : `textfield`
   * [!UICONTROL 欄位標籤] : `Event Title`
   * [!UICONTROL 屬性名稱] : `eventTitle`
   * [!UICONTROL 最大長度] :25
   * [!UICONTROL 必要] :  `Yes`

使用下面定義的輸入定義重複這些步驟，以建立「事件內容片段模型」的其餘部分。

>[!NOTE]
>
> **屬性名稱**&#x200B;欄位必須完全相符，因為Android應用程式已設定為關閉這些名稱。

### 事件說明

* [!UICONTROL 資料類型] : `Multi-line text`
* [!UICONTROL 欄位標籤] : `Event Description`
* [!UICONTROL 屬性名稱] : `eventDescription`
* [!UICONTROL 預設類型] : `Rich text`

### 事件日期和時間

* [!UICONTROL 資料類型] : `Date and time`
* [!UICONTROL 欄位標籤] : `Event Date and Time`
* [!UICONTROL 屬性名稱] : `eventDateAndTime`
* [!UICONTROL 必要] :  `Yes`

### 事件類型

* [!UICONTROL 資料類型] : `Enumeration`
* [!UICONTROL 欄位標籤] : `Event Type`
* [!UICONTROL 屬性名稱] : `eventType`
* [!UICONTROL 選項] :  `Art,Music,Performance,Photography`

### 票價

* [!UICONTROL 資料類型] : `Number`
* [!UICONTROL 呈現為] : `numberfield`
* [!UICONTROL 欄位標籤] : `Ticket Price`
* [!UICONTROL 屬性名稱] : `eventPrice`
* [!UICONTROL 類型] :  `Integer`
* [!UICONTROL 必要] :  `Yes`

### 事件影像

* [!UICONTROL 資料類型] : `Content Reference`
* [!UICONTROL 呈現為] : `contentreference`
* [!UICONTROL 欄位標籤] : `Event Image`
* [!UICONTROL 屬性名稱] : `eventImage`
* [!UICONTROL 根路徑] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必要] :  `Yes`

### 場地名稱

* [!UICONTROL 資料類型] : `Single-line text`
* [!UICONTROL 呈現為] : `textfield`
* [!UICONTROL 欄位標籤] : `Venue Name`
* [!UICONTROL 屬性名稱] : `venueName`
* [!UICONTROL 最大長度] :20
* [!UICONTROL 必要] :  `Yes`

### Venue City

* [!UICONTROL 資料類型] : `Enumeration`
* [!UICONTROL 欄位標籤] : `Venue City`
* [!UICONTROL 屬性名稱] : `venueCity`
* [!UICONTROL 選項] :  `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL 屬性名稱]**&#x200B;表示&#x200B;**both**&#x200B;將儲存此值的JCR屬性名稱，以及JSON檔案中的金鑰。 這應該是一個語義名稱，在內容片段模型的生命週期中不會改變。

完成內容片段模型的建立後，您最後應該會得到如下定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

（可選）透過[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 此套件包含教學課程本部分中概述的設定和內容。

* [第3章——製作事件內容片段](./chapter-3.md)
