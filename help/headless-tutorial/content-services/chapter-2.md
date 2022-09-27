---
title: 第2章 — 定義事件內容片段模型 — 內容服務
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: AEM Headless教學課程的第2章涵蓋啟用和定義內容片段模型，這些模型用來定義標準化的資料結構和建立事件的製作介面。
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: cfb7ed39ecb85998192ba854b34161f7e1dba19a
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 7%

---

# 第2章 — 使用內容片段模型

AEM內容片段模型會定義內容結構，供AEM作者用來範本建立原始內容。 此方法類似於架構或表單式製作。 內容片段的主要概念是製作內容不受簡報限制，這表示其用途為多管道使用，耗用的應用程式(包括AEM、單頁應用程式或行動應用程式)可控制內容向使用者顯示的方式。

「內容片段」的主要考量是：

1. 會從作者收集正確的內容
2. 內容可以以結構化、理解得當的格式向消費應用程式公開。

本章涵蓋啟用和定義內容片段模型，用於定義標準化的資料結構和製作介面，以用於建模和建立「事件」。

## 啟用內容片段模型

內容片段模型 **必須** 透過 **[AEM [!UICONTROL 配置瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

如果內容片段模型為 **not** 已啟用設定， **[!UICONTROL 建立] > [!UICONTROL 內容片段]** 相關AEM設定不會顯示「 」按鈕。

>[!NOTE]
>
>AEM設定代表一組 [內容感知租用戶配置](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 儲存在 `/conf`. 通常，AEM設定會與AEM Sites中管理的特定網站或負責子集內容（資產、頁面等）的業務單位產生關聯 在AEM中。
>
>為了讓設定影響內容階層，必須透過 `cq:conf` 屬性。 (這是為 [!DNL WKND Mobile] 配置 **步驟5** )。
>
>當 `global` 已使用設定，此設定會套用至所有內容，而 `cq:conf` 不需要設定。
>
>請參閱 [[!UICONTROL 配置瀏覽器] 檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) 以取得更多資訊。

1. 以具有修改相關設定之適當權限的使用者身分登入AEM Author。
   * 在本教學課程中， **管理員** 可使用。
1. 導覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 配置瀏覽器]**
1. 點選 **資料夾圖示** 下一頁 **[!DNL WKND Mobile]** ，然後點選 **[!UICONTROL 編輯] 按鈕** 左上角。
1. 選擇 **[!UICONTROL 內容片段模型]**，然後點選 **[!UICONTROL 儲存並關閉]** 在右上角。

   如此可在具有的資產資料夾內容樹狀結構上啟用內容片段模型 [!DNL WKND Mobile] 已套用設定。

   >[!NOTE]
   >
   >此配置更改不可從 [!UICONTROL AEM設定] 網頁UI。 若要還原此設定：
   >    
   >    1. 開啟 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 導航到 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除 `models` 節點

   >    
   >在此設定下建立的任何現有內容片段模型都會遭到刪除，其定義也會儲存在 `/conf/wknd-mobile/settings/dam/cfm/models`.

1. 套用 **[!DNL WKND Mobile]** 設定 **[!DNL WKND Mobile]資產資料夾** 若要允許在該「資產」資料夾階層內建立內容片段模型的內容片段：

   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案]**
   1. 選取 **[!UICONTROL WKND Mobile] 資料夾**
   1. 點選 **[!UICONTROL 屬性]** 按鈕以開啟 [!UICONTROL 資料夾屬性]
   1. 在 [!UICONTROL 資料夾屬性]，點選 **[!UICONTROL Cloud Services]** 標籤
   1. 驗證 **[!UICONTROL 雲端設定]** 欄位設為 **/conf/wknd-mobile**
   1. 點選 **[!UICONTROL 儲存並關閉]** 在保留變更的右上角

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

>[!WARNING]
>
> __內容片段模型__ 已從 __工具>資產__ to __工具>一般__.

## 了解要建立的內容片段模型

在定義「內容片段」模型之前，請先檢閱我們將帶來的體驗，以確保擷取所有必要的資料點。 為此，我們將審核行動應用程式設計，並將設計元素對應至要收集的內容。

我們可依下列方式劃分定義事件的資料點：

![建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

透過對應，我們可以定義內容片段，以用於收集並最終公開事件資料。

## 建立內容片段模型

1. 導覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 內容片段模型]**.
1. 點選 **[!DNL WKND Mobile]** 要開啟的資料夾。
1. 點選 **[!UICONTROL 建立]** 開啟「內容片段模型建立」精靈。
1. 輸入 **[!DNL Event]** 作為 **[!UICONTROL 模型標題]** *（說明為選填）* 點選 **[!UICONTROL 建立]** 儲存。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## 定義內容片段模型的結構

1. 導覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 內容片段模型] >[!DNL WKND]**.
1. 選取 **[!DNL Event]** 內容片段模型和點選 **[!UICONTROL 編輯]** 在頂端動作列。
1. 從 **[!UICONTROL 資料類型] 標籤** 在右側拖動 **[!UICONTROL 單行文本輸入]** 放入左側拖放區域，以定義 **[!DNL Question]** 欄位。
1. 確保新 **[!UICONTROL 單行文本輸入]** 在左側選取，而 **[!UICONTROL 屬性] 標籤** 的URL。 填入「屬性」欄位，如下所示：

   * [!UICONTROL 呈現為] : `textfield`
   * [!UICONTROL 欄位標籤] : `Event Title`
   * [!UICONTROL 屬性名稱] : `eventTitle`
   * [!UICONTROL 最大長度] :25
   * [!UICONTROL 必填] : `Yes`

使用下面定義的輸入定義重複這些步驟，以建立其餘的事件內容片段模型。

>[!NOTE]
>
> 此 **屬性名稱** 欄位必須完全相符，因為Android應用程式已設定為關閉這些名稱。

### 事件說明

* [!UICONTROL 資料類型] : `Multi-line text`
* [!UICONTROL 欄位標籤] : `Event Description`
* [!UICONTROL 屬性名稱] : `eventDescription`
* [!UICONTROL 預設類型] : `Rich text`

### 事件日期和時間

* [!UICONTROL 資料類型] : `Date and time`
* [!UICONTROL 欄位標籤] : `Event Date and Time`
* [!UICONTROL 屬性名稱] : `eventDateAndTime`
* [!UICONTROL 必填] : `Yes`

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
* [!UICONTROL 必填] : `Yes`

### 事件影像

* [!UICONTROL 資料類型] : `Content Reference`
* [!UICONTROL 呈現為] : `contentreference`
* [!UICONTROL 欄位標籤] : `Event Image`
* [!UICONTROL 屬性名稱] : `eventImage`
* [!UICONTROL 根路徑] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必填] : `Yes`

### 場地名稱

* [!UICONTROL 資料類型] : `Single-line text`
* [!UICONTROL 呈現為] : `textfield`
* [!UICONTROL 欄位標籤] : `Venue Name`
* [!UICONTROL 屬性名稱] : `venueName`
* [!UICONTROL 最大長度] :20
* [!UICONTROL 必填] : `Yes`

### 法尼奧城

* [!UICONTROL 資料類型] : `Enumeration`
* [!UICONTROL 欄位標籤] : `Venue City`
* [!UICONTROL 屬性名稱] : `venueCity`
* [!UICONTROL 選項] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>此 **[!UICONTROL 屬性名稱]** 表示 **both** 將儲存此值的JCR屬性名稱，以及JSON檔案中的索引鍵。 這應該是在內容片段模型期間不會變更的語義名稱。

完成內容片段模型的建立後，您最後應會得到如下的定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

（可選）安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的內容套件(透過 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含本教學課程中概述的設定和內容。

* [第3章 — 編寫事件內容片段](./chapter-3.md)
