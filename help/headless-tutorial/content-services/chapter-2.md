---
title: 第2章 — 定義事件內容片段模型 — 內容服務
seo-title: AEM內容服務快速入門 — 第2章 — 定義事件內容片段模型
description: AEM Headless教學課程的第2章涵蓋啟用和定義內容片段模型，這些模型用來定義標準化的資料結構和建立事件的製作介面。
seo-description: AEM Headless教學課程的第2章涵蓋啟用和定義內容片段模型，這些模型用來定義標準化的資料結構和建立事件的製作介面。
feature: 內容片段、API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1002'
ht-degree: 6%

---


# 第2章 — 使用內容片段模型

AEM內容片段模型會定義內容結構，供AEM作者用來範本建立原始內容。 此方法類似於架構或表單式製作。 內容片段的主要概念是製作內容不受簡報限制，這表示其用途為多管道使用，耗用的應用程式(包括AEM、單頁應用程式或行動應用程式)可控制內容向使用者顯示的方式。

「內容片段」的主要考量是：

1. 會從作者收集正確的內容
2. 內容可以以結構化、理解得當的格式向消費應用程式公開。

本章涵蓋啟用和定義內容片段模型，用於定義標準化的資料結構和製作介面，以用於建模和建立「事件」。

## 啟用內容片段模型

內容片段模型&#x200B;**必須透過**[ AEM [!UICONTROL 設定瀏覽器]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**啟用。**

如果針對配置啟用了「內容片段模型」 **「不**」，則相關AEM配置將不會顯示「**[!UICONTROL 建立] > [!UICONTROL 內容片段]**」按鈕。

>[!NOTE]
>
>AEM設定代表儲存在`/conf`下的一組[內容感知租用戶設定](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html)。 通常，AEM設定會與AEM Sites中管理的特定網站或負責子集內容（資產、頁面等）的業務單位產生關聯 在AEM中。
>
>為了讓配置影響內容層次結構，必須通過該內容層次結構上的`cq:conf`屬性引用該配置。 （這是在下面的&#x200B;**步驟5**&#x200B;中針對[!DNL WKND Mobile]配置實現的）。
>
>使用`global`設定時，該設定會套用至所有內容，且不需要設定`cq:conf`。
>
>如需詳細資訊，請參閱[[!UICONTROL 設定瀏覽器]檔案](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html) 。

1. 以具有修改相關設定之適當權限的使用者身分登入AEM Author。
   * 在本教學課程中，可使用&#x200B;**admin**&#x200B;使用者。
1. 導覽至&#x200B;**[!UICONTROL Tool] > [!UICONTROL General] > [!UICONTROL Configuration Browser]**
1. 點選&#x200B;**資料夾圖示**&#x200B;並選取&#x200B;**[!DNL WKND Mobile]**&#x200B;旁，然後點選左上角的&#x200B;**[!UICONTROL Edit]按鈕**。
1. 選取「**[!UICONTROL 內容片段模型]**」，然後點選右上角的「**[!UICONTROL 儲存並關閉]**」。

   如此可在套用[!DNL WKND Mobile]設定的資產資料夾內容樹狀結構上啟用內容片段模型。

   >[!NOTE]
   >
   >此配置更改無法從[!UICONTROL AEM Configuration] Web UI中可逆。 若要還原此設定：
   >    
   >    1. 開啟[CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 導航到 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除`models`節點

   >    
   >在此配置下建立的任何現有內容片段模型都將被刪除，其定義將儲存在`/conf/wknd-mobile/settings/dam/cfm/models`下。

1. 將&#x200B;**[!DNL WKND Mobile]**&#x200B;設定套用至&#x200B;**[!DNL WKND Mobile]資產資料夾**，以允許在該Assets資料夾階層內建立內容片段模型：

   1. 導覽至&#x200B;**[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案]**
   1. 選擇&#x200B;**[!UICONTROL WKND Mobile]資料夾**
   1. 點選頂端動作列中的&#x200B;**[!UICONTROL 屬性]**&#x200B;按鈕，以開啟[!UICONTROL 資料夾屬性]
   1. 在[!UICONTROL 資料夾屬性]中，點選&#x200B;**[!UICONTROL Cloud Services]**&#x200B;標籤
   1. 確認「**[!UICONTROL 雲端設定]**」欄位已設為&#x200B;**/conf/wknd-mobile**
   1. 點選右上角的&#x200B;**[!UICONTROL 儲存並關閉]**&#x200B;以保留變更

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## 了解要建立的內容片段模型

在定義「內容片段」模型之前，請先檢閱我們將帶來的體驗，以確保擷取所有必要的資料點。 為此，我們將審核行動應用程式設計，並將設計元素對應至要收集的內容。

我們可依下列方式劃分定義事件的資料點：

![建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

透過對應，我們可以定義內容片段，以用於收集並最終公開事件資料。

## 建立內容片段模型

1. 導覽至&#x200B;**[!UICONTROL 工具] > [!UICONTROL 資產] > [!UICONTROL 內容片段模型]**。
1. 點選&#x200B;**[!DNL WKND Mobile]**&#x200B;資料夾以開啟。
1. 點選&#x200B;**[!UICONTROL 建立]**&#x200B;以開啟「內容片段模型」建立精靈。
1. 輸入&#x200B;**[!DNL Event]**&#x200B;作為&#x200B;**[!UICONTROL 模型標題]** *（說明為可選）*&#x200B;並點選&#x200B;**[!UICONTROL 建立]**&#x200B;以儲存。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## 定義內容片段模型的結構

1. 導覽至&#x200B;**[!UICONTROL 工具] > [!UICONTROL 資產] > [!UICONTROL 內容片段模型] >[!DNL WKND]**。
1. 選取「**[!DNL Event]**&#x200B;內容片段模型」，然後點選頂端動作列中的「**[!UICONTROL 編輯]**」。
1. 從右側的&#x200B;**[!UICONTROL 資料類型]標籤**，將&#x200B;**[!UICONTROL 單行文本輸入]**&#x200B;拖曳至左側拖放區域以定義&#x200B;**[!DNL Question]**&#x200B;欄位。
1. 確保左側選擇了新的&#x200B;**[!UICONTROL 單行文本輸入]**，右側選擇了&#x200B;**[!UICONTROL 屬性]頁簽**。 填入「屬性」欄位，如下所示：

   * [!UICONTROL 呈現為] : `textfield`
   * [!UICONTROL 欄位標籤] : `Event Title`
   * [!UICONTROL 屬性名稱] : `eventTitle`
   * [!UICONTROL 最大長度] :25
   * [!UICONTROL 必要] :  `Yes`

使用下面定義的輸入定義重複這些步驟，以建立其餘的事件內容片段模型。

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

### 法尼奧城

* [!UICONTROL 資料類型] : `Enumeration`
* [!UICONTROL 欄位標籤] : `Venue City`
* [!UICONTROL 屬性名稱] : `venueCity`
* [!UICONTROL 選項] :  `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL 屬性名稱]**&#x200B;表示將儲存此值的JCR屬性名稱，以及JSON檔案中的索引鍵&#x200B;**兩者**。 這應該是在內容片段模型期間不會變更的語義名稱。

完成內容片段模型的建立後，您最後應會得到如下的定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

選擇性地，透過[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 此套件包含本教學課程中概述的設定和內容。

* [第3章 — 編寫事件內容片段](./chapter-3.md)
