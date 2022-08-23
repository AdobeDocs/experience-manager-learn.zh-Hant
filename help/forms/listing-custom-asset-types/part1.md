---
title: 註冊自定義資產類型
seo-title: Registering Custom Asset Types
description: 啟用AEMForms門戶中列出的自定義資產類型
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
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 1%

---

# 註冊自定義資產類型 {#registering-custom-asset-types}

啟用AEMForms門戶中列出的自定義資產類型

>[!NOTE]
>
>確保安裝了AEMSP1的6.3和相應的AEM Forms載入程式。 此功能僅適用於AEM Forms6.3 SP1及更高版本

## 指定基本路徑 {#specify-base-path}

基本路徑是包含用戶可能希望在search &amp; lister元件中列出的所有資產的頂級儲存庫路徑。 如果需要，用戶還可以配置來自元件編輯對話框的基本路徑內的特定位置，從而在特定位置上觸發搜索，而不是搜索基本路徑內的所有節點。 預設情況下，基本路徑用作獲取資產的搜索路徑標準，除非用戶從此位置配置一組特定路徑。 為了進行效能搜索，必須使此路徑具有最佳值。 基本路徑的預設值將保持為 **_/content/dam/formsanddocuments_** 因為AEM Forms的所有資產 **_/content/dam/formsanddocuments。_**

配置基本路徑的步驟

1. 登錄到crx
1. 導航到 **/libs/fd/fp/extensions/querybuilder/basepath**

1. 按一下工具欄中的「覆蓋節點」
1. 確保覆蓋位置為「/apps/」
1. 按一下確定
1. 按一下「保存」
1. 導航至在以下位置建立的新結構 **/apps/fd/fp/extensions/querybuilder/basepath**

1. 將path屬性的值更改為 **&quot;/content/dam&quot;**
1. 按一下「保存」

通過指定路徑屬性 **&quot;/content/dam&quot;** 您基本上是在將基本路徑設定為/content/dam。 這可以通過開啟Search和Lister元件來驗證。

![基本路徑](assets/basepath.png)

## 註冊自定義資產類型 {#register-custom-asset-types}

我們在搜索和線索元件中添加了一個新頁籤（資產清單）。 此頁籤將列出您配置的現有資產類型和附加資產類型。 預設情況下，將列出以下資產類型

1. 調適型表單
1. 表單模板
1. PDF forms
1. 文檔(靜態PDF)

**註冊自定義資產類型的步驟**

1. 建立的覆蓋節點 **/libs/fd/fp/extensions/querybuilder/assettypes**

1. 將覆蓋位置設定為「/apps」
1. 導航到在**/apps/fd/fp/extensions/querybuilder/assettypes中建立的新結構**

1. 在此位置下，為要註冊的類型建立一個「nt:antrograbled」節點，將節點命名為 **mp4files。 將以下兩個屬性添加到此mp4files節點**

   1. 添加jcr:title屬性以指定資產類型的顯示名稱。 將jcr:title的值設定為「Mp4檔案」。
   1. 添加&quot;type&quot;屬性並將其值設定為&quot;videos&quot;。 這是我們在模板中用於列出類型視頻資產的值。 保存更改。

1. 在mp4files下建立「nt:antrograbled」類型的節點。 將此節點命名為&quot;searchcriteria&quot;
1. 根據搜索條件添加一個或多個篩選器。 假設用戶希望有一個搜索過濾器來列出mime類型為&quot;video/mp4&quot;的mp4Files，您可以在此處這樣做
1. 在節點搜索條件下建立「nt:antrogized」類型的節點。 將此節點命名為「檔案類型」
1. 將以下2個屬性添加到此「檔案類型」節點

   1. 名稱: ./jcr:content/metadata/dc:format
   1. 值：視頻/mp4

1. 這意味著，具有dc:format等於video/mp4的屬性的資產將被視為資產類型「Mp4視頻」。 您可以使用「jcr:content/metadata」節點上列出的任何屬性作為搜索條件

1. **確保保存您的工作**

執行上述步驟後，新資產類型（Mp4檔案）將開始顯示在Search and Lister元件的資產類型下拉清單中，如下所示

![mp4檔案](assets/mp4files.png)

[如果在使此程式工作時遇到問題，可以導入以下程式包。](assets/assettypeskt1.zip) 包定義了兩種自定義資產類型。 Mp4檔案和Word文檔。 建議你看看 **/apps/fd/fp/extensions/querybuilder/assettypes**

[安裝客戶門戶包](assets/customportalpage.zip)。 此包包含示例門戶頁。 本教程第2部分將使用此頁
