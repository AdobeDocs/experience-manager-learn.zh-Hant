---
title: 定義內容片段模型 — AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 瞭解如何在AEM中建立內容模型，並使用內容片段模型建置結構。 檢閱現有模型並建立模型。 瞭解定義結構時可用的不同資料型別。
version: Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
duration: 228
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 1%

---

# 定義內容片段模型 {#content-fragment-models}

在本章中，瞭解如何使用建立內容模型並建置結構 **內容片段模型**. 您會瞭解可用來將結構描述定義為模型一部分的不同資料型別。

我們建立了兩個簡單的模型， **團隊** 和 **個人**. 此 **團隊** 資料模型有名稱、簡短名稱和說明，並參照 **個人** 資料模型，有全名、個人資料細節、個人資料圖片和職業清單。

您也可以按照基本步驟建立自己的模型，並調整相關步驟，例如GraphQL查詢和React應用程式程式碼，或只是按照這些章節中概述的步驟進行。

## 先決條件 {#prerequisites}

此教學課程包含多個部分，且假設您可使用 [AEM作者環境可供使用](./overview.md#prerequisites).

## 目標 {#objectives}

* 建立內容片段模型。
* 識別建立模型的可用資料型別和驗證選項。
* 瞭解內容片段模式如何定義 **兩者** 內容片段的資料結構和製作範本。

## 建立專案設定

專案設定包含與特定專案關聯的所有內容片段模式，並提供組織模式的方法。 至少必須建立一個專案 **早於** 正在建立內容片段模型。

1. 登入AEM **作者** 環境(例如 `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. 從AEM開始畫面，導覽至 **工具** > **一般** > **設定瀏覽器**.

   ![導覽至設定瀏覽器](assets/content-fragment-models/navigate-config-browser.png)
1. 按一下 **建立**，位於右上角
1. 在產生的對話方塊中，輸入：

   * 標題*： **我的專案**
   * 名稱*： **my-project** (偏好使用所有小寫字母並使用連字型大小來分隔單字。 此字串會影響使用者端應用程式執行要求的唯一GraphQL端點。)
   * 檢查 **內容片段模型**
   * 檢查 **GraphQL持續查詢**

   ![我的專案設定](assets/content-fragment-models/my-project-configuration.png)

## 建立內容片段模型

接下來，為建立兩個模型 **團隊** 和 **個人**.

### 建立人員模型

建立模型 **個人**，此資料模型代表屬於團隊的個人。

1. 從AEM開始畫面，導覽至 **工具** > **一般** > **內容片段模型**.

   ![導覽至內容片段模型](assets/content-fragment-models/navigate-cf-models.png)

1. 導覽至 **我的專案** 資料夾。
1. 點選 **建立** 在右上角顯示 **建立模型** 精靈。
1. 在 **模型標題** 欄位，輸入 **個人** 然後點選 **建立**. 在產生的對話方塊中，點選 **開啟**，以建立模式。

1. 拖放 **單行文字** 元素移至主面板。 在 **屬性** 標籤：

   * **欄位標籤**： **全名**
   * **屬性名稱**： `fullName`
   * 檢查 **必填**

   ![完整名稱屬性欄位](assets/content-fragment-models/full-name-property-field.png)

   此 **屬性名稱** 會定義儲存至AEM的屬性名稱。 此 **屬性名稱** 也會定義 **key** 做為資料結構一部分之這個屬性的名稱。 這個 **key** 透過GraphQL API公開內容片段資料時使用。

1. 點選 **資料型別** 標籤並拖放 **多行文字** 欄位於 **全名** 欄位。 輸入下列屬性：

   * **欄位標籤**： **傳記**
   * **屬性名稱**： `biographyText`
   * **預設型別**： **RTF文字**

1. 按一下 **資料型別** 標籤並拖放 **內容參考** 欄位。 輸入下列屬性：

   * **欄位標籤**： **個人資料圖片**
   * **屬性名稱**： `profilePicture`
   * **根路徑**： `/content/dam`

   設定時 **根路徑**，您可以按一下 **資料夾** 圖示可顯示強制回應視窗以選取路徑。 這會限製作者用來填入路徑的資料夾。 `/content/dam` 是所有AEM Assets （影像、影片和其他內容片段）儲存的根目錄。

1. 新增驗證至 **圖片引用** 因此只有內容型別 **影像** 可用來填入欄位。

   ![限製為影像](assets/content-fragment-models/picture-reference-content-types.png)

1. 按一下 **資料型別** 標籤並拖放 **分項清單**  下的資料型別 **圖片引用** 欄位。 輸入下列屬性：

   * **呈現為**： **核取方塊**
   * **欄位標籤**： **職業**
   * **屬性名稱**： `occupation`

1. 新增多個 **選項** 使用 **新增選項** 按鈕。 使用相同的值 **選項標籤** 和 **選項值**：

   **藝人**， **影響者**， **攝影師**， **Traveler**， **作者**， **YouTuber**

1. 最終的 **個人** 模型應如下所示：

   ![最終人員模型](assets/content-fragment-models/final-author-model.png)

1. 按一下 **儲存** 以儲存變更。

### 建立團隊模型

建立模型 **團隊**，這是一組人員的資料模型。 「專案團隊」模型會參考「人員」模型來代表專案團隊成員。

1. 在 **我的專案** 資料夾，點選 **建立** 在右上角顯示 **建立模型** 精靈。
1. 在 **模型標題** 欄位，輸入 **團隊** 然後點選 **建立**.

   點選 **開啟** 在產生的對話方塊中，開啟新建立的模型。

1. 拖放 **單行文字** 元素移至主面板。 在 **屬性** 標籤：

   * **欄位標籤**： **標題**
   * **屬性名稱**： `title`
   * 檢查 **必填**

1. 點選 **資料型別** 標籤並拖放 **單行文字** 元素移至主面板。 在 **屬性** 標籤：

   * **欄位標籤**： **簡短名稱**
   * **屬性名稱**： `shortName`
   * 檢查 **必填**
   * 檢查 **獨特**
   * 下， **驗證型別** >選擇 **自訂**
   * 下， **自訂驗證Regex** > enter `^[a-z0-9\-_]{5,40}$`  — 這可確保只能輸入5到40個字元的小寫字母數字值和破折號。

   此 `shortName` 屬性提供依據縮短路徑查詢個別團隊的方法。 此 **獨特** 設定可確保此值在此模式的每個內容片段中一律是唯一的。

1. 點選 **資料型別** 標籤並拖放 **多行文字** 欄位於 **簡短名稱** 欄位。 輸入下列屬性：

   * **欄位標籤**： **說明**
   * **屬性名稱**： `description`
   * **預設型別**： **RTF文字**

1. 按一下 **資料型別** 標籤並拖放 **片段引用** 欄位。 輸入下列屬性：

   * **呈現為**： **多個欄位**
   * **欄位標籤**： **團隊成員**
   * **屬性名稱**： `teamMembers`
   * **允許的內容片段模型**：使用資料夾圖示以選取 **個人** 模型。

1. 最終的 **團隊** 模型應如下所示：

   ![最終團隊模型](assets/content-fragment-models/final-team-model.png)

1. 按一下 **儲存** 以儲存變更。

1. 您現在應該可以從以下兩種模式運作：

   ![兩個模型](assets/content-fragment-models/two-new-models.png)

## 發佈專案設定和內容片段模型

檢閱和驗證後，發佈 `Project Configuration` &amp; `Content Fragment Model`

1. 從AEM開始畫面，導覽至 **工具** > **一般** > **設定瀏覽器**.

1. 點選「 」旁的核取方塊 **我的專案** 然後點選 **發佈**

   ![發佈專案設定](assets/content-fragment-models/publish-project-config.png)

1. 從AEM開始畫面，導覽至 **工具** > **一般** > **內容片段模型**.

1. 導覽至 **我的專案** 資料夾。

1. 點選 **個人** 和 **團隊** 模型和點選 **發佈**

   ![發佈內容片段模型](assets/content-fragment-models/publish-content-fragment-model.png)

## 恭喜！ {#congratulations}

恭喜，您剛才已建立第一個內容片段模型！

## 後續步驟 {#next-steps}

在下一章中， [製作內容片段模型](author-content-fragments.md)，您會根據內容片段模式建立及編輯新的內容片段。 您也會瞭解如何建立內容片段的變體。

## 相關檔案

* [內容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

