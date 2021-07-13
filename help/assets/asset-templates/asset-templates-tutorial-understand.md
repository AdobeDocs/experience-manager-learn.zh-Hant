---
title: '了解AEM Assets中的InDesign檔案和資產範本 '
description: 本教學課程影片會逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以用於AEM Assets的「資產範本」功能。
version: 6.3, 6.4, 6.5
topic: 內容管理
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---


# 了解AEM Assets中的InDesign檔案和資產範本 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本教學課程影片會逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以用於AEM Assets的「資產範本」功能。

## 建立InDesign範本檔案 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下載並開啟&#x200B;[**InDesign檔案範本**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **開啟「標籤」面板，** 檢閱「標籤」命名慣例，並注意InDesign檔案中可製作的元素已經過標籤。請記住，AEM中只有已標籤的元素可編輯。

   * **「窗口」>「實用程式」>「標籤」**

3. 在「頁面」上，添加新的文本元素，提供文本&quot;Header&quot;，並應用&#x200B;**Heading**&#x200B;段落樣式。

   * **窗口>樣式>段落樣式**

   然後，建立並應用名為&#x200B;**Page2Heading.**&#x200B;的新標籤

4. 將FPO標誌影像（[，在zip](assets/asset-templates-tutorial-video--supporting-files.zip)中提供）新增至主版頁面上的Logo元素。

   * **按一下右**&#x200B;鍵，**然後按一下「管接頭」(Fitting)>「框架管接頭選項……」(Frame Fitting Options...)>「內容管接頭」(Content Fitting)>「按比例填充框架」(Fill Frame)**
   [深入了解「框架管接頭」選項](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，以及適合您的使用案例。

5. 透過就地貼上，從「頁面」和「頁面」的「主版」範本下複製標題（標誌和公司名稱）。

   * 在第1頁上，按住Shift鍵並按一下macOS上的，或在Windows上按住Shift鍵並按一下，以選擇從「主版」頁公開的「標題」，然後將其刪除。
   * 從「主版」頁，通過「就地貼上」將頁首複製到第1頁
   * 重複第2頁的步驟

6. 開啟「結構」面板，按兩下每個面板，確保所有結構元素都與InDesign檔案中的實際元素對應。 移除任何未使用或不需要的元素。 請確定所有標籤都是語意，且元素標籤正確。

   >[!NOTE]
   >
   >請記住，建構不良的InDesign檔案是造成AEM資產範本問題最常見的原因，因此請確定標籤和結構是乾淨且正確的。

## 在AEM Assets中建立和編寫資產範本 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **啟動InDesign** 伺服器埠8080。
2. 確認&#x200B;**AEM Author例項已設定為與您的InDesign Server**&#x200B;互動（反之亦然）。

   * [IDS工作Cloud Service設定](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [雲端代理Cloud Service設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi設定](http://localhost:4502/system/console/configMgr)

3. **已上傳InDesign檔案至AEM** Assets，並允許AEM工作流程和InDesign Server完全處理資產。
4. **在「資產>** 範本」 **下建立新** 範本，並在步驟#4中選取上傳至AEM的InDesign檔案。
5. **編輯在步** 驟#5中建立的資產範本，並編寫可編輯的欄位。
6. 按一下&#x200B;**Done**&#x200B;以產生資產範本的最終高保真轉譯。
7. 按一下「資產範本」卡片以開啟，並檢閱「資產轉譯」以下載高保真轉譯。

## 其他資源 {#additional-resources}

InDesign範本檔案和支援的影像

下載[InDesign範本檔案並支援影像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC試用版下載](https://creative.adobe.com/products/download/indesign)
* [CC Enterprise客戶可以聯絡其客戶經理以要求取得InDesign Server試用授權](https://www.adobe.com/products/indesignserver/faq.html)
