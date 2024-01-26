---
title: 第2章 — 定義事件內容片段模型 — 內容服務
description: AEM Headless教學課程的第2章涵蓋啟用和定義內容片段模型，這些模型用來定義標準化資料結構和建立事件的製作介面。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 453
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 1%

---

# 第2章 — 使用內容片段模型

AEM內容片段模型會定義內容結構，以便AEM作者將原始內容的建立製作成範本。 此方法類似於支架或表單式製作。 內容片段的主要概念是編寫的內容與簡報無關，這表示其用於多管道用途，其中消費應用程式(AEM、單頁應用程式或行動應用程式)會控制向使用者顯示內容的方式。

內容片段的主要問題是確保：

1. 系統會向作者收集正確的內容
2. 內容能以結構化、容易理解的格式向消費的應用程式公開。

本章涵蓋啟用和定義內容片段模型，這些模型用來定義標準化資料結構和撰寫介面，以便建模和建立「事件」。

## 啟用內容片段模型

內容片段模型 **必須** 啟用方式： **[AEM [!UICONTROL 設定瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

如果內容片段模型為 **非** 已為設定啟用， **[!UICONTROL 建立] > [!UICONTROL 內容片段]** 按鈕將不會針對相關AEM設定顯示。

>[!NOTE]
>
>AEM設定代表一組 [內容感知租使用者設定](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 儲存於 `/conf`. AEM設定通常會與AEM Sites中管理的特定網站或負責內容子集（資產、頁面等）的業務單位建立關聯 在AEM中。
>
>為了讓設定影響內容階層，必須透過 `cq:conf` 屬性。 (這是透過以下專案達成： [!DNL WKND Mobile] 中的設定 **步驟5** 下)。
>
>當 `global` 設定已使用，設定會套用至所有內容，並且 `cq:conf` 不需要設定。
>
>請參閱 [[!UICONTROL 設定瀏覽器] 檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) 以取得詳細資訊。

1. 以具有適當許可權的使用者身分登入AEM Author，以修改相關的設定。
   * 在本教學課程中， **管理員** 使用者可以使用。
1. 瀏覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 設定瀏覽器]**
1. 點選 **資料夾圖示** 旁邊 **[!DNL WKND Mobile]** 以選取，然後點選 **[!UICONTROL 編輯] 按鈕** 左上角。
1. 選取 **[!UICONTROL 內容片段模型]**，然後點選 **[!UICONTROL 儲存並關閉]** 右上角。

   這樣會啟用具有下列條件的資產資料夾內容樹狀結構上的內容片段模型 [!DNL WKND Mobile] 已套用設定。

   >[!NOTE]
   >
   >此設定變更無法從 [!UICONTROL AEM設定] Web UI。 若要復原此設定：
   >    
   >    1. 開啟 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 瀏覽至 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除 `models` 節點
   >    
   >任何在此設定下建立的現有內容片段模型都會刪除，且其定義會儲存在 `/conf/wknd-mobile/settings/dam/cfm/models`.

1. 套用 **[!DNL WKND Mobile]** 的設定 **[!DNL WKND Mobile]資產資料夾** 若要允許在該資產資料夾階層中建立內容片段模型的內容片段：

   1. 瀏覽至 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案]**
   1. 選取 **[!UICONTROL WKND Mobile] 資料夾**
   1. 點選 **[!UICONTROL 屬性]** 按鈕以開啟 [!UICONTROL 資料夾屬性]
   1. 在 [!UICONTROL 資料夾屬性]，點選 **[!UICONTROL Cloud Service]** 標籤
   1. 驗證 **[!UICONTROL 雲端設定]** 欄位已設為 **/conf/wknd-mobile**
   1. 點選 **[!UICONTROL 儲存並關閉]** 以保留變更

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __內容片段模型__ 已從「 」移出「 」 __「工具」>「資產」__ 至 __「工具」>「一般」__.

## 瞭解要建立的內容片段模型

在定義內容片段模式之前，請先檢閱將帶動的體驗，以確保擷取到所有必要的資料點。 為此，我們將檢閱行動應用程式設計，並將設計元素對應至要收集的內容。

我們可以劃分定義事件的資料點，如下所示：

![建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

透過對應，我們可以定義用來收集並最終公開事件資料的內容片段。

## 建立內容片段模型

1. 導覽至「**[!UICONTROL 工具]」>「[!UICONTROL 一般]」>「[!UICONTROL 內容片段模型]**」。
1. 點選 **[!DNL WKND Mobile]** 資料夾開啟。
1. 點選 **[!UICONTROL 建立]** 以開啟內容片段模式建立精靈。
1. 輸入 **[!DNL Event]** 作為 **[!UICONTROL 模型標題]** *（說明為選用）* 然後點選 **[!UICONTROL 建立]** 以儲存。

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 定義內容片段模型的結構

1. 瀏覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 內容片段模型] >[!DNL WKND]**.
1. 選取 **[!DNL Event]** 內容片段模型並點選 **[!UICONTROL 編輯]** 在頂端動作列中。
1. 從 **[!UICONTROL 資料型別] 標籤** 在右側，拖曳 **[!UICONTROL 單行文字輸入]** 至左側下拉區域以定義 **[!DNL Question]** 欄位。
1. 確認新的 **[!UICONTROL 單行文字輸入]** 左側選取「 」，且 **[!UICONTROL 屬性] 標籤** ，即會在右側選取。 依照以下方式填入「屬性」欄位：

   * [!UICONTROL 呈現為] ： `textfield`
   * [!UICONTROL 欄位標籤] ： `Event Title`
   * [!UICONTROL 屬性名稱] ： `eventTitle`
   * [!UICONTROL 最大長度] ：25
   * [!UICONTROL 必填] ： `Yes`

使用下列定義的輸入定義重複這些步驟，以建立其餘的事件內容片段模式。

>[!NOTE]
>
> 此 **屬性名稱** 欄位必須完全相符，因為Android應用程式的程式設計會關閉這些名稱。

### 事件說明

* [!UICONTROL 資料型別] ： `Multi-line text`
* [!UICONTROL 欄位標籤] ： `Event Description`
* [!UICONTROL 屬性名稱] ： `eventDescription`
* [!UICONTROL 預設型別] ： `Rich text`

### 事件日期和時間

* [!UICONTROL 資料型別] ： `Date and time`
* [!UICONTROL 欄位標籤] ： `Event Date and Time`
* [!UICONTROL 屬性名稱] ： `eventDateAndTime`
* [!UICONTROL 必填] ： `Yes`

### 事件類型

* [!UICONTROL 資料型別] ： `Enumeration`
* [!UICONTROL 欄位標籤] ： `Event Type`
* [!UICONTROL 屬性名稱] ： `eventType`
* [!UICONTROL 選項] ： `Art,Music,Performance,Photography`

### 票價

* [!UICONTROL 資料型別] ： `Number`
* [!UICONTROL 呈現為] ： `numberfield`
* [!UICONTROL 欄位標籤] ： `Ticket Price`
* [!UICONTROL 屬性名稱] ： `eventPrice`
* [!UICONTROL 型別] ： `Integer`
* [!UICONTROL 必填] ： `Yes`

### 事件影像

* [!UICONTROL 資料型別] ： `Content Reference`
* [!UICONTROL 呈現為] ： `contentreference`
* [!UICONTROL 欄位標籤] ： `Event Image`
* [!UICONTROL 屬性名稱] ： `eventImage`
* [!UICONTROL 根路徑] ： `/content/dam/wknd-mobile/images`
* [!UICONTROL 必填] ： `Yes`

### 地點名稱

* [!UICONTROL 資料型別] ： `Single-line text`
* [!UICONTROL 呈現為] ： `textfield`
* [!UICONTROL 欄位標籤] ： `Venue Name`
* [!UICONTROL 屬性名稱] ： `venueName`
* [!UICONTROL 最大長度] ：20
* [!UICONTROL 必填] ： `Yes`

### 地點城市

* [!UICONTROL 資料型別] ： `Enumeration`
* [!UICONTROL 欄位標籤] ： `Venue City`
* [!UICONTROL 屬性名稱] ： `venueCity`
* [!UICONTROL 選項] ： `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>此 **[!UICONTROL 屬性名稱]** 表示 **兩者** 儲存此值的JCR屬性名稱以及JSON檔案中的索引鍵。 這應為語意名稱，不會隨著內容片段模式生命週期而變更。

完成內容片段模型的建立後，您應該會有如下的定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

或者安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM Author上的內容套件，透過 [AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含教學課程本部分中概述的設定和內容。

* [第3章 — 編寫事件內容片段](./chapter-3.md)
