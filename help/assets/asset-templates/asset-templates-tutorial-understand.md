---
title: '瞭解AEM Assets的InDesign檔案和資產模板 '
description: 本教學課程影片將逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以便用於AEM Assets的「資產範本」功能。
version: 6.3, 6.4, 6.5
topic: 內容管理
role: 業務從業人員
level: 中級
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---


# 瞭解AEM Assets的InDesign檔案和資產模板{#understanding-indesign-files-and-asset-templates-in-aem-assets}

本教學課程影片將逐步說明如何定義InDesign檔案，以及所有隨附的考量事項，以便用於AEM Assets的「資產範本」功能。

## 構建InDesign模板檔案{#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下載並開啟&#x200B;[**InDesign檔案模板**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **開啟「標籤」面** 板，檢閱「標籤命名」慣例，請注意InDesign檔案中的可作者元素已標籤。請記住，只有標籤的元素可在中編AEM輯。

   * **「視窗>公用程式>標籤」**

3. 在頁面上，新增文字元素，提供「標題」文字，並套用&#x200B;**標題**&#x200B;段落樣式。

   * **「視窗>樣式>段落樣式」**

   然後，建立並套用名為&#x200B;**Page2Heading.**&#x200B;的新標籤

4. 將FPO標誌影像（位於zip](assets/asset-templates-tutorial-video--supporting-files.zip)中的[）新增至主版頁面上的標誌元素。

   * **按一下右鍵**&#x200B;並選&#x200B;**取「管接頭」(Fitting)>「框架管接頭選項……」(Frame Fitting Options...)>內容調整>等比例填滿影格**
   [進一步瞭解「影格調整」選項](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，以及哪些選項最適合您的使用案例。

5. 從「頁面」和「頁面」中的「主版」範本，透過「就地貼上」下載頁首（標誌和公司名稱）。

   * 在第1頁上，在macOS上按住Shift-Cmd-Click或在Windows上按住Shift-Alt-Click，以選取「主版」頁面中公開的「標題」，並加以刪除。
   * 從「主版」頁面，透過「就地貼上」將頁首複製到第1頁
   * 重複第2頁的步驟

6. 在「結構」(Structure)面板上按兩下每個元素，確保所有結構元素都與InDesign檔案中的實際元素對應。 移除任何未使用或不需要的元素。 請確定所有標籤都是語義的，並正確標籤元素。

   >[!NOTE]
   >
   >請記住，InDesign檔案的建構不當是造成資產範本問題的最常見原因AEM，因此請確定標籤和結構是乾淨且正確的。

## 在AEM Assets{#creating-and-authoring-an-asset-template-in-aem-assets}中建立和編寫資產模板

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **啟動InDesign** 伺服器埠8080。
2. 請確定&#x200B;**AEM Author例項已設定為與您的InDesign Server**&#x200B;互動（反之亦然）。

   * [IDS工作Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [雲端代理Cloud Service設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi配置](http://localhost:4502/system/console/configMgr)

3. **已將InDesign檔案上傳AEM至資產，AEM並允許工作流程和InDesign Server完全處理資產。** 
4. **在「資產>范** 本」下 **建立新** 的範本，並選取在步驟#4中上傳AEM至的InDesign檔案。
5. **編輯在步驟#5** 中建立的資產範本，並編寫可編輯的欄位。
6. 按一下「**完成**」，產生「資產範本」的最終高精確轉譯。
7. 按一下「資產範本」卡片以開啟，並檢閱「資產轉譯」以下載高精確轉譯。

## 其他資源 {#additional-resources}

InDesign範本檔案與支援影像

下載[InDesign範本檔案並支援Images](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC試用版下載](https://creative.adobe.com/products/download/indesign)
* [InDesign Server試用版下載](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
