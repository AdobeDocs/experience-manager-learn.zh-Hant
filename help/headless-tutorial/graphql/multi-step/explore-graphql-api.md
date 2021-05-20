---
title: 深入了解GraphQL API — 開始使用AEM無周邊功能 — GraphQL
description: 開始使用Adobe Experience Manager(AEM)和GraphQL。 使用內建的GrapiQL IDE，探索AEM GraphQL API。 了解AEM如何根據內容片段模型自動產生GraphQL架構。 使用GraphQL語法實驗建構基本查詢。
sub-product: 資產
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: 內容片段、GraphQL API
topic: 無頭式、內容管理
role: Developer
level: Beginner
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 1%

---


# 探索GraphQL API {#explore-graphql-apis}

AEM的GraphQL API提供強大的查詢語言，可將內容片段的資料公開給下游應用程式。 內容片段模型會定義內容片段所使用的資料結構。 每當建立或更新內容片段模型時，架構就會轉譯並新增至組成GraphQL API的「圖表」中。

在本章中，我們將探索一些常見的GraphQL查詢，以使用名為[GraphiQL](https://github.com/graphql/graphiql)的IDE收集內容。 GraphiQL IDE允許您快速測試和調整返回的查詢和資料。 GraphiQL還可輕鬆存取說明檔案，讓您輕鬆了解和了解可用的方法。

## 必備條件 {#prerequisites}

這是多部分教學課程，假設已完成[製作內容片段](./author-content-fragments.md)中概述的步驟。

## 目標 {#objectives}

* 了解如何使用GraphiQL工具，使用GraphQL語法來建構查詢。
* 了解如何查詢內容片段和單一內容片段清單。
* 了解如何篩選及要求特定資料屬性。
* 了解如何查詢內容片段的變異。
* 了解如何加入多個內容片段模型的查詢

## 安裝GraphiQL工具{#install-graphiql}

GraphiQL IDE是開發工具，僅在開發或本地實例等較低級別環境中需要。 因此，AEM專案中不包含此套件，而是可臨機安裝的個別套件。

1. 導覽至&#x200B;**[Software Distribution Portal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as aCloud Service**。
1. 搜索「GraphiQL」(請務必在&#x200B;**GraphiQL**&#x200B;中包含&#x200B;**i**。
1. 下載最新&#x200B;**GraphiQL內容包v.x.x.x**

   ![下載GraphiQL包](assets/explore-graphql-api/software-distribution.png)

   zip檔案是可直接安裝的AEM套件。

1. 從&#x200B;**AEM Start**&#x200B;菜單導航至&#x200B;**Tools** > **Deployment** > **Packages**。
1. 按一下「**上傳套件**」 ，然後選擇上一步驟中下載的套件。 按一下&#x200B;**Install**&#x200B;以安裝軟體包。

   ![安裝GraphiQL包](assets/explore-graphql-api/install-graphiql-package.png)

## 查詢內容片段清單{#query-list-cf}

常見的需求是查詢多個內容片段。

1. 導航到GraphiQL IDE([http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html))。
1. 將下列查詢貼到左側面板（在備注清單下）:

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. 按頂部菜單中的&#x200B;**播放**&#x200B;按鈕以執行查詢。 您應該會看到上一章中「貢獻者」內容片段的結果：

   ![貢獻者清單結果](assets/explore-graphql-api/contributorlist-results.png)

1. 將游標置於`_path`文本下，然後輸入&#x200B;**CTRL+空格**&#x200B;以觸發代碼提示。 將`fullName`和`occupation`新增至查詢。

   ![使用程式碼編輯更新查詢](assets/explore-graphql-api/update-query-codehinting.png)

1. 按&#x200B;**Play**&#x200B;按鈕再次執行查詢，您應該會看到結果包含`fullName`和`occupation`的其他屬性。

   ![全名和職業結果](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` 和屬 `occupation` 性簡單。從[定義內容片段模型](./content-fragment-models.md)章節回想，`fullName`和`occupation`是定義各個欄位的&#x200B;**屬性名稱**&#x200B;時使用的值。

1. `pictureReference` 和代 `biographyText` 表更複雜的欄位。使用下列內容更新查詢，以傳回`pictureReference`和`biographyText`欄位的相關資料。

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biographyText {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biographyText` 是多行文字欄位，GraphQL API可讓我們為結果選擇各種格式， `html`例如 `markdown`、 `json` 或 `plaintext`。

   `pictureReference` 是內容參考，且應為影像，因此會使用內 `ImageRef` 建物件。這可讓我們要求有關要參考之影像的其他資料，例如`width`和`height`。

1. 接下來，嘗試查詢&#x200B;**Adventures**&#x200B;清單。 執行下列查詢：

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   您應該會看到傳回的&#x200B;**Adventures**&#x200B;清單。 您可以在查詢中新增其他欄位，以進行實驗。

## 篩選內容片段清單{#filter-list-cf}

接下來，我們將探討如何根據屬性值將結果篩選為內容片段的子集。

1. 在GraphiQL UI中輸入以下查詢：

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   上述查詢會對系統中的所有貢獻者執行搜尋。 新增的篩選器至查詢的開頭，將對`occupation`欄位和字串&quot;**Photographer**&quot;執行比較。

1. 執行查詢時，應該只傳回單一&#x200B;**貢獻者**。
1. 輸入以下查詢以查詢&#x200B;**Adventures**&#x200B;清單，其中`adventureActivity`為&#x200B;**not**&#x200B;等於&#x200B;**&quot;Surfing&quot;**:

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. 執行查詢並檢查結果。 請注意，所有結果中均未包含等於&#x200B;**&quot;Surfing&quot;**&#x200B;的`adventureType`。

篩選和建立複雜查詢有許多其他選項，以上只是幾個範例。

## 查詢單一內容片段{#query-single-cf}

您也可以直接查詢單一內容片段。 AEM中的內容以分層方式儲存，而片段的唯一識別碼基於片段的路徑。 如果目標是傳回關於單一片段的資料，建議您使用路徑並直接查詢模型。 使用此語法表示查詢複雜度會非常低，且會產生更快的結果。

1. 在GraphiQL編輯器中輸入以下查詢：

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

1. 執行查詢並觀察&#x200B;**Stacey Roswells**&#x200B;片段的單一結果是否已傳回。

   在上一個練習中，您使用篩選器來縮小結果清單。 您可以使用類似的語法來依路徑篩選，但基於效能原因，建議使用上述語法。

1. 回想一下[製作內容片段](./author-content-fragments.md)章節中，為&#x200B;**Stacey Roswells**&#x200B;建立了&#x200B;**Summary**&#x200B;變異。 更新查詢以傳回&#x200B;**Summary**&#x200B;變數：

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

   即使變體名為&#x200B;**Summary**，變體仍以小寫形式保存，因此會使用`summary`。

1. 執行查詢並觀察`biography`欄位包含的`html`結果要短得多。

## 查詢多個內容片段模型{#query-multiple-models}

您也可以將個別查詢合併為單一查詢。 這對於將為應用程式提供電源所需的HTTP請求數減到最少非常有用。 例如，應用程式的&#x200B;*首頁*&#x200B;檢視可根據&#x200B;**兩個**&#x200B;不同的內容片段模型來顯示內容。 我們不必執行&#x200B;**兩個**&#x200B;個別的查詢，而是可以將查詢合併為單一請求。

1. 在GraphiQL編輯器中輸入以下查詢：

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. 執行查詢並觀察結果集包含&#x200B;**Adventures**&#x200B;和&#x200B;**Contributors**&#x200B;的資料：

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## 其他資源

如需更多GraphQL查詢的範例，請參閱：[學習如何搭配AEM使用GraphQL — 範例內容與查詢](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html)。

## 恭喜！ {#congratulations}

恭喜，您剛剛建立並執行了多個GraphQL查詢！

## 後續步驟{#next-steps}

在下一章[從React應用程式查詢AEM](./graphql-and-external-app.md)中，您將探索外部應用程式如何查詢AEM GraphQL端點。 修改範例WKND GraphQL React應用程式以新增篩選GraphQL查詢的外部應用程式，可讓應用程式的使用者依活動篩選歷險。 您也將了解一些基本的錯誤處理。
