---
title: '瞭解AEM Assets中的InDesign檔案和資產範本 '
description: 本教學課程影片將逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以用於AEM Assets的「資產範本」功能。
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# 瞭解AEM Assets中的InDesign檔案和資產範本 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本教學課程影片將逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以用於AEM Assets的「資產範本」功能。

## 建構InDesign範本檔案 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下載並開啟 [**InDesign檔案範本**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **開啟「標籤」面板** ，檢閱「標籤命名」慣例，並注意InDesign檔案中的可作者元素已標籤。 請記住，AEM中只有標籤的元素是可編輯的。

   * **「視窗>公用程式>標籤」**

3. 在頁面上新增文字元素、提供「頁首」文字並套用「標題 **段落** 」樣式。

   * **「視窗>樣式>段落樣式」**

   然後，建立並套用名為 **Page2Heading的新標籤。**

4. 將FPO標誌影像([位於郵遞區號中](assets/asset-templates-tutorial-video--supporting-files.zip))新增至主版頁面的標誌元素。

   * **按一下右鍵**&#x200B;並選取「管接頭&#x200B;**」(Fitting)>「框架管接頭選項……」(Frame Fitting Options...)>內容調整>等比例填滿影格**
   [進一步瞭解「影格調整」選項](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，以及哪些選項最適合您的使用案例。

5. 從「頁面」和「頁面」中的「主版」範本，透過「就地貼上」下載頁首（標誌和公司名稱）。

   * 在第1頁上，在macOS上按住Shift-Cmd-Click或在Windows上按住Shift-Alt-Click，以選取「主版」頁面中公開的「標題」，並加以刪除。
   * 從「主版」頁面，透過「就地貼上」將頁首複製到第1頁
   * 重複第2頁的步驟

6. 開啟「結構」面板，方法是連按兩下每個面板，確保所有結構元素都與InDesign檔案中的實際元素對應。 移除任何未使用或不需要的元素。 請確定所有標籤都是語義的，並正確標籤元素。

   >[!NOTE]
   >
   >請記住，建構不良的InDesign檔案是導致AEM資產範本問題的最常見原因，因此請確定標籤和結構是乾淨且正確的。

## 在AEM Assets中建立和編寫資產範本 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **在連接埠** 8080上啟動InDesign Server。
2. 請確定 **AEM Author執行個體已設定為與您的InDesign Server**（反之亦然）互動。

   * [IDS Worker Cloud服務配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [雲端代理雲端服務設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi組態](http://localhost:4502/system/console/configMgr)

3. **已將InDesign檔案上傳至AEM Assets** ，並允許AEM Workflow和InDesign Server完全處理資產。
4. **在「資產** >範本」 **下建立新的範本** ，並在步驟#4中選取上傳至AEM的InDesign檔案。
5. **編輯在步驟** #5中建立的資產範本，並編寫可編輯的欄位。
6. 按一 **下「完成** 」，產生「資產範本」的最終高精確轉譯。
7. 按一下「資產範本」卡片以開啟，並檢閱「資產轉譯」以下載高精確轉譯。

## 其他資源 {#additional-resources}

InDesign範本檔案與支援影像

下載 [InDesign範本檔案和支援影像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesign CC試用版下載](https://creative.adobe.com/products/download/indesign)
* [InDesign Server試用版下載](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
