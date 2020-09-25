---
title: 第4章——定義內容服務模板
seo-title: AEM Headless快速入門——第4章——定義內容服務範本
description: AEM Headless教學課程的第4章涵蓋AEM Content Services內容中AEM Editable Templates的角色。 可編輯的範本可用來定義AEM Content Services最終將公開的JSON內容結構。
seo-description: AEM Headless教學課程的第4章涵蓋AEM Content Services內容中AEM Editable Templates的角色。 可編輯的範本可用來定義AEM Content Services最終將公開的JSON內容結構。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 0%

---


# 第4章——定義內容服務模板

AEM Headless教學課程的第4章涵蓋AEM Content Services內容中AEM Editable Templates的角色。 「可編輯範本」可用來定義AEM Content Services透過「啟用Content Services的AEM元件」構圖公開給客戶的JSON內容結構。

## 瞭解範本在AEM Content Services中的角色

AEM可編輯範本可用來定義HTTP端點，以便存取該端點以將事件內容顯示為JSON。

傳統上，AEM的「可編輯範本」是用來定義網頁的，但這種使用只是慣例。 可編輯的範本可用來合 **成任** 何內容集；如何存取該內容：在瀏覽器中以HTML格式顯示，JavaScript（AEM SPA編輯器）或行動應用程式所使用的JSON是請求該頁面的函式。

在AEM Content Services中，可編輯的範本可用來定義JSON資料的公開方式。

對於應 [!DNL WKND Mobile] 用程式，我們將建立單一可編輯範本，用來驅動單一API端點。 雖然此範例可簡單說明AEM Headless的概念，但您可以建立多個頁面（或端點），每個頁面都會顯示不同的內容集，以建立更複雜、組織更有條理的API。

## 瞭解API端點

若要瞭解如何編寫API端點，並瞭解應將哪些內容公開至我們的應 [!DNL WKND Mobile] 用程式，請讓我們重新造訪設計。

![事件API頁面分解](./assets/chapter-4/design-to-component-mapping.png)

如我們所見，我們有三組邏輯內容要提供給行動應用程式。

1. The **Logo**
2. 標 **簽行**
3. 事件清 **單**

若要這麼做，我們可以將這些需求對應至「AEM元件」（在本例中為「AEM WCM核心元件」），以便將必要的內容顯示為JSON。

1. 標 **志將** 透過影像元 **件呈現**
2. 標 **簽行** 將通過文本組 **件呈現**
3. 事件清 **單將透過內容** 片段清單元件呈現 **** ，而此元件反過來參照一組事件內容片段。

>[!NOTE]
>
>若要支援AEM Content Service的「頁面與元件」JSON匯出，「頁面與元件」必 **須衍生自AEM WCM核心元件**。
>
>[AEM的WCM核心元件具備內建功能](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) ，可支援轉存頁面和元件的標準化JSON結構描述。 本教學課程中使用的所有WKND Mobile元件（頁面、影像、文字和內容片段清單）都衍生自AEM的WCM核心元件。

## 定義事件API範本

1. 導覽至「 **[!UICONTROL 工具]>一[!UICONTROL 般]>范[!UICONTROL 本]>[!DNL WKND Mobile]**」。

1. 建立范 **[!DNL Events API]** 本：

   1. 點選 **[!UICONTROL 頂端動作列中的]** 「建立」
   1. 選擇范 **[!DNL WKND Mobile - Empty Page]** 本
   1. 在頂端 **[!UICONTROL 動作列中點選]** 「下一步」
   1. 在「 **[!DNL Events API]** 範本標 [!UICONTROL 題」欄位中輸入]
   1. 點選 **[!UICONTROL 頂端動作列中的]** 「建立」
   1. 點選 **[!UICONTROL 開啟]** ，開啟新範本以進行編輯

1. 首先，我們允許我們需要的三個已識別的AEM元件，透過編輯根版面配置容器的 [!UICONTROL Policy] ，來建 [!UICONTROL 立內容模型]。 確保「結 **[!UICONTROL 構]** 」模式處於活動狀態，選擇 **[!DNL Layout Container \[Root\]]**&#x200B;並點選「策略 **** 」按鈕。
1. 在「屬 **[!UICONTROL 性]>允[!UICONTROL 許的元件]** 」下搜尋 **[!DNL WKND Mobile]**。 允許從元件群組 [!DNL WKND Mobile] 使用下列元件，以便在 [!DNL Events] API頁面上使用。

   * **[!DNL WKND Mobile > Image]**

      * 應用程式的標誌
   * **[!DNL WKND Mobile > Text]**

      * 應用程式的入門文字
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在應用程式中顯示的「事件」類別清單



1. 完成 **[!UICONTROL 時]** ，點選右上角的「完成」核取標籤。
1. **重新整理** 瀏覽器視窗，以在左側  導軌中查看新允許的元件清單。
1. 從左側導軌的「元件搜尋器」中，拖曳下列AEM元件：
   1. **[!DNL Image]** 標誌
   2. **[!DNL Text]** 標籤行
   3. **[!DNL Content Fragment List]** 活動
1. **對於上述每個元件**，選擇它們並按 **解鎖** 按鈕。
1. 不過，請確定 **版面容器已鎖定** , **** 以防止新增其他元件，或移除這三個元件。
1. 點選 **[!UICONTROL 「頁面資訊]>管理[!UICONTROL 員中的檢視]** 」可返回 [!DNL WKND Mobile] 範本清單。 選取新建立的范 **[!DNL Events API]** 本，然後點選 **[!UICONTROL 頂端動作列中的]** 「啟用」。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 請注意，用於呈現內容的元件會新增至範本本身，並鎖定。 這可讓作者編輯預先定義的元件，但不能任意新增或移除元件，因為變更API本身可能會打破JSON結構的假設，並中斷使用應用程式。 所有API都必須穩定。

## 後續步驟

（可選）透過 [AEM的Package Manager，在AEM Author上安裝](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip [content套件](http://localhost:4502/crx/packmgr/index.jsp)。 本套件包含教學課程本章及前幾章中概述的設定和內容。

* [第5章——編寫內容服務頁面](./chapter-5.md)
