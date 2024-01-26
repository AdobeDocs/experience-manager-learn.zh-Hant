---
title: 瞭解AEM Assets中的InDesign檔案和資產範本
description: 本影片教學課程會逐步解說如何定義AEM Assets資產範本功能中使用的InDesign檔案，以及隨附的所有考量事項。
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 941
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# 瞭解AEM Assets中的InDesign檔案和資產範本 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本影片教學課程會逐步解說如何定義AEM Assets資產範本功能中使用的InDesign檔案，以及隨附的所有考量事項。

## 建構InDesign範本檔案 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. 下載並開啟 [**InDesign檔案範本**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **開啟「標籤」面板，** 請檢閱標籤命名慣例，並注意InDesign檔案中的可作者元素已加上標籤。 請記住，在AEM中只能編輯已標籤的元素。

   * **「視窗>公用程式>標籤」**

3. 在頁面上，新增文字元素，提供文字「Header」並套用 **標題** 段落樣式。

   * **視窗>樣式>段落樣式**

   然後，建立並套用名為的新標籤 **Page2Heading。**

4. 新增FPO標誌影像([提供於zip中](assets/asset-templates-tutorial-video--supporting-files.zip))至主版頁面上的Logo元素。

   * **按一下右鍵**&#x200B;並選取&#x200B;**「符合」>「框架符合」選項…… >「內容符合」>「按比例填滿框架」**

   [深入瞭解框架彎管頭選項](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，，且適用於您的使用案例。

5. 透過「就地貼上」，從頁面和頁面中的主版範本複製頁首（標誌和公司名稱）。

   * 在第1頁上，按住Shift鍵並按一下macOS或Shift鍵並按一下Windows，以選取主版頁面所公開的頁首，然後將其刪除。
   * 從「主版」頁面，透過「就地貼上」將頁首複製到第1頁
   * 針對第2頁重複這些步驟

6. 開啟「結構」面板，連按兩下每個面板，確定所有結構元素都對應至InDesign檔案中的真實元素。 移除任何未使用或不需要的元素。 請確定所有標籤都是語意標籤，且元素已正確標籤。

   >[!NOTE]
   >
   >請記住，InDesign檔案建構不當是AEM資產範本問題最常見的原因，因此請確保標籤和結構乾淨且正確。

## 在AEM Assets中建立和編寫資產範本 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **開始InDesign Server** 在連線埠8080上。
2. 確保 **AEM編寫執行個體已設定為與您的InDesign Server互動**（反之亦然）。

   * [IDS背景工作Cloud Service設定](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [雲端ProxyCloud Service設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi設定](http://localhost:4502/system/console/configMgr)

3. **已將InDesign檔案上傳至AEM Assets** 並允許AEM工作流程和InDesign Server完整處理資產。
4. **建立新範本** 在 **資產>範本** 並選取在步驟#4中上傳至AEM的InDesign檔案。
5. **編輯資產範本** 在步驟#5中建立，並編寫可編輯欄位。
6. 按一下 **完成** 以產生「資產範本」的最終高傳真轉譯。
7. 按一下「資產範本」卡以開啟，並檢閱「資產轉譯」以下載高傳真轉譯。

## 其他資源 {#additional-resources}

InDesign範本檔案和支援的影像

下載 [InDesign範本檔案和支援的影像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC試用版下載](https://creative.adobe.com/products/download/indesign)
* InDesign Server試用版可從以下網址下載： [Adobe發行前網站](https://www.adobeprerelease.com/) 或 [CC Enterprise客戶可以聯絡其帳戶管理員，要求InDesign Server試用版授權](https://www.adobe.com/products/indesignserver/faq.html)
