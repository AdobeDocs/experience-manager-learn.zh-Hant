---
title: 編寫內容片段 — AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 根據內容片段模型建立及編輯新內容片段。 瞭解如何建立內容片段的變體。
version: Cloud Service
mini-toc-levels: 1
jira: KT-6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
duration: 173
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '853'
ht-degree: 3%

---

# 製作內容片段 {#authoring-content-fragments}

在本章中，您將根據[新定義的內容片段模式](./content-fragment-models.md)來建立和編輯新的內容片段。 您也會瞭解如何建立內容片段的變體。

## 先決條件 {#prerequisites}

此教學課程包含多個部分，並假設已完成[定義內容片段模式](./content-fragment-models.md)中概述的步驟。

## 目標 {#objectives}

* 根據內容片段模型製作內容片段
* 建立內容片段變數

## 建立資產資料夾

內容片段儲存在AEM Assets的資料夾中。 若要使用上一章建立的模型建立內容片段，必須建立資料夾以儲存它們。 需要在該資料夾上進行設定，才能從特定模型建立片段。

1. 從AEM開始畫面，瀏覽至&#x200B;**Assets** > **檔案**。

   ![瀏覽至資產檔案](assets/author-content-fragments/navigate-assets-files.png)

1. 點選右上角的&#x200B;**建立**，然後點選&#x200B;**資料夾**。 在產生的對話方塊中，輸入：

   * 標題*： **我的專案**
   * 名稱： **my-project**

   ![建立資料夾對話框](assets/author-content-fragments/create-folder-dialog.png)

1. 選取&#x200B;**我的資料夾**&#x200B;資料夾，然後點選&#x200B;**屬性**。

   ![開啟資料夾屬性](assets/author-content-fragments/open-folder-properties.png)

1. 點選&#x200B;**Cloud Service**&#x200B;標籤。 在[雲端設定]索引標籤下，使用路徑尋找器來選取&#x200B;**我的專案**&#x200B;設定。 值應為`/conf/my-project`。

   ![設定雲端設定](assets/author-content-fragments/set-cloud-config-my-project.png)

   設定此屬性可讓內容片段使用上一章建立的模型來建立。

1. 點選「**原則**」標籤，在「**允許的內容片段模式**」欄位下，使用路徑尋找器來選取先前建立的「**人員**」和「**團隊**」模式。

   ![允許的內容片段模型](assets/author-content-fragments/allowed-content-fragment-models.png)

   任何子資料夾都會自動繼承這些原則，且這些原則可以覆寫。 您也可以依標籤允許模型，或從其他專案設定啟用模型。 此機制提供管理內容階層的強大方式。

1. 點選&#x200B;**儲存並關閉**&#x200B;以儲存資料夾屬性的變更。

1. 瀏覽&#x200B;**我的專案**&#x200B;資料夾。

1. 使用下列值建立另一個資料夾：

   * 標題*： **英文**
   * 名稱： **en**

   最佳實務是設定專案以提供多語言支援。 如需詳細資訊，請參閱[下列檔案頁面](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html)。


## 建立內容片段 {#create-content-fragment}

>[!TIP]
>
>本機AEM SDK使用者：使用AEM Assets UI來建立和編寫內容片段，而非下列內容片段UI。 如需詳細指示，請參閱[AEM檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)。

接下來幾個內容片段是根據&#x200B;**團隊**&#x200B;和&#x200B;**人員**&#x200B;模型建立的。

1. 從AEM開始畫面，點選&#x200B;**內容片段**&#x200B;以開啟內容片段UI。

   ![內容片段UI](assets/author-content-fragments/cf-fragment-ui.png)

1. 在左側邊欄中，展開&#x200B;**我的專案**，然後點選&#x200B;**英文**。
1. 點選「**建立**」以開啟「**新內容片段**」對話方塊並輸入下列值：

   * 位置： `/content/dam/my-project/en`
   * 內容片段模型： **人員**
   * 標題： **John Doe**
   * 名稱：`john-doe`

   ![新內容片段](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. 點選「**建立**」。
1. 重複上述步驟以建立代表&#x200B;**Alison Smith**&#x200B;的片段：

   * 位置： `/content/dam/my-project/en`
   * 內容片段模型： **人員**
   * 標題： **Alison Smith**
   * 名稱：`alison-smith`

   點選&#x200B;**建立**&#x200B;以建立人員片段。

1. 接著，重複步驟以建立代表&#x200B;**團隊Alpha**&#x200B;的&#x200B;**團隊**&#x200B;片段：

   * 位置： `/content/dam/my-project/en`
   * 內容片段模型： **團隊**
   * 標題： **團隊Alpha**
   * 名稱：`team-alpha`

   點選&#x200B;**建立**&#x200B;以建立團隊片段。

1. **我的專案** > **英文**&#x200B;下應該有三個內容片段：

   ![新內容片段](assets/author-content-fragments/new-content-fragments.png)

## 編輯個人內容片段 {#edit-person-content-fragments}

接著，將資料填入新建立的片段中。

1. 點選&#x200B;**John Doe**&#x200B;旁的核取方塊，然後點選&#x200B;**開啟**。

   ![開啟內容片段](assets/author-content-fragments/open-fragment-for-editing.png)

1. 內容片段編輯器包含以內容片段模式為基礎的表單。 填寫各種欄位以新增內容至&#x200B;**John Doe**&#x200B;片段。 若為個人資料圖片，請將自己的影像上傳至AEM Assets。

   ![內容片段編輯器](assets/author-content-fragments/content-fragment-editor-jd.png)

1. 點選&#x200B;**儲存並關閉**&#x200B;以儲存對John Doe片段的變更。
1. 返回內容片段UI並開啟&#x200B;**Alison Smith**&#x200B;檔案進行編輯。
1. 重複上述步驟以使用內容填入&#x200B;**Alison Smith**&#x200B;片段。

## 編輯團隊內容片段 {#edit-team-content-fragment}

1. 使用內容片段UI開啟&#x200B;**團隊Alpha**&#x200B;內容片段。
1. 填寫&#x200B;**標題**、**簡短名稱**&#x200B;和&#x200B;**描述**&#x200B;的欄位。
1. 選取&#x200B;**John Doe**&#x200B;和&#x200B;**Alison Smith**&#x200B;內容片段，以填入&#x200B;**團隊成員**&#x200B;欄位：

   ![設定團隊成員](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >您也可以使用&#x200B;**新增內容片段**&#x200B;按鈕來建立內嵌內容片段。

1. 點選&#x200B;**儲存並關閉**&#x200B;以儲存對團隊Alpha片段的變更。

## Publish內容片段

>[!TIP]
>
>本機AEM SDK使用者：使用AEM Assets UI來發佈內容片段，而非下列內容片段UI。 如需詳細指示，請參閱[AEM檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html#publishing-and-referencing-a-fragment)。

檢閱和驗證後，發佈編寫的`Content Fragments`

1. 從AEM開始畫面，點選&#x200B;**內容片段**&#x200B;以開啟內容片段UI。

1. 在左側邊欄中，展開&#x200B;**我的專案**，然後點選&#x200B;**英文**。

1. 點選內容片段旁的核取方塊，然後點選&#x200B;**Publish**。
   ![Publish內容片段](assets/author-content-fragments/publish-content-fragment.png)

## 恭喜！ {#congratulations}

恭喜，您已編寫多個內容片段並建立變數。

## 後續步驟 {#next-steps}

在下一章[探索GraphQL API](explore-graphql-api.md)中，您將使用內建GrapiQL工具探索AEM的GraphQL API。 瞭解AEM如何根據內容片段模型自動產生GraphQL結構描述。 您將使用GraphQL語法嘗試建構基本查詢。

## 相關檔案

* [管理內容片段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [變化 - 編寫片段內容](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)
