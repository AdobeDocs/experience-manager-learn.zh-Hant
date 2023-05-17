---
title: 內容模型 — AEM Headless第一個教學課程
description: 了解如何在AEM中運用內容片段、建立片段模型及使用GraphQL端點。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---


# 內容模型

歡迎使用Adobe Experience Manager(AEM)中內容片段和GraphQL端點的教學課程章節。 我們將說明如何運用內容片段、建立片段模型，以及在AEM中使用GraphQL端點。

內容片段提供結構化方法，可跨頻道管理內容，提供彈性和可重複使用性。 在AEM中啟用內容片段可建立模組化內容，增強一致性和適應性。

首先，我們會引導您完成在AEM中啟用內容片段的作業，涵蓋必要的設定和設定，以便順暢整合。

接下來，我們將介紹如何建立定義結構和屬性的片段模型。 了解如何設計符合您的內容需求的模型並有效管理這些模型。

接著，我們將示範如何從模型建立內容片段，提供製作和發佈的逐步指引。

此外，我們也將探索定義AEM GraphQL端點。 GraphQL能有效地從AEM擷取資料，我們會設定和設定端點以公開所需的資料。 持續查詢可最佳化效能和快取。

在本教學課程中，我們將提供說明、程式碼範例和實用秘訣。 最後，您將具備啟用內容片段、建立片段模型、產生片段，以及定義AEM GraphQL端點和持續查詢的技能。 開始吧！

## 內容感知配置

1. 導覽至 __工具>配置瀏覽器__ 來建立無頭式體驗的設定。

   ![建立資料夾](./assets/1/create-configuration.png)

   提供 __標題__ 和 __名稱__，然後檢查 __GraphQL持續查詢__ 和 __內容片段模型__.


## 內容片段模型

1. 導覽至 __工具>內容片段模型__ 並選取資料夾，其名稱為在步驟1中建立的組態。

   ![模型資料夾](./assets/1/model-folder.png)

1. 在資料夾內，選取 __建立__ 為模型命名 __Teaser__. 將下列資料類型新增至 __Teaser__ 模型。

   | 資料類型 | 名稱 | 必要 | 選項 |
   |----------|------|----------|---------|
   | 內容參考 | 資產 | 是 | 您可以新增預設影像。 例如：/content/dam/wknd-headless/assets/AdobeStock_307513975.mp4 |
   | 單行文字 | 標題 | 是 |
   | 單行文字 | 標題前 | 否 |
   | 多行文字 | 說明 | 否 | 確認預設類型為RTF |
   | 列舉 | 樣式 | 是 | 呈現為下拉式清單。 選項為Hero -> hero and Featured ->精選 |

   ![預告模型](./assets/1/teaser-model.png)

1. 在資料夾內，建立名為 __選件__. 按一下「建立」，將模型命名為「選件」，然後新增下列資料類型：

   | 資料類型 | 名稱 | 必要 | 選項 |
   |----------|------|----------|---------|
   | 內容參考 | 資產 | 是 | 新增預設影像。 例如：`/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | 多行文字 | 說明 | 否 |  |
   | 多行文字 | 文章 | 否 |  |

   ![選件模型](./assets/1/offer-model.png)

1. 在資料夾內，建立名為 __影像清單__. 按一下「建立」，將模型命名為「影像清單」，然後新增下列資料類型：

   | 資料類型 | 名稱 | 必要 | 選項 |
   |----------|------|----------|---------|
   | 片段參考 | 清單項目 | 是 | 呈現為多個欄位。 允許的內容片段模型為選件。 |

   ![影像清單模型](./assets/1/imagelist-model.png)

## 內容片段

1. 現在導覽至「資產」，並為新網站建立資料夾。 按一下「建立」並為資料夾命名。

   ![新增資料夾](./assets/1/create-folder.png)

1. 建立資料夾後，選取資料夾並開啟其 __屬性__.
1. 在資料夾的 __雲端設定__ 頁簽，選擇配置 [已建立較早](#enable-content-fragments-and-graphql).

   ![資產資料夾AEM無頭雲端設定](./assets/1/cloud-config.png)

   按一下新資料夾並建立預告。 按一下 __建立__ 和 __內容片段__ ，然後選取 __Teaser__ 模型。 為模型命名 __英雄__ 按一下 __建立__.

   | 名稱 | 附註 |
   |----------|------|
   | 資產 | 保留為預設值，或選擇不同的資產（視訊或影像） |
   | 標題 | `Explore. Discover. Live.` |
   | 標題前 | `Join use for your next adventure.` |
   | 說明 | 留空 |
   | 樣式 | `Hero` |

   ![英雄片段](./assets/1/teaser-model.png)

## GraphQL 端點

1. 導覽至 __工具> GraphQL__

   ![AEM GraphQL](./assets/1/endpoint-nav.png)

1. 按一下 __建立__ 並為新端點命名，然後選擇新建立的配置。

   ![AEM無頭GraphQL端點](./assets/1/endpoint.png)

## GraphQL 持續性查詢

1. 讓我們測試新的端點。 導覽至 __工具> GraphQL查詢編輯器__ 並在視窗右上方的下拉式清單中選擇端點。

1. 在查詢編輯器中，建立一些不同的查詢。


   ```graphql
   {
       teaserList {
           items {
           title
           }
       }
   }
   ```

   您應該會取得包含已建立之單一片段的清單 [abos](#create-content).

   在本練習中，建立AEM無頭應用程式使用的完整查詢。 建立按路徑傳回單一預告的查詢。 在查詢編輯器中，輸入以下查詢：

   ```graphql
   query TeaserByPath($path: String!) {
   component: teaserByPath(_path: $path) {
       item {
       __typename
       _path
       _metadata {
           stringMetadata {
           name
           value
           }
       }
       title
       preTitle
       style
       asset {
           ... on MultimediaRef {
           __typename
           _authorUrl
           _publishUrl
           format
           }
           ... on ImageRef {
           __typename
           _authorUrl
           _publishUrl
           mimeType
           width
           height
           }
       }
       description {
           html
           plaintext
       }
       }
   }
   }
   ```

   在 __查詢變數__ 在底部輸入，輸入：

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > 您可能需要調整查詢變數 `path` 根據資料夾和片段名稱。


   執行查詢以接收先前建立之內容片段的結果。

1. 按一下 __儲存__  要保存（保存）查詢並為查詢命名 __Teaser__. 這可讓我們依應用程式中的名稱參考查詢。

## 後續步驟

恭喜！您已成功設定AEM as a Cloud Service，以允許建立內容片段和GraphQL端點。 您也已建立內容片段模型和內容片段，並定義了GraphQL端點及持續查詢。 您現在已準備好繼續前往下一個教學課程章節，了解如何建立AEM Headless React應用程式，以使用您在本章中建立的內容片段和GraphQL端點。

[下一章：AEM無頭API與React](./2-aem-headless-apis-and-react.md)