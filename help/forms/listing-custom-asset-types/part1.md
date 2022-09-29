---
title: 註冊自訂資產類型
seo-title: Registering Custom Asset Types
description: 啟用自訂資產類型，以便在AEMForms入口網站中列出
seo-description: Enabling custom asset types for listing in AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 1%

---

# 註冊自訂資產類型 {#registering-custom-asset-types}

啟用自訂資產類型，以便在AEMForms入口網站中列出

>[!NOTE]
>
>請確定您已安裝AEM 6.3(SP1)和對應的AEM Forms Add On。 此功能僅適用於AEM Forms 6.3 SP1及更新版本

## 指定基本路徑 {#specify-base-path}

基本路徑是頂層存放庫路徑，包含使用者可能想在搜尋和索引標籤元件中列出的所有資產。 如果需要，用戶也可以從元件編輯對話框配置基本路徑內的特定位置，以便在特定位置上觸發搜索，而不是搜索基本路徑內的所有節點。 依預設，除非使用者從此位置內設定一組特定路徑，否則基本路徑會作為擷取資產的搜尋路徑標準。 要進行效能搜索，必須擁有此路徑的最佳值。 基本路徑的預設值將保持為 **_/content/dam/formsanddocuments_** 因為所有AEM Forms資產都位於 **_/content/dam/formsanddocuments。_**

配置基本路徑的步驟

1. 登入crx
1. 導覽至 **/libs/fd/fp/extensions/querybuilder/basepath**

1. 按一下工具列中的「覆蓋節點」
1. 確認覆蓋位置為「/apps/」
1. 按一下「確定」
1. 按一下儲存
1. 導覽至建立於的新結構 **/apps/fd/fp/extensions/querybuilder/basepath**

1. 將path屬性的值變更為 **&quot;/content/dam&quot;**
1. 按一下儲存

將路徑屬性指定為 **&quot;/content/dam&quot;** 基本上，您是在將「基本路徑」設為/content/dam。 這可以通過開啟Search和Lister元件來驗證。

![basepath](assets/basepath.png)

## 註冊自訂資產類型 {#register-custom-asset-types}

我們已在搜尋和清單元件中新增一個索引標籤（資產清單）。 此索引標籤會列出您所設定的現成資產類型和其他資產類型。 依預設，會列出下列資產類型

1. 調適型表單
1. 表單範本
1. PDF forms
1. 文檔(靜態PDF)

**註冊自訂資產類型的步驟**

1. 建立覆蓋節點 **/libs/fd/fp/extensions/querybuilder/assettypes**

1. 將覆蓋位置設為「/apps」
1. 導覽至建立於的新結構 `/apps/fd/fp/extensions/querybuilder/assettypes`

1. 在此位置下，為要註冊的類型建立一個「nt:unstructured」節點，將節點命名為 **mp4檔案。 將以下兩個屬性添加到此mp4files節點**

   1. 新增jcr:title屬性以指定資產類型的顯示名稱。 將jcr:title的值設定為「Mp4檔案」。
   1. 新增「type」屬性，並將其值設為「videos」。 這是我們在範本中用來列出類型影片資產的值。 儲存您的變更。

1. 在mp4files下建立「nt:unstructured」類型的節點。 將此節點命名為「searchcriteria」
1. 在搜尋條件下新增一或多個篩選器。 假設，如果用戶想要有搜索篩選器來列出mime類型為&quot;video/mp4&quot;的mp4Files，您可以在此處這樣做
1. 在節點搜索條件下建立「nt:unstructured」類型的節點。 將此節點命名為「filetypes」
1. 將以下2個屬性添加到此「filetypes」節點

   1. 名稱: ./jcr:content/metadata/dc:format
   1. 值：video/mp4

1. 這表示屬性dc:format等於video/mp4的資產被視為「Mp4視訊」資產類型。 您可以對搜尋條件使用「jcr:content/metadata」節點上列出的任何屬性

1. **確保保存您的工作**

執行上述步驟後，新資產類型（Mp4檔案）將開始顯示在Search和Lister元件的「資產類型」下拉式清單中，如下所示

![mp4檔案](assets/mp4files.png)

[如果您在讓此功能運作時遇到問題，可匯入下列套件。](assets/assettypeskt1.zip) 套件已定義兩種自訂資產類型。 Mp4檔案和Word文檔。 建議您查看 **/apps/fd/fp/extensions/querybuilder/assettypes**

[安裝customportal套件](assets/customportalpage.zip). 此套件包含範例入口網頁。 本教學課程第2部分會使用本頁面
