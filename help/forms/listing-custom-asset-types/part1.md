---
title: 註冊自訂資產類型
seo-title: 註冊自訂資產類型
description: 啟用自訂資產類型以在AEMForms Portal中列出
seo-description: 啟用自訂資產類型以在AEMForms Portal中列出
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: 適用性表單
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: 開發
role: 開發人員
level: 經驗豐富
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 2%

---


# 註冊自訂資產類型{#registering-custom-asset-types}

啟用自訂資產類型以在AEMForms Portal中列出

>[!NOTE]
>
>請確定您已AEM安裝了6.3 SP1和相應的AEM Forms附加模組。 此功能僅適用於AEM Forms6.3 SP1和更高版本

## 指定基本路徑{#specify-base-path}

基本路徑是頂層儲存庫路徑，該路徑包含用戶可能希望在search &amp; lister元件中列出的所有資產。 如果需要，用戶還可以從元件編輯對話框配置基本路徑內的特定位置，以便在特定位置上觸發搜索，而不是搜索基本路徑內的所有節點。 依預設，基本路徑會用作擷取資產的搜尋路徑標準，除非使用者在此位置設定一組特定路徑。 為了進行效能搜索，必須擁有此路徑的最佳值。 基本路徑的預設值將維持為&#x200B;**_/content/dam/formsanddocuments_**，因為所有AEM Forms資產都位於&#x200B;**_/content/dam/formsanddocuments。_**

配置基本路徑的步驟

1. 登入crx
1. 導覽至&#x200B;**/libs/fd/fp/extensions/querybuilder/basepath**

1. 按一下工具列中的「覆蓋節點」
1. 請確定覆蓋位置為「/apps/」
1. 按一下「確定」
1. 按一下「儲存」
1. 導覽至在&#x200B;**/apps/fd/fp/extensions/querybuilder/basepath**&#x200B;建立的新結構

1. 將path屬性的值更改為&#x200B;**&quot;/content/dam&quot;**
1. 按一下「儲存」

指定&#x200B;**&quot;/content/dam&quot;**&#x200B;的路徑屬性，基本上就是將「基本路徑」設為/content/dam。 這可以通過開啟Search和Lister元件來驗證。

![basepath](assets/basepath.png)

## 註冊自訂資產類型{#register-custom-asset-types}

我們已在搜尋和清單元件中新增標籤（資產清單）。 此標籤將列出您所設定的現成可用資產類型和其他資產類型。 依預設，會列出下列資產類型

1. 適用性表單
1. 表單範本
1. PDF forms
1. 檔案（靜態PDF）

**註冊自訂資產類型的步驟**

1. 建立&#x200B;**/libs/fd/fp/extensions/querybuilder/assettypes**&#x200B;的覆蓋節點

1. 將覆蓋位置設為「/apps」
1. 導覽至在**/apps/fd/fp/extensions/querybuilder/assettypes中建立的新結構**

1. 在此位置下，為要註冊的類型建立一個「nt:unstructured」節點，為節點命名&#x200B;**mp4檔案。 將以下兩個屬性添加到此mp4files節點**

   1. 新增jcr:title屬性，以指定資產類型的顯示名稱。 將jcr:title的值設為&quot;Mp4 Files&quot;。
   1. 新增「type」屬性，並將其值設為「videos」。 這是我們在範本中用來列出類型影片資產的值。 儲存您的變更。

1. 在mp4files下建立類型為&quot;nt:unstructured&quot;的節點。 將此節點命名為&quot;searchcriteria&quot;
1. 在搜尋准則下新增一或多個篩選。 假設，如果使用者想要有搜尋篩選器來列出mime類型為&quot;video/mp4&quot;的mp4Files，您可以在這裡這麼做
1. 在節點搜索條件下建立類型為&quot;nt:unstructured&quot;的節點。 將此節點命名為&quot;filetypes&quot;
1. 將以下2個屬性添加到此&quot;filetypes&quot;節點

   1. 名稱: ./jcr:content/metadata/dc:format
   1. 值：video/mp4

1. 這表示具有dc:format等於video/mp4屬性的資產將被視為資產類型「Mp4視訊」。 您可以使用&quot;jcr:content/metadata&quot;節點上列出的任何屬性做為搜尋准則

1. **請務必儲存您的作品**

執行上述步驟後，新資產類型（Mp4檔案）將開始顯示在Search and Lister元件的資產類型下拉式清單中，如下所示

![mp4檔案](assets/mp4files.png)

[如果您在使此功能運作時遇到問題，可匯入下列套件。](assets/assettypeskt1.zip) 套件已定義兩種自訂資產類型。Mp4檔案和Word檔案。 建議您查看&#x200B;**/apps/fd/fp/extensions/querybuilder/assettypes**

[安裝customportal套件](assets/customportalpage.zip)。此套件包含範例入口網頁。 本頁將用於本教學課程的第2部份

