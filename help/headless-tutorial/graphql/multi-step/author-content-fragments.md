---
title: 製作內容片段 — AEM無周邊功能快速入門 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 根據內容片段模型建立和編輯新內容片段。 了解如何建立內容片段的變體。
version: Cloud Service
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 2%

---

# 製作內容片段 {#authoring-content-fragments}

在本章中，您將根據 [新定義的內容片段模型](./content-fragment-models.md). 您也會學習如何建立內容片段的變體。

## 必備條件 {#prerequisites}

此為多部分教學課程，假設要執行 [定義內容片段模型](./content-fragment-models.md) 已完成。

## 目標 {#objectives}

* 根據內容片段模型製作內容片段
* 建立內容片段變數

## 建立資產資料夾

內容片段儲存在AEM Assets的資料夾中。 若要從上一章建立的模型建立內容片段，必須建立資料夾才能儲存這些片段。 資料夾上需要設定，才能從特定模型建立片段。

1. 從AEM開始畫面導覽至 **資產** > **檔案**.

   ![導覽至資產檔案](assets/author-content-fragments/navigate-assets-files.png)

1. 點選 **建立** 在角落里，點一下 **資料夾**. 在產生的對話方塊中輸入：

   * 標題*: **我的專案**
   * 名稱： **my-project**

   ![建立資料夾對話方塊](assets/author-content-fragments/create-folder-dialog.png)

1. 選取 **我的資料夾** 資料夾和點選 **屬性**.

   ![開啟資料夾屬性](assets/author-content-fragments/open-folder-properties.png)

1. 點選 **Cloud Services** 標籤。 在 **雲端設定** 使用路徑查找器來選擇 **我的專案** 設定。 值應為 `/conf/my-project`.

   ![設定雲端設定](assets/author-content-fragments/set-cloud-config-my-project.png)

   設定此屬性可讓您使用在前一章中建立的模型來建立內容片段。

1. 點選 **原則** 標籤。 在 **允許的內容片段模型** 使用路徑查找器來選擇 **人員** 和 **團隊** 先前建立的模型。

   ![允許的內容片段模型](assets/author-content-fragments/allowed-content-fragment-models.png)

   這些策略由任何子資料夾自動繼承，並且可以覆蓋。 請注意，您也可以依標籤允許模型，或從其他專案設定啟用模型。 此機制提供管理內容階層的強大方式。

1. 點選 **儲存並關閉** 保存對資料夾屬性的更改。

1. 在內導覽 **我的專案** 檔案夾。

1. 使用下列值建立另一個資料夾：

   * 標題*: **英文**
   * 名稱： **en**

   最佳實務是設定多語言支援的專案。 請參閱 [下列檔案頁面以取得詳細資訊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## 建立內容片段 {#create-content-fragment}

接下來會根據 **團隊** 和 **人員** 模型。

1. 從「 AEM Start Screen 」點選 **內容片段** 開啟內容片段UI。

   ![內容片段UI](assets/author-content-fragments/cf-fragment-ui.png)

1. 在左側邊欄中展開 **我的專案** 點選 **英文**.
1. 點選 **建立** 提起 **新內容片段** 對話框，然後輸入以下值：

   * 位置: `/content/dam/my-project/en`
   * 內容片段模型： **人員**
   * 標題： **無名氏**
   * 名稱: `john-doe`

   ![新內容片段](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. 點選 **建立**.
1. 重複上述步驟以建立代表 **艾莉森·史密斯**:

   * 位置: `/content/dam/my-project/en`
   * 內容片段模型： **人員**
   * 標題： **艾莉森·史密斯**
   * 名稱: `alison-smith`

   點選 **建立** 來建立新的人員片段。

1. 接下來，重複步驟以建立新 **團隊** 片段表示 **A隊**:

   * 位置: `/content/dam/my-project/en`
   * 內容片段模型： **團隊**
   * 標題： **A隊**
   * 名稱: `team-alpha`

   點選 **建立** 建立新團隊片段。

1. 現在應該有三個內容片段位於下方 **我的專案** > **英文**:

   ![新內容片段](assets/author-content-fragments/new-content-fragments.png)

## 編輯人員內容片段 {#edit-person-content-fragments}

接著，將新建立的片段填入資料。

1. 點選旁邊的核取方塊 **無名氏** 點選 **開啟**.

   ![開啟內容片段](assets/author-content-fragments/open-fragment-for-editing.png)

1. 內容片段編輯器包含以內容片段模型為基礎的表單。 填寫各種欄位，將內容新增至 **無名氏** 片段。 若為個人資料圖片，請將您自己的影像上傳至AEM Assets。

   ![內容片段編輯器](assets/author-content-fragments/content-fragment-editor-jd.png)

1. 點選 **儲存並關閉** 以儲存對John Doe片段的變更。
1. 返回內容片段UI並開啟 **艾莉森·史密斯** 檔案進行編輯。
1. 重複上述步驟以填入 **艾莉森·史密斯** 含有內容的片段。

## 編輯團隊內容片段 {#edit-team-content-fragment}

1. 開啟 **A隊** 使用內容片段UI的內容片段。
1. 填寫 **標題**, **簡稱**，和 **說明**.
1. 選取 **無名氏** 和 **艾莉森·史密斯** 要填入的內容片段 **團隊成員** 欄位：

   ![設定團隊成員](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >您也可以使用 **新內容片段** 按鈕。

1. 點選 **儲存並關閉** 以儲存對Team Alpha片段的變更。

## 發佈內容片段

審核完成後，發佈作者 `Content Fragments`

1. 從「 AEM Start Screen 」點選 **內容片段** 開啟內容片段UI。

1. 在左側邊欄中展開 **我的專案** 點選 **英文**.

1. 點選內容片段旁的核取方塊，然後點選 **發佈**

   ![發佈內容片段](assets/author-content-fragments/publish-content-fragment.png)

## 恭喜！ {#congratulations}

恭喜，您剛撰寫了多個內容片段，並建立了變體。

## 後續步驟 {#next-steps}

在下一章中， [了解GraphQL API](explore-graphql-api.md)，您將使用內建的GrapiQL工具探索AEM GraphQL API。 了解AEM如何根據內容片段模型自動產生GraphQL架構。 您將嘗試使用GraphQL語法來建構基本查詢。

## 相關檔案

* [管理內容片段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [變化 - 編寫片段內容](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)
