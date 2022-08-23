---
title: 瞭解AEM Assets的InDesign檔案和資產模板
description: 本視頻教程將介紹定義InDesign檔案以及所有附帶的注意事項，以用於AEM Assets的「資產模板」功能。
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 0%

---

# 瞭解AEM Assets的InDesign檔案和資產模板 {#understanding-indesign-files-and-asset-templates-in-aem-assets}

本視頻教程將介紹定義InDesign檔案以及所有附帶的注意事項，以用於AEM Assets的「資產模板」功能。

## 構造InDesign模板檔案 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. 下載並開啟 [**InDesign檔案模板**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **開啟「標籤」面板，** 查看「標籤命名」約定，並注意InDesign檔案中的可作者元素已被標籤。 請記住，只有標籤的元素可在中編AEM輯。

   * **「窗口」(Window)>「實用程式」(Utilities)>「標籤」(Tags)**

3. 在頁面上，添加新文本元素，提供文本「標題」並應用 **標題** 段落樣式。

   * **「窗口」>「樣式」>「段落樣式」**

   然後，建立並應用名為 **Page2標題。**

4. 添加FPO徽標影像([在郵政編輯中提供](assets/asset-templates-tutorial-video--supporting-files.zip))到母版頁上的Logo元素。

   * **按一下右鍵**&#x200B;選擇&#x200B;**「管接頭」(Fitting)>「框架管接頭選項」(Frame Fitting Options)。..>內容管接頭>按比例填充框架**
   [瞭解有關幀管接頭選項的詳細資訊](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)，這適合您的用例。

5. 通過「貼上到位」從頁面和頁面的母版模板中複製標題（徽標和公司名稱）。

   * 在第1頁上，按住Shift-Cmd鍵並按一下macOS或按住Shift-Alt鍵按一下Windows，選擇母版頁中顯示的標題，然後將其刪除。
   * 從母版頁中，通過「貼上到位」將頁眉複製到第1頁
   * 重複第2頁的步驟

6. 通過按兩下每個結構，開啟「結構」面板，確保所有結構元素都與InDesign檔案中的實際元素相對應。 刪除任何未使用或未需要的元素。 確保所有標籤都是語義的，並且元素已正確標籤。

   >[!NOTE]
   >
   >請記住，構造不良的InDesign檔案是導致「資產模板」問題的最AEM常見原因，因此請確保標籤和結構乾淨且正確。

## 在AEM Assets建立和建立資產模板 {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **開始InDesign Server** 在8080埠。
2. 確保 **AEM Author實例配置為與您的InDesign Server交互**（反之亦然）。

   * [IDS工作Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [雲代理Cloud Service配置](http://localhost:4502/etc/cloudservices/proxy.html)
   * [外AEM部化器OSGi配置](http://localhost:4502/system/console/configMgr)

3. **已將InDesign檔案上載到AEM Assets** 並允AEM許工作流和InDesign Server完全處理資產。
4. **建立新模板** 在 **資產>模板** 並選擇步驟#4中上載到AEM的InDesign檔案。
5. **編輯資產模板** 在步驟#5中建立，並建立可編輯欄位。
6. 按一下 **完成** 生成「資產模板」的最終高保真格式副本。
7. 按一下「資產模板」卡以開啟，然後查看「資產格式副本」以下載高保真格式副本。

## 其他資源 {#additional-resources}

InDesign模板檔案和支援映像

下載 [InDesign模板檔案和支援映像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC試用下載](https://creative.adobe.com/products/download/indesign)
* InDesign Server試用可從 [Adobe預發行站點](https://www.adobeprerelease.com/) 或 [CC企業客戶可以聯繫其客戶主管請求獲得InDesign Server試用許可證](https://www.adobe.com/products/indesignserver/faq.html)
