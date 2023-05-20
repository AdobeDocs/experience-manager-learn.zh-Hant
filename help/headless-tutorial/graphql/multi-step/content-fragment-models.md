---
title: 定義內容片段模型 — 無頭入門AEM-GraphQL
description: 從Adobe Experience Manager和AEMGraphQL開始。 瞭解如何在中構建內容模型和使用內容片段模型構建架AEM構。 查看現有模型並建立模型。 瞭解可用於定義架構的不同資料類型。
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 2%

---

# 定義內容片段模型 {#content-fragment-models}

在本章中，瞭解如何對內容建模並使用 **內容片段模型**。 您可以瞭解可用於將方案定義為模型一部分的不同資料類型。

我們創造了兩個簡單的模型， **團隊** 和 **人員**。 的 **團隊** 資料模型具有名稱、短名稱和說明，並引用 **人員** 資料模型，包含全名、生物詳細資訊、個人資料圖片和職業清單。

還歡迎您按照基本步驟建立自己的模型，並調整相應的步驟，如GraphQL查詢和React App代碼，或只是按照這些章節中概述的步驟進行操作。

## 必備條件 {#prerequisites}

這是一個多部分教程，假定 [作者AEM環境可用](./overview.md#prerequisites)。

## 目標 {#objectives}

* 建立內容片段模型。
* 確定用於構建模型的可用資料類型和驗證選項。
* 瞭解內容片段模型如何定義 **兩者** 內容片段的資料架構和創作模板。

## 建立項目配置

項目配置包含與特定項目關聯的所有內容片段模型，並提供組織模型的方法。 必須至少建立一個項目 **先** 建立內容片段模型。

1. 登錄AEM到 **作者** 環境(例如 `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **配置瀏覽器**。

   ![導航到配置瀏覽器](assets/content-fragment-models/navigate-config-browser.png)
1. 按一下 **建立**，位於右上角
1. 在生成的對話框中，輸入：

   * 標題*: **我的項目**
   * 名稱*: **我的項目** (喜歡使用所有小寫，使用連字元分隔單詞。 此字串影響客戶端應用程式對其執行請求的唯一GraphQL終結點。)
   * 檢查 **內容片段模型**
   * 檢查 **GraphQL持久查詢**

   ![我的項目配置](assets/content-fragment-models/my-project-configuration.png)

## 建立內容片段模型

接下來，為 **團隊** 和 **人員**。

### 建立人員模型

為 **人員**，即表示屬於團隊的人員的資料模型。

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **內容片段模型**。

   ![導航到內容片段模型](assets/content-fragment-models/navigate-cf-models.png)

1. 導航到 **我的項目** 的子菜單。
1. 點擊 **建立** 右上角的 **建立模型** 的子菜單。
1. 在 **模型標題** 欄位，輸入 **人員** 點擊 **建立**。 在生成的對話框中，點擊 **開啟**，以構建模型。

1. 拖放 **單行文本** 的上界。 在 **屬性** 頁籤：

   * **欄位標籤**: **全名**
   * **屬性名稱**: `fullName`
   * 檢查 **必需**

   ![「全名」屬性欄位](assets/content-fragment-models/full-name-property-field.png)

   的 **屬性名稱** 定義永續到的屬性的名AEM稱。 的 **屬性名稱** 也定義 **鍵** 作為資料架構的一部分的此屬性的名稱。 此 **鍵** 在通過GraphQLAPI公開內容片段資料時使用。

1. 點擊 **資料類型** 頁籤並拖放 **多行文本** 的下方 **全名** 的子菜單。 輸入以下屬性：

   * **欄位標籤**: **傳記**
   * **屬性名稱**: `biographyText`
   * **預設類型**: **富文本**

1. 按一下 **資料類型** 頁籤並拖放 **內容引用** 的子菜單。 輸入以下屬性：

   * **欄位標籤**: **配置檔案圖片**
   * **屬性名稱**: `profilePicture`
   * **根路徑**: `/content/dam`

   配置 **根路徑**，可按一下 **資料夾** 表徵圖，以開啟模式以選擇路徑。 這將限製作者可用於填充路徑的資料夾。 `/content/dam` 是儲存所有AEM Assets（影像、視頻、其他內容片段）的根。

1. 將驗證添加到 **圖片引用** 只有內容類型 **影像** 可用於填充欄位。

   ![限制為影像](assets/content-fragment-models/picture-reference-content-types.png)

1. 按一下 **資料類型** 頁籤並拖放 **枚舉**  資料類型 **圖片引用** 的子菜單。 輸入以下屬性：

   * **呈現為**: **複選框**
   * **欄位標籤**: **職業**
   * **屬性名稱**: `occupation`

1. 添加幾個 **選項** 使用 **添加選項** 按鈕 使用相同值 **選項標籤** 和 **選項值**:

   **藝術家**。 **影響者**。 **攝影師**。 **旅行者**。 **作者**。 **尤圖普**

1. 決賽 **人員** 模型應如下所示：

   ![最終人員模型](assets/content-fragment-models/final-author-model.png)

1. 按一下 **保存** 的子菜單。

### 建立團隊模型

為 **團隊**，這是一組人員的資料模型。 「團隊」模型參照「人員」模型來表示團隊的成員。

1. 在 **我的項目** 資料夾，點擊 **建立** 右上角的 **建立模型** 的子菜單。
1. 在 **模型標題** 欄位，輸入 **團隊** 點擊 **建立**。

   點擊 **開啟** 在生成的對話框中，開啟新建立的模型。

1. 拖放 **單行文本** 的上界。 在 **屬性** 頁籤：

   * **欄位標籤**: **標題**
   * **屬性名稱**: `title`
   * 檢查 **必需**

1. 點擊 **資料類型** 頁籤，然後拖放 **單行文本** 的上界。 在 **屬性** 頁籤：

   * **欄位標籤**: **短名稱**
   * **屬性名稱**: `shortName`
   * 檢查 **必需**
   * 檢查 **唯一**
   * 在， **驗證類型** >選擇 **自定義**
   * 在， **自定義驗證規則運算式** >輸入 `^[a-z0-9\-_]{5,40}$`  — 這確保只能輸入小寫字母數字值和5到40個字元的短划線。

   的 `shortName` 屬性提供了基於縮短路徑查詢單個團隊的方法。 的 **唯一** 設定可確保該值在此模型的每個內容片段中始終唯一。

1. 點擊 **資料類型** 頁籤並拖放 **多行文本** 的下方 **短名稱** 的子菜單。 輸入以下屬性：

   * **欄位標籤**: **說明**
   * **屬性名稱**: `description`
   * **預設類型**: **富文本**

1. 按一下 **資料類型** 頁籤並拖放 **片段引用** 的子菜單。 輸入以下屬性：

   * **呈現為**: **多欄位**
   * **欄位標籤**: **團隊成員**
   * **屬性名稱**: `teamMembers`
   * **允許的內容片段模型**:使用資料夾表徵圖選擇 **人員** 模型。

1. 決賽 **團隊** 模型應如下所示：

   ![最終團隊模型](assets/content-fragment-models/final-team-model.png)

1. 按一下 **保存** 的子菜單。

1. 現在，您應該有兩種模式可以使用：

   ![兩種型號](assets/content-fragment-models/two-new-models.png)

## 發佈項目配置和內容片段模型

經審閱和驗證後，發佈 `Project Configuration` &amp; `Content Fragment Model`

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **配置瀏覽器**。

1. 按一下旁邊的複選框 **我的項目** 點擊 **發佈**

   ![發佈項目配置](assets/content-fragment-models/publish-project-config.png)

1. 從「開始AEM」螢幕導航到 **工具** > **常規** > **內容片段模型**。

1. 導航到 **我的項目** 的子菜單。

1. 點擊 **人員** 和 **團隊** 模型和點擊 **發佈**

   ![發佈內容片段模型](assets/content-fragment-models/publish-content-fragment-model.png)

## 恭喜！ {#congratulations}

恭喜，您剛剛建立了第一個內容片段模型！

## 後續步驟 {#next-steps}

在下一章， [創作內容片段模型](author-content-fragments.md)，可以基於內容片段模型建立和編輯新的內容片段。 您還將學習如何建立內容片段的變體。

## 相關文檔

* [內容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

