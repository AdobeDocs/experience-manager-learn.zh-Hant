---
title: 了解AEM Assets中的InDesign檔案和資產範本
description: 本教學課程影片會逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以用於AEM Assets的「資產範本」功能。
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# 了解AEM Assets中的InDesign檔案和資產範本 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本教學課程影片會逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以用於AEM Assets的「資產範本」功能。

## 建立InDesign範本檔案 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. 下載並開啟 [**InDesign檔案範本**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **開啟「標籤」面板，** 請檢閱「標籤」命名慣例，並注意InDesign檔案中可製作的元素已經經過標籤。 請記住，AEM中只有已標籤的元素可編輯。

   * **「窗口」>「實用程式」>「標籤」**

3. 在頁面上，新增文字元素、提供「Header」文字並套用 **標題** 段落樣式。

   * **窗口>樣式>段落樣式**

   然後，建立並套用名為 **Page2Heading.**

4. 添加FPO徽標影像([郵遞區號中提供](assets/asset-templates-tutorial-video--supporting-files.zip))到「主版」頁面上的「標誌」元素。

   * **按一下右鍵**&#x200B;選取&#x200B;**「管接頭」(Fitting)>「框架管接頭選項」(Frame Fitting Options...)>「內容管接頭」(Content Fitting)>「按比例填充框架」(Fill Frame)**
   [進一步了解框架管接頭選項](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，哪個適合您的使用案例。

5. 從「頁面」和「頁面」的「主版」範本，透過「就地貼上」複製標題（標誌和公司名稱）。

   * 在第1頁上，按住Shift鍵並按一下macOS，或在Windows上按住Shift鍵並按一下Alt鍵，以選擇「主版」頁面公開的標題，並將其刪除。
   * 從「主版」頁，通過「就地貼上」將頁首複製到第1頁
   * 重複第2頁的步驟

6. 開啟「結構」面板，按兩下每個面板，確保所有結構元素都與InDesign檔案中的實際元素相對應。 移除任何未使用或不需要的元素。 請確定所有標籤都是語意，且元素標籤正確。

   >[!NOTE]
   >
   >請記住，建構不良的InDesign檔案是造成AEM資產範本問題最常見的原因，因此請確定標籤和結構是乾淨且正確的。

## 在AEM Assets中建立和編寫資產範本 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **開始InDesign Server** 在8080埠。
2. 確保 **AEM Author例項已設定為與您的InDesign Server互動**（反之亦然）。

   * [IDS工作Cloud Service設定](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [雲端代理Cloud Service設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi設定](http://localhost:4502/system/console/configMgr)

3. **已上傳InDesign檔案至AEM Assets** 並允許AEM Workflow和InDesign Server完全處理資產。
4. **建立新範本** 在 **資產>範本** 並選取在步驟#4中上傳至AEM的InDesign檔案。
5. **編輯資產範本** 在步驟#5中建立，並編寫可編輯的欄位。
6. 按一下 **完成** 以產生資產範本的最終高保真轉譯。
7. 按一下「資產範本」卡片以開啟，並檢閱「資產轉譯」以下載高保真轉譯。

## 其他資源 {#additional-resources}

InDesign範本檔案和支援的影像

下載 [InDesign範本檔案和支援的影像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC試用版下載](https://creative.adobe.com/products/download/indesign)
* InDesign Server試用版可從 [Adobe發行前網站](https://www.adobeprerelease.com/) 或 [CC Enterprise客戶可以聯絡其客戶經理，要求獲得InDesign Server試用許可](https://www.adobe.com/products/indesignserver/faq.html)
