---
title: 建立AEM工作流程
description: 使用AEM Forms工作流程元件建立AEM Workflow模型，以檢視已提交的資料。
sub-product: 表單
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 4271
thumbnail: kt-4271.jpg
translation-type: tm+mt
source-git-commit: 738e356c4e72e0c3518bb5fd4ad6a076522e9f5c
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---


# 建立工作流程以檢閱提交的資料

工作流程通常用來傳送已提交的資料以供審查和核准。 工作流程是使用AEM中的工作流程編輯器來建立。 可在提交最適化表單時觸發工作流程。 下列步驟將引導您完成建立第一個工作流程的程式。

## 先決條件

請確定您有AEM Forms的運作實例。 請依照安 [裝指南](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) ，安裝和設定AEM Forms


## 建立工作流程模型

* [開放的工作流程模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
* 按一下「建立」(Create)->「建立模型」(Create Model)
* 提供有意義的標題和名稱，例如「檢閱 _提交的資料」_。
* 輕輕點選新建立的工作流程，然後按一下「編 _輯_ 」圖示。
* 工作流程會以編輯模式開啟。 預設情況下，工作流有一個名為 _Step1的元件_。 選擇此元件並按一下刪除表徵圖可刪除該元件。
* 左側列出了可用於構建工作流的各種工作流元件。 您可以依「表單工作流程」類 _型來篩選元件_ 。

## 建立變數

* 按一下變數的圖示以建立新變數。 變數可用來儲存值。 AEM Forms提供許多可建立的變數類型。 今天，我們將建立XML類型的變數，以保存Adaptive Form提交的資料。 建立名為submittedData _的XML類型_ ，如下圖所示。

   >[!NOTE]
如果表單是以表單資料模型為基礎，則提交的資料為JSON格式，此時您將建立JSON類型的變數，以儲存提交的資料。

![提交的資料變數](assets/submitted-data-variable.PNG)

* 按一下左 _側的_ 「步驟」圖示，列出各種工作流程元件。 將「設定變 _數」元件拖放_ ，至右側的工作流程。 請務必將「設定變 _數」元件置於_ 「流程開始」下方。
   * 按一下「 _Set Variable_ 」(設定變數 _)元件，然後按一下「_ Wrench」（扳手）圖示以開啟元件的屬性表單。
   * 按一下「映射」頁籤->「添加映射」->「映射變數」。 設定如下螢幕擷取畫面中所示的值。
      ![建立變數](assets/set-variable.PNG)

## 新增工作流程元件

* 將Assign Task _(指定任_ 務 _)元件拖放到Set Variable（設定變數）元件下_ 方的右側。
   * 按一下「指 _派工作_ 」元件，然後按一下「扳手」 __ 圖示以開啟屬性工作表。
   * 為「指派任務」元件提供有意義的標題。
   * 按一下「表單與檔案」標籤，並設定下列屬性，如螢幕擷取畫面中所示
      ![「表單文檔」頁籤](assets/forms-documents.PNG)

   * _1通過選擇此選項，工作流不會與特定的自適應表單相耦合。_
   * _2工作流程引擎會相對於儲存庫中的裝載尋找名為Data.xml的檔案_

   * 按一下「受託人」標籤。 您可以在這裡將任務指派給組織中的使用者。 對於此使用案例，我們將指派任務給管理員用戶，如下面的螢幕抓圖所示。
      ![受託人標籤](assets/assignee-tab.PNG)
   * 按一下元件的「完成 __ 」圖示，儲存變更
* 按一下「 _同步_ 」，產生工作流程的執行階段模型。
您的工作流程模型現在已就緒，可與Adaptive Form的提交操作關聯。



