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
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# 在AEM中建立標籤Cloud Service設定 {#create-launch-cloud-service}

瞭解如何在Adobe Experience Manager中建立標籤Cloud Service設定。 AEM的標籤Cloud Service設定隨後可套用至現有的Site，在Author和Publish環境中都能看見標籤程式庫載入中。

## 建立標籤雲端服務

使用以下步驟建立標籤雲端服務設定。

1. 從&#x200B;**工具**&#x200B;功能表，選取&#x200B;**Cloud Service**&#x200B;區段，然後按一下&#x200B;**Adobe啟動設定**
1. 選取您網站的設定資料夾或選取&#x200B;**WKND網站** （若使用WKND指南專案），然後按一下&#x200B;**建立**
1. 從「_一般_」索引標籤中，使用「**標題**」欄位來命名您的設定，然後從「_關聯的Adobe IMS設定_」下拉式清單中選取「**Adobe啟動**」。 然後，從&#x200B;_公司_&#x200B;下拉式清單中選取您的公司名稱，並從&#x200B;_屬性_&#x200B;下拉式清單中選取先前建立的屬性。
1. 從&#x200B;_測試_&#x200B;和&#x200B;_生產_&#x200B;索引標籤保留預設設定。 不過，建議您檢閱並變更實際生產設定的設定，尤其是&#x200B;_以非同步方式載入程式庫_&#x200B;切換（根據您的效能和最佳化需求）。 另請注意，_資料庫URI_&#x200B;值對於中繼和生產環境是不同的。
1. 最後，按一下&#x200B;**建立**&#x200B;以完成標籤雲端服務。

   ![標籤Cloud Service設定](assets/launch-cloud-services-config.png)

## 套用標籤雲端服務至網站

若要將Tag屬性及其程式庫載入AEM網站，標籤雲端服務設定會套用至網站。 在上一步中，雲端服務設定是在網站名稱資料夾（WKND網站）下建立，因此應該會自動套用，接下來讓我們驗證一下。

1. 從&#x200B;**導覽**&#x200B;功能表，選取&#x200B;**網站**&#x200B;圖示。

1. 選取AEM網站的根頁面，然後按一下&#x200B;**屬性**。 接著，導覽至「**進階**」標籤，並在「**設定**」區段下，確認Cloud Configuration值指向您的網站特定`conf`資料夾。

   ![套用Cloud Service組態至網站](assets/apply-cloud-services-config-to-site.png)

## 驗證在作者和Publish頁面上載入標籤屬性

現在該確認Tag屬性及其程式庫已載入至AEM網站頁面。

1. 以&#x200B;**以發佈檢視**&#x200B;模式開啟您最愛的網站頁面，在瀏覽器主控台中，您應該會看到記錄訊息。 這是觸發&#x200B;_Library Loaded (Page Top)_&#x200B;事件時觸發的相同Tag屬性規則JavaScript程式碼片段訊息。

1. 若要在Publish上驗證，請先發佈您的&#x200B;**標籤雲端服務**&#x200B;設定，並在Publish執行個體上開啟網站頁面。

   作者和Publish頁面上的![標籤屬性](assets/tag-property-on-author-publish-pages.png)

恭喜！您已完成AEM和資料收集標籤整合，將JavaScript程式碼插入您的AEM網站而不更新AEM專案程式碼。

## 挑戰 — 更新並發佈標籤屬性中的規則

使用先前[建立標籤屬性](./create-tag-property.md)的課程來完成簡單的挑戰，更新現有的規則以新增其他主控台陳述式，並使用&#x200B;_發佈流程_&#x200B;將其部署到AEM網站。

## 後續步驟

[對標籤實作除錯](debug-tags-implementation.md)
