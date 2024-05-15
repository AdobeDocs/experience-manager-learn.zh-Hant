---
title: 在AEM Sites中建立標籤Cloud Service設定
description: 瞭解如何在AEM中建立標籤Cloud Service設定。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="整合" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# 在AEM中建立標籤Cloud Service設定 {#create-launch-cloud-service}

瞭解如何在Adobe Experience Manager中建立標籤Cloud Service設定。 AEM TagsCloud Service設定隨後可套用至現有的Site，在「作者」和「發佈」環境中都能看見系統載入Tags資料庫。

## 建立標籤雲端服務

使用以下步驟建立標籤雲端服務設定。

1. 從 **工具** 功能表，選取 **Cloud Service** 區段並按一下 **AdobeLaunch設定**
1. 選取您網站的設定資料夾或選取 **WKND網站** （若使用WKND指南專案）並按一下 **建立**
1. 從 _一般_ 索引標籤，使用為您的設定命名 **標題** 欄位，然後選取 **Adobe啟動** 從 _相關聯的Adobe IMS設定_ 下拉式清單。 然後，從中選擇您的公司名稱 _公司_ 下拉式清單，選取先前建立的屬性，從 _屬性_ 下拉式清單。
1. 從 _分段_ 和 _生產_ 索引標籤會保留預設設定。 不過，我們建議您檢閱並變更實際生產設定的設定，尤其是 _非同步載入程式庫_ 根據您的效能和最佳化需求進行切換。 另請注意 _資料庫URI_ 測試和生產環境的值不同。
1. 最後，按一下 **建立** 以完成標籤雲端服務。

   ![標籤Cloud Service設定](assets/launch-cloud-services-config.png)

## 套用標籤雲端服務至網站

若要將Tag屬性及其程式庫載入AEM網站，標籤雲端服務設定會套用至網站。 在上一步中，雲端服務設定是在網站名稱資料夾（WKND網站）下建立，因此應該會自動套用，接下來讓我們驗證一下。

1. 從 **導覽** 功能表，選取 **網站** 圖示。

1. 選取AEM網站的根頁面，然後按一下 **屬性**. 然後，導覽至 **進階** 標籤和底下 **設定** 區段，確認Cloud Configuration值指向您的網站特定 `conf` 資料夾。

   ![套用Cloud Service設定至網站](assets/apply-cloud-services-config-to-site.png)

## 驗證在製作和發佈頁面上載入標籤屬性

現在該確認Tag屬性及其程式庫已載入至AEM網站頁面。

1. 在「 」中開啟您最愛的網站頁面 **以發佈的形式檢視** 模式，您應該會在瀏覽器主控台中看到記錄訊息。 這是來自標籤屬性規則的JavaScript程式碼片段的相同訊息，引發時機 _程式庫已載入（頁面頂端）_ 事件已觸發。

1. 若要在發佈上進行驗證，請先發佈您的 **標籤雲端服務** 設定，並在發佈執行個體上開啟網站頁面。

   ![製作和發佈頁面上的標籤屬性](assets/tag-property-on-author-publish-pages.png)

恭喜！您已完成AEM和資料收集標籤整合，將JavaScript程式碼插入您的AEM網站而不更新AEM專案程式碼。

## 挑戰 — 更新並發佈標籤屬性中的規則

使用從前一個事件中吸取的教訓 [建立標籤屬性](./create-tag-property.md) 若要完成簡單的挑戰，請更新現有規則以新增其他主控台陳述式並使用 _發佈流程_ 將其部署至AEM網站。

## 後續步驟

[對標籤實作除錯](debug-tags-implementation.md)
