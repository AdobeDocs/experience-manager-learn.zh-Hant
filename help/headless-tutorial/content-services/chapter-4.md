---
title: 第4章——定義內容服務模板——內容服務
seo-title: 無頭入門AEM-第4章——定義內容服務範本
description: 「無頭教學AEM課程」的第4章介紹「可編AEM輯範本」在「內容服務」中AEM的角色。 可編輯的範本可用來定義Content Services最終將公開AEM的JSON內容結構。
seo-description: 「無頭教學AEM課程」的第4章介紹「可編AEM輯範本」在「內容服務」中AEM的角色。 可編輯的範本可用來定義Content Services最終將公開AEM的JSON內容結構。
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '837'
ht-degree: 0%

---


# 第4章——定義內容服務模板

「無頭教學AEM課程」的第4章介紹「可編AEM輯範本」在「內容服務」中AEM的角色。 「可編輯範本」可用來定義「內容服務」透AEM過「啟用內容服務的元件」構成公開給客戶的JSON內容AEM結構。

## 瞭解範本在Content Services中AEM的角色

可AEM編輯範本用來定義將存取的HTTP端點，以將事件內容顯示為JSON。

傳統AEM上，可編輯範本用於定義網頁，但這只是慣例。 可編輯的範本可用來合成&#x200B;**任何**&#x200B;內容集；如何存取該內容：在瀏覽器中以HTML格式顯示，因為JavaScript(AEM編輯SPA器)或行動應用程式所使用的JSON是請求該頁面的方式的函式。

在Content AEM Services中，可編輯的範本可用來定義JSON資料的公開方式。

對於[!DNL WKND Mobile]應用程式，我們將建立單一可編輯範本，用來驅動單一API端點。 雖然此範例可簡單說明AEMHeadless的概念，但您可以建立多個頁面（或端點），每個頁面都會顯示不同的內容集，以建立更複雜、組織更好的API。

## 瞭解API端點

若要瞭解如何編寫API端點，並瞭解哪些內容應公開至我們的[!DNL WKND Mobile]應用程式，請讓我們重新造訪設計。

![事件API頁面分解](./assets/chapter-4/design-to-component-mapping.png)

如我們所見，我們有三組邏輯內容要提供給行動應用程式。

1. **標誌**
2. **標籤行**
3. **Events**&#x200B;的清單

若要這麼做，我們可將這些需求對應AEM至「元件」(在本例中為AEM「WCM核心元件」)，以便將必要的內容顯示為JSON。

1. **標誌**&#x200B;將通過&#x200B;**影像元件**&#x200B;呈現
2. **標籤行**&#x200B;將通過&#x200B;**文本元件**&#x200B;呈現
3. **事件**&#x200B;的清單將透過&#x200B;**內容片段清單元件**&#x200B;呈現，而該元件又參照一組事件內容片段。

>[!NOTE]
>
>若要支AEM援Content Service的「頁面與元件」JSON匯出，「頁面與元件」必須從AEMWCM核心元件&#x200B;**衍生。**
>
>[WCMAEM核心元](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 件內建功能，可支援轉存頁面和元件的標準化JSON結構描述。本教學課程中使用的所有WKND Mobile元件（頁面、影像、文字和內容片段清單）都衍生自AEMWCM核心元件。

## 定義事件API範本

1. 導覽至「**[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**」。

1. 建立&#x200B;**[!DNL Events API]**&#x200B;模板：

   1. 點選頂端動作列中的「建立」****
   1. 選擇&#x200B;**[!DNL WKND Mobile - Empty Page]**&#x200B;模板
   1. 在頂端動作列中點選&#x200B;**[!UICONTROL Next]**
   1. 在[!UICONTROL 範本標題]欄位中輸入&#x200B;**[!DNL Events API]**
   1. 點選頂端動作列中的「建立」****
   1. 點選&#x200B;**[!UICONTROL 開啟]**&#x200B;開啟新範本以進行編輯

1. 首先，我們允許所AEM需的三個已識別的元件，透過編輯根[!UICONTROL 配置容器]的[!UICONTROL Policy]來建立內容模型。 確保&#x200B;**[!UICONTROL Structure]**&#x200B;模式處於活動狀態，選擇&#x200B;**[!DNL Layout Container \[Root\]]** ，然後按一下&#x200B;**[!UICONTROL Policy]**&#x200B;按鈕。
1. 在「**[!UICONTROL 屬性] > [!UICONTROL 允許的元件]**」下搜索&#x200B;**[!DNL WKND Mobile]**。 允許[!DNL WKND Mobile]元件組中的以下元件，以便在[!DNL Events] API頁上使用。

   * **[!DNL WKND Mobile > Image]**

      * 應用程式的標誌
   * **[!DNL WKND Mobile > Text]**

      * 應用程式的入門文字
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在應用程式中顯示的「事件」類別清單



1. 完成時，點選右上角的&#x200B;**[!UICONTROL Done]**&#x200B;核取標籤。
1. **重** 新整理瀏覽器視窗，在左 [!UICONTROL 側] 導軌中查看新允許的元件清單。
1. 從左側導軌的「元件搜尋器」中，拖曳下列元AEM件：
   1. **[!DNL Image]** 標誌
   2. **[!DNL Text]** 標籤行
   3. **[!DNL Content Fragment List]** 活動
1. **對於上述每個元件**，選擇它們並按解除 **** 鎖定按鈕。
1. 不過，請確定&#x200B;**版面配置容器**&#x200B;是&#x200B;**已鎖定**，以防止新增其他元件，或移除這三個元件。
1. 點選「**[!UICONTROL 頁面資訊] > [!UICONTROL 在管理]**&#x200B;中檢視」，返回[!DNL WKND Mobile]範本清單。 選擇新建立的&#x200B;**[!DNL Events API]**&#x200B;模板，然後在頂部操作欄中按一下&#x200B;**[!UICONTROL 啟用]**。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 請注意，用於呈現內容的元件會新增至範本本身，並鎖定。 這可讓作者編輯預先定義的元件，但不能任意新增或移除元件，因為變更API本身可能會打破JSON結構的假設，並中斷使用應用程式。 所有API都必須穩定。

## 後續步驟

（可選）透過[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)，在AEM Author上安裝[com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)內容套件。 本套件包含教學課程本章及前幾章中概述的設定和內容。

* [第5章——編寫內容服務頁面](./chapter-5.md)
