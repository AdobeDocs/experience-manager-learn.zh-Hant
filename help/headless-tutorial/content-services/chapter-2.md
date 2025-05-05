---
title: 第2章 — 定義事件內容片段模型 — 內容服務
description: AEM Headless教學課程的第2章涵蓋啟用和定義內容片段模型，這些模型用來定義標準化資料結構和建立事件的製作介面。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 378
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
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

內容片段模型&#x200B;**必須**&#x200B;透過&#x200B;**[AEM [!UICONTROL 設定瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**&#x200B;啟用。

如果設定未啟用內容片段模型&#x200B;**&#x200B;**，則相關AEM設定將不會顯示&#x200B;**[!UICONTROL 建立] > [!UICONTROL 內容片段]**&#x200B;按鈕。

>[!NOTE]
>
>AEM設定代表一組儲存在`/conf`底下的[內容感知租使用者設定](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html)。 AEM設定通常會與AEM Sites中管理的特定網站或負責內容子集（資產、頁面等）的業務單位建立關聯 在AEM中。
>
>為了讓設定影響內容階層，必須透過該內容階層上的`cq:conf`屬性參考設定。 （以下&#x200B;**步驟5**&#x200B;中的[!DNL WKND Mobile]設定已達成此目的）。
>
>使用`global`設定時，該設定會套用至所有內容，而且不需要設定`cq:conf`。
>
>如需詳細資訊，請參閱[[!UICONTROL 設定瀏覽器]檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)。

1. 以具有適當許可權的使用者身分登入AEM Author，以修改相關的設定。
   * 在本教學課程中，可以使用&#x200B;**管理員**&#x200B;使用者。
1. 瀏覽至&#x200B;**[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 設定瀏覽器]**
1. 點選「**[!DNL WKND Mobile]**」旁的「**」資料夾圖示**」以選取，然後點選左上方的「**[!UICONTROL 」編輯]按鈕**。
1. 選取&#x200B;**[!UICONTROL 內容片段模型]**，然後點選右上方的&#x200B;**[!UICONTROL 儲存並關閉]**。

   這會啟用已套用[!DNL WKND Mobile]設定的資產資料夾內容樹狀結構上的內容片段模型。

   >[!NOTE]
   >
   >此組態變更無法從[!UICONTROL AEM組態] Web UI還原。 若要復原此設定：
   >    
   >    1. 開啟[CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 瀏覽至`/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除`models`節點
   >    
   >在此設定下建立的任何現有內容片段模型都會刪除，且其定義會儲存在`/conf/wknd-mobile/settings/dam/cfm/models`下。

1. 將&#x200B;**[!DNL WKND Mobile]**&#x200B;設定套用至&#x200B;**[!DNL WKND Mobile]Assets資料夾**，以允許在Assets資料夾階層中建立內容片段模型的內容片段：

   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL 檔案]**
   1. 選取&#x200B;**[!UICONTROL WKND Mobile]資料夾**
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 屬性]**&#x200B;按鈕以開啟[!UICONTROL 資料夾屬性]
   1. 在[!UICONTROL 資料夾屬性]中，點選&#x200B;**[!UICONTROL Cloud Service]**&#x200B;標籤
   1. 確認&#x200B;**[!UICONTROL 雲端設定]**&#x200B;欄位已設定為&#x200B;**/conf/wknd-mobile**
   1. 點選右上方的&#x200B;**[!UICONTROL 儲存並關閉]**&#x200B;以保留變更

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __內容片段模型__&#x200B;已從&#x200B;__工具> Assets__&#x200B;移至&#x200B;__工具>一般__。

## 瞭解要建立的內容片段模型

在定義內容片段模式之前，請先檢閱將帶動的體驗，以確保擷取到所有必要的資料點。 為此，我們將檢閱行動應用程式設計，並將設計元素對應至要收集的內容。

我們可以劃分定義事件的資料點，如下所示：

![正在建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

透過對應，我們可以定義用來收集並最終公開事件資料的內容片段。

## 建立內容片段模型

1. 導覽至「**[!UICONTROL 工具]」>「[!UICONTROL 一般]」>「[!UICONTROL 內容片段模型]**」。
1. 點選&#x200B;**[!DNL WKND Mobile]**&#x200B;資料夾以開啟。
1. 點選&#x200B;**[!UICONTROL 建立]**&#x200B;以開啟內容片段模型建立精靈。
1. 輸入&#x200B;**[!DNL Event]**&#x200B;作為&#x200B;**[!UICONTROL 模型標題]** *（說明為選用）*，然後點選&#x200B;**[!UICONTROL 建立]**&#x200B;以儲存。

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 定義內容片段模型的結構

1. 導覽至&#x200B;**[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 內容片段模型] >[!DNL WKND]**。
1. 選取&#x200B;**[!DNL Event]**&#x200B;內容片段模式，然後點選頂端動作列中的&#x200B;**[!UICONTROL 編輯]**。
1. 從右側的&#x200B;**[!UICONTROL 資料型別]索引標籤**，將&#x200B;**[!UICONTROL 單行文字輸入]**&#x200B;拖曳至左側拖放區域，以定義&#x200B;**[!DNL Question]**&#x200B;欄位。
1. 請確認已在左側選取新的&#x200B;**[!UICONTROL 單行文字輸入]**，並在右側選取&#x200B;**[!UICONTROL 屬性]索引標籤**。 依照以下方式填入「屬性」欄位：

   * [!UICONTROL 呈現為] ： `textfield`
   * [!UICONTROL 欄位標籤] ： `Event Title`
   * [!UICONTROL 屬性名稱] ： `eventTitle`
   * [!UICONTROL 最大長度] ： 25
   * [!UICONTROL 必要]： `Yes`

使用下列定義的輸入定義重複這些步驟，以建立其餘的事件內容片段模式。

>[!NOTE]
>
> **屬性名稱**&#x200B;欄位必須完全相符，因為Android應用程式的程式設計會關閉這些名稱。

### 事件說明

* [!UICONTROL 資料型別] ： `Multi-line text`
* [!UICONTROL 欄位標籤] ： `Event Description`
* [!UICONTROL 屬性名稱] ： `eventDescription`
* [!UICONTROL 預設型別]： `Rich text`

### 事件日期和時間

* [!UICONTROL 資料型別] ： `Date and time`
* [!UICONTROL 欄位標籤] ： `Event Date and Time`
* [!UICONTROL 屬性名稱] ： `eventDateAndTime`
* [!UICONTROL 必要]： `Yes`

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
* [!UICONTROL 必要]： `Yes`

### 事件影像

* [!UICONTROL 資料型別] ： `Content Reference`
* [!UICONTROL 呈現為] ： `contentreference`
* [!UICONTROL 欄位標籤] ： `Event Image`
* [!UICONTROL 屬性名稱] ： `eventImage`
* [!UICONTROL 根路徑] ： `/content/dam/wknd-mobile/images`
* [!UICONTROL 必要]： `Yes`

### 地點名稱

* [!UICONTROL 資料型別] ： `Single-line text`
* [!UICONTROL 呈現為] ： `textfield`
* [!UICONTROL 欄位標籤] ： `Venue Name`
* [!UICONTROL 屬性名稱] ： `venueName`
* [!UICONTROL 最大長度] ： 20
* [!UICONTROL 必要]： `Yes`

### 地點城市

* [!UICONTROL 資料型別] ： `Enumeration`
* [!UICONTROL 欄位標籤] ： `Venue City`
* [!UICONTROL 屬性名稱] ： `venueCity`
* [!UICONTROL 選項] ： `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL 屬性名稱]**&#x200B;表示&#x200B;**both**&#x200B;儲存此值的JCR屬性名稱以及JSON檔案中的金鑰。 這應為語意名稱，不會隨著內容片段模式生命週期而變更。

完成內容片段模型的建立後，您應該會有如下的定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

可選擇透過[AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)在AEM Author上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容封裝。 此套件包含教學課程本部分中概述的設定和內容。

* [第3章 — 編寫事件內容片段](./chapter-3.md)
