---
title: 第2章 — 定義事件內容片段模型 — Content Services
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: 《無頭教程》AEM第2章介紹了啟用和定義內容片段模型，用於定義用於建立事件的標準化資料結構和創作介面。
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 7%

---

# 第2章 — 使用內容片段模型

內AEM容片段模型定義內容架構，可用於模板由作者建立原始內AEM容。 這種方法類似於腳手架或基於形式的創作。 內容片段的關鍵概念是創作的內容與演示無關，這意味著它用於多渠道使用，在多渠道使用中，無論是單頁應用程式還是移動應用程式，都控制內容向用戶顯示的方式。

內容片段的主要關注點是確保：

1. 從作者處收集正確的內容
2. 該內容可以以結構化、易於理解的格式公開給消費的應用。

本章介紹啟用和定義用於定義標準化資料結構和創作介面以建模和建立「事件」的內容片段模型。

## 啟用內容片段模型

內容片段模型 **必須** 通過 **[AEM [!UICONTROL 配置瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**。

如果內容片段模型是 **不** 為配置啟用， **[!UICONTROL 建立] > [!UICONTROL 內容片段]** 按鈕將不顯示相關配AEM置。

>[!NOTE]
>
>AEM配置表示 [上下文感知租戶配置](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 儲存 `/conf`。 通AEM常，配置與在AEM Sites管理的特定網站或負責子集內容（資產、頁面等）的業務部門相關 的上AEM界。
>
>為了使配置影響內容層次結構，必須通過 `cq:conf` 內容層次結構上的屬性。 (這是為 [!DNL WKND Mobile] 配置 **步驟5** )。
>
>當 `global` 使用配置，配置適用於所有內容， `cq:conf` 不需要設定。
>
>查看 [[!UICONTROL 配置瀏覽器] 文檔](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) 的子菜單。

1. 以具有相應權限修改相關配置的用戶身份登錄到AEM Author。
   * 對於本教程， **管理員** 可以使用用戶。
1. 導航到 **[!UICONTROL 工具] > [!UICONTROL 常規] > [!UICONTROL 配置瀏覽器]**
1. 點擊 **資料夾表徵圖** 下 **[!DNL WKND Mobile]** ，然後點擊 **[!UICONTROL 編輯] 按鈕** 左上角。
1. 選擇 **[!UICONTROL 內容片段模型]**，然後點擊 **[!UICONTROL 保存並關閉]** 右上角。

   這將啟用具有以下功能的資產資料夾內容樹上的內容片段模型 [!DNL WKND Mobile] 已應用配置。

   >[!NOTE]
   >
   >此配置更改不可從 [!UICONTROL 配AEM置] Web UI。 要撤消此配置：
   >    
   >    1. 開啟 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 瀏覽到 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除 `models` 節點

   >    
   >在此配置下建立的任何現有內容片段模型都將被刪除，並且它們的定義將儲存在 `/conf/wknd-mobile/settings/dam/cfm/models`。

1. 應用 **[!DNL WKND Mobile]** 配置 **[!DNL WKND Mobile]資產資料夾** 允許在該資產資料夾層次結構中建立內容片段模型的內容片段：

   1. 導航到 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案]**
   1. 選擇 **[!UICONTROL WKND移動] 資料夾**
   1. 點擊 **[!UICONTROL 屬性]** 按鈕 [!UICONTROL 資料夾屬性]
   1. 在 [!UICONTROL 資料夾屬性]，按一下 **[!UICONTROL Cloud Services]** 頁籤
   1. 驗證 **[!UICONTROL 雲配置]** 欄位設定為 **/conf/wknd mobile**
   1. 點擊 **[!UICONTROL 保存並關閉]** 在右上角保留更改

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __內容片段模型__ 從 __工具>資產__ 至 __「工具」>「常規」__。

## 瞭解要建立的內容片段模型

在定義內容片段模型之前，讓我們回顧一下我們將帶來的體驗，以確保捕獲所有必要的資料點。 為此，我們將回顧移動應用程式設計並將設計元素映射到要收集的內容。

我們可以按如下方式細分定義Event的資料點：

![建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

利用映射，我們可以定義用於收集並最終公開事件資料的內容片段。

## 建立內容片段模型

1. 導航到 **[!UICONTROL 工具] > [!UICONTROL 常規] > [!UICONTROL 內容片段模型]**。
1. 點擊 **[!DNL WKND Mobile]** 開啟。
1. 點擊 **[!UICONTROL 建立]** 開啟「內容片段模型建立」嚮導。
1. 輸入 **[!DNL Event]** 的 **[!UICONTROL 模型標題]** *（說明是可選的）* 點擊 **[!UICONTROL 建立]** 來保存。

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 定義內容片段模型的結構

1. 導航到 **[!UICONTROL 工具] > [!UICONTROL 常規] > [!UICONTROL 內容片段模型] >[!DNL WKND]**。
1. 選擇 **[!DNL Event]** 內容片段模型和點擊 **[!UICONTROL 編輯]** 按鈕。
1. 從 **[!UICONTROL 資料類型] 頁籤** 右邊，拖動 **[!UICONTROL 單行文本輸入]** 到左下拉區域以定義 **[!DNL Question]** 的子菜單。
1. 確保新 **[!UICONTROL 單行文本輸入]** 的 **[!UICONTROL 屬性] 頁籤** 的下界。 按如下方式填充「屬性」欄位：

   * [!UICONTROL 呈現為] : `textfield`
   * [!UICONTROL 欄位標籤] : `Event Title`
   * [!UICONTROL 屬性名稱] : `eventTitle`
   * [!UICONTROL 最大長度] :25
   * [!UICONTROL 必需] : `Yes`

使用下面定義的輸入定義重複這些步驟，以建立「事件內容片段模型」的其餘部分。

>[!NOTE]
>
> 的 **屬性名稱** 欄位必須完全匹配，因為Android應用程式被寫程式為關閉這些名稱。

### 事件說明

* [!UICONTROL 資料類型] : `Multi-line text`
* [!UICONTROL 欄位標籤] : `Event Description`
* [!UICONTROL 屬性名稱] : `eventDescription`
* [!UICONTROL 預設類型] : `Rich text`

### 事件日期和時間

* [!UICONTROL 資料類型] : `Date and time`
* [!UICONTROL 欄位標籤] : `Event Date and Time`
* [!UICONTROL 屬性名稱] : `eventDateAndTime`
* [!UICONTROL 必需] : `Yes`

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
* [!UICONTROL 必需] : `Yes`

### 事件影像

* [!UICONTROL 資料類型] : `Content Reference`
* [!UICONTROL 呈現為] : `contentreference`
* [!UICONTROL 欄位標籤] : `Event Image`
* [!UICONTROL 屬性名稱] : `eventImage`
* [!UICONTROL 根路徑] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必需] : `Yes`

### 地點名稱

* [!UICONTROL 資料類型] : `Single-line text`
* [!UICONTROL 呈現為] : `textfield`
* [!UICONTROL 欄位標籤] : `Venue Name`
* [!UICONTROL 屬性名稱] : `venueName`
* [!UICONTROL 最大長度] :20
* [!UICONTROL 必需] : `Yes`

### 韋納城

* [!UICONTROL 資料類型] : `Enumeration`
* [!UICONTROL 欄位標籤] : `Venue City`
* [!UICONTROL 屬性名稱] : `venueCity`
* [!UICONTROL 選項] : `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>的 **[!UICONTROL 屬性名稱]** 表示 **兩者** 儲存此值的JCR屬性名稱以及JSON檔案中的鍵。 這應該是一個在內容片段模型的生命期內不會改變的語義名稱。

完成內容片段模型的建立後，您最終應該有一個如下定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

（可選）安裝 [com.adobe.aem.guides.wknd-mobile content-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) 通過 [包管AEM理器](http://localhost:4502/crx/packmgr/index.jsp)。 此軟體包包含本教程中概述的配置和內容。

* [第3章 — 創作事件內容片段](./chapter-3.md)
