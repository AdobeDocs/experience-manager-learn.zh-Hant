---
title: 在AEM中建立LaunchCloud Service設定
description: 了解如何在AEM中建立LaunchCloud Service設定。 接著，LaunchCloud Service設定便可套用至現有的網站，且在製作和發佈環境中都可看見載入標籤程式庫。
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# 在AEM中建立LaunchCloud Service設定 {#create-launch-cloud-service}

>[!NOTE]
>
>AEM產品UI、內容和檔案中已實作將Adobe Experience Platform Launch重新命名為一組資料收集技術的程式，因此此處仍使用Launch一詞。

了解如何在Adobe Experience Manager中建立LaunchCloud Service設定。 AEM LaunchCloud Service設定隨後可套用至現有的網站，且在製作和發佈環境中都可看見載入標籤程式庫。

## 建立Launch雲端服務

使用下列步驟建立Launch雲端服務設定。

1. 從 **工具** 菜單，選擇 **Cloud Services** 區段，按一下 **Adobe啟動設定**

1. 選取您網站的設定資料夾或選取 **WKND站點** （若使用WKND指南專案），然後按一下 **建立**

1. 從 _一般_ 標籤，使用 **標題** 欄位，然後選取 **Adobe啟動** 從 _相關聯的Adobe IMS設定_ 下拉式清單。 然後，從 _公司_ 下拉式清單，然後從中選取先前建立的屬性 _屬性_ 下拉式清單。

1. 從 _測試_ 和 _生產_ 標籤保留預設配置。 不過，建議您檢閱並變更實際生產設定的設定，尤其是 _非同步載入程式庫_ 根據您的效能和最佳化需求切換。 另請注意， _庫URI_ 「測試」和「生產」的值不同。

1. 最後，按一下 **建立** 來完成Launch雲端服務。

   ![啟動Cloud Services設定](assets/launch-cloud-services-config.png)

## 將Launch雲端服務套用至網站

若要將Tag屬性及其程式庫載入AEM網站，則會將Launch雲端服務設定套用至網站。 在上一步中，雲端服務設定是在網站名稱資料夾（WKND網站）下建立，因此應該自動套用，讓我們驗證一下。

1. 從 **導覽** 菜單，選擇 **網站** 表徵圖。

1. 選取AEM網站的根頁面，然後按一下 **屬性**. 接著，導覽至 **進階** 標籤和下方 **設定** 區段中，確認雲端設定值指向您的網站特定 `conf` 檔案夾。

   ![將Cloud Services配置應用到站點](assets/apply-cloud-services-config-to-site.png)

## 驗證在製作和發佈頁面上載入標籤屬性

現在是時候驗證Tag屬性及其程式庫已載入AEM網站頁面上了。

1. 在 **檢視為已發佈** 模式中，您應會在瀏覽器主控台中看到記錄訊息。 這是來自標籤屬性規則的JavaScript程式碼片段所傳送的相同訊息，當 _程式庫已載入（頁面頂端）_ 事件觸發。

1. 若要在發佈時驗證，請先發佈 **Launch雲端服務** 設定，並在Publish執行個體上開啟網站頁面。

   ![製作和發佈頁面上的標籤屬性](assets/tag-property-on-author-publish-pages.png)

恭喜！您已完成AEM和資料收集標籤整合，將JavaScript程式碼插入AEM網站，而不更新AEM專案程式碼。

## 挑戰 — 在標籤屬性中更新和發佈規則

使用從前一次 [建立標籤屬性](./create-tag-property.md) 要完成簡單的挑戰，請更新現有規則以添加其他控制台語句，並使用 _發佈流程_ 將其部署至AEM網站。

## 後續步驟

[為標籤實作除錯](debug-tags-implementation.md)
